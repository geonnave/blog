---
title: "How gen_server works: implementing a subset of it in Elixir"
date: 2016-04-22T21:27:45-03:00
draft: false
---

[Elixir](http://elixir-lang.org/), the language built to run on
[Erlang](http://erlang.org/) VM, brings all the awesome and solid parts of
Erlang, whilst it also adds new cool features. My goal with this writing is to
use Elixir to explore the inner workings of one of the decades-old Erlang/OTP
behavior: the **gen_server** (which, in Elixir land, is called **GenServer**).

First, a bit of context. I’ve read Joe Armstrong’s PhD
[thesis](http://erlang.org/download/armstrong_thesis_2003.pdf) “Making reliable
distributed systems in the presence of software errors” where he makes a
magnific explanation of a generic server for Erlang using processes, higher
order functions and pattern-matching. This inspired me to replicate it in Elixir
— which was surprisingly quick and simple once I understood how it worked. And
now I’m here to share it :)

*****

So, **GenServer**. There are some explanations of [how to
use](http://elixir-lang.org/getting-started/mix-otp/genserver.html) it in the
Internet and in books. Here I want to invite you to step back for a moment and
think about the **GenServer** abstraction. Here is a picture I picked from the
Erlang docs to help you:

![](https://cdn-images-1.medium.com/max/1040/1*3Lwd9A0qL8NVbWi5Ecm63Q.gif)
<span class="figcaption_hack">[http://erlang.org/doc/design_principles/clientserver.gif](http://erlang.org/doc/design_principles/clientserver.gif)</span>

So, if you read about or already tried GenServer, you shall agree with me that
it brings a generic abstraction for a server*ish* behavior implementation. From
now on we will analyze the requirements for building such an abstraction and
effectively build a simple, but functional, version of it.

*****

First of all, as in Erlang/Elixir everything is a process, we are going to run
the server (which I’m going to call **MyGenServer** from now on) in it’s own
process.

Second, **MyGenServer** must provide the programmer a very simple and
parameterizable API.

Third, It must handle concurrency stuff internally, *so that the programmer
using it need not to worry about concurrency*.

The silver bullet about making a server generic is the use of high order
functions together with pattern-matching. **MyGenServer** must agnostically run
a function **f** like this:

{reply, new_state} = **f**.(query, state)

The code above runs inside **MyGenServer**. **f** is a high order function
implemented by the programmer which uses **MyGenServer**, thus **f** is
decoupled from the server behavior. **query** is an *atom* or *tuple* which will
match against a pattern.

For a deeper understanding, nothing better than the whole code (read the
comments as they were part of this blog post):

```elixir
defmodule MyGenServer do

  @doc """
  Spawns a new MyGenServer process which can be further referenced by `name`Spawns a new MyGenServer process which can be further referenced by `name`
  `name` is normally an atom and will be used as an alias of a process' pid
  `f` is a function that the programmer using MyGenServer will have to implement
  `state` is the initial state of the MyGenServer process
  """
  def start(name, f, state) do
    fn -> loop(name, f, state) end
    |> spawn
    |> Process.register(name)
  end

  @doc """
  Stop the MyGenServer process execution
  """
  def stop(name), do: send name, :stop

  @doc """
  Communicate with the MyGenServer process identified by `name`
  The function `loop` implements the other side of this communication.
  """
  def call(name, query) do
    send name, {self(), query} #a

    receive do                 #b
      {^name, reply} -> reply
    end
  end

  @doc """
  The "main" routine of the MyGenServer
  Receive a request (a message sent from `call` function), process
  it and send back a response.
  """
  def loop(name, f, state) do
    receive do                             #c
      :stop ->
        nil
      {pid, query} ->
        {reply, state1} = f.(query, state) # this is important!
        send pid, {name, reply}            #d
        loop(name, f, state1)              # tail-call to wait on #c again!
    end
  end

end
```

First, regarding the *send* and *receive* stuff. This is concurrent code, which
tends to be difficult to understand. Use the #[**abcd**] comments to orient
yourself. Message passing happens in this order: #**a** -> #**c** -> #**d** ->
#**b**. I hope this is clear, because this is simply beautiful once you
understand :)

Notes: **(1)** the *receive* (#**b**) in the **call** function will wait for the
*send* (#**c**) in **loop**. **(2)** when *receive* (#**c**) triggers in
**loop**, it has the **name, f,** and **state** inputs from the last time
**loop** was called.

Stop a moment to understand the code, it is not trivial (many interactive moving
parts!). Use the comments in your favor! Also, do not forget to pay attention to
the line marked as “important” ;)

Now, this is from Joe’s thesis:

> difficult modules should be few and written by experts (…) easy modules should
> be many and written by less experienced programmers.

We saw a difficult but generic module. Now let’s see an easier, but specific one
that uses it.

*****

As I wrote this based on the example on Joe’s thesis, I’m using the the same
sample implementation: a telecom’s Home Location Register (HLR) — whose role is
to record the location of a phone user. Thus, we will specialize MyGenServer to
store/query a user’s name and its location. Check the code:

```elixir
defmodule HLR do
  @moduledoc """
  HLR (Home Location Register) - record the location of a phone user
  Use `i_am_at/2` to store the location of a user and `find/1` to query it
  """

  @doc """
  Start a new MyGenServer with `name` = `:hlr`
  the `f` inside MyGenServer is going to be handle_call/2
  `state` is initialized to an empty map
  """
  def start, do: MyGenServer.start(:hlr, &handle_call/2, %{})

  def stop, do: MyGenServer.stop(:hlr)

  def i_am_at(who, where) do
    MyGenServer.call(:hlr, {:i_am_at, who, where})
  end

  def find(who) do
    MyGenServer.call(:hlr, {:find, who})
  end

  @doc """
  The specific implementation of `f`
  """
  def handle_call({:i_am_at, who, where}, map) do
    {:ok, Map.put(map, who, where)}
  end
  def handle_call({:find, who}, map) do
    {map[who], map}
  end

end
```

If you are familiar with using GenServer, the code and comments should be
sufficient to understand how it works. If not, please check
[this](http://elixir-lang.org/getting-started/mix-otp/genserver.html).

Note that there are no *send*/*receive* instructions in this code! Only
sequential instructions on the surface — the underlying, hidden system, is the
one using inter-process message-passing! This is the easy, safe and happy path
we shall follow on most of our code :)

*****

Now to an example of usage in *iex* console:

```
    iex(1)> HLR.start
    true
    iex(2)> HLR.i_am_at "frodo", "shire"
    :ok
    iex(3)> HLR.i_am_at "gandalf", "n/a (roaming)"    
    :ok
    iex(4)> HLR.find "frodo"
    "shire"
```

It works :)

*****

Today we saw how to build **MyGenServer**, a simpler implementation of the
**GenServer** behavior. Check the [full code on
github](https://github.com/geonnave/my_gen_server).

It was possible to see the difference between concurrent-mixed code and
sequential-pure code. Also, we saw how functional programming abstractions like
message-passing, high order functions and pattern matching can be combined to
build even more powerful concepts.

Many important features like fault-tolerance were not addressed. If you are
really curious, I recommend reading chapter 4 (if not all of them) of Joe’s
[thesis](http://erlang.org/download/armstrong_thesis_2003.pdf).

It is important to note that this is simply an experiment. The official Elixir
**GenServer** module [actually delegates stuff to
Erlang](https://github.com/elixir-lang/elixir/blob/v1.2.4/lib/elixir/lib/gen_server.ex#L561)
(curious people check
[here](https://github.com/erlang/otp/blob/maint/lib/stdlib/src/gen_server.erl#L192)
and
[here](https://github.com/erlang/otp/blob/maint/lib/stdlib/src/gen.erl#L143)).

Finally, I hope this has helped you to have an idea of what happens behind the
scenes of **handle_call**, **handle_cast**, etc.

Comments are welcome!
