---
title: "Navigate faster up and down directories"
date: 2020-02-16T20:18:02-08:00
draft: false
categories: [dev]
tags: [bash, tools]
---

TL;DR you'll be able to do this:
{{< figure src="/img/fastnav.gif" width="600" title="" >}}

# Intro

If you work with software, you may appreciate the command line as a productivity tool.
This is the case for me, and some tools that make my developer life easier include: `vim`, `tmux`, and `zsh`.

One thing I like about zsh is the `..` shortcut to go back 1 directory. See:

```
$ pwd
~/dev/teach/blog
$ ..
$ pwd
~/dev/teach
```

It merely saves having to type the full `cd ..` command, but it is so handy!

(note: the actual `pwd` output does not abbreviate `/home/geovane` to `~`, but I do it here for simplicity)


# The gap

What I wanted, however, was a way to go back N directories. So that I could do this:


```
$ pwd
~/dev/teach/blog
$ .. 2
$ pwd
~/dev
```

So I created `go_up_n`, a bash function that does exactly that.
It is very simple:

```bash
go_up_n() {
    # if no parameters, navigate up only 1 directory and stop
    [ -z "$1" ] && cd .. && return 0

    # filter parameter $1, should contains only numbers
    [ "$( echo $1 | sed "s/[0-9]*//g" )" ] && echo "\$1 must be a number" && return 1 || n=$1

    dirs_up=
    for i in $(seq $1); do
        dirs_up+="../"
    done
    cd $dirs_up

    return 0
}
alias ..="go_up_n"
```

It just works! I actually created this in 2015, and it's been a life saver ever since.

# I want more

Alright, the new `.. 2` is cool, but what if I wanted to count backwards? 
Like, if I am too deeply nested, it may become difficult to count how many directories to go up.

For this I created the `go_down_n_from_home` function (aliased to `,,`), which works like this:

```
$ pwd
~/dev/teach/blog/resources/_gen/images
$ ,, 1
$ pwd
~/dev/
```

The code is also simple:

```bash
go_down_n_from_home() {
    [ "$( echo $1 | sed "s/[0-9]*//g" )" ] && echo "\$1 must be a number" && return 1 || n=$1

    # separate path components into a string array
    a=( $(echo $(pwd) | sed "s;\/; ;g") )

    ((n+=2)) # we are starting from /home/<your-user>/
    dir=$(echo "${a[@]:0:$n}" | sed "s; ;\/;g") # filter the array and put slashes back
    cd /$dir

    return 0
}
alias ,,="go_down_n_from_home"

```

# Enjoy!

If you found this useful, you may want to add enhanced `..` and `,,` to your `.bashrc` (or `.zshrc`, etc.).
A commented version of them is available [at this github gist](https://gist.github.com/geonnave/335fa20e82a19d9fb177d65e27828fe2), and an automated way to add it is:

```
$ wget https://gist.githubusercontent.com/geonnave/335fa20e82a19d9fb177d65e27828fe2/raw/709a98cdb6b6b514b66c31a6cab4af41f99a3793/fastnav.sh && chmod +x fastnav.sh && echo "source $(pwd)/fastnav.sh" >> $HOME/.bashrc
```

(note: if you are using `zsh`, change `.bashrc` to `.zsh`)
