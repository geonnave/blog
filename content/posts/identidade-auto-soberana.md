---
title: "Identidade Auto Soberana"
date: 2020-01-14T12:59:02-08:00
draft: true
---

Existe um problema hoje na internet: como identificar pessoas? Pensa no login que você faz no email, twitter, etc.: você cria um par de usuário e senha para cada site, e essa sua "identidade" fica lá fechada na nuvem. Você não é dono da sua identidade na internet!

{{< figure src="/img/internet-dog.jpg" width="500" title="\"na internet, ninguém sabe que você é um cão\"" >}}

Isso aconteceu, principalmente, porque a internet foi criada para conectar computadores, e não pessoas. Aí quando as pessoas começaram a ter suas próprias "contas", inventaram uma das maiores gambiarras da história da computação: usuário e senha...

Agora, apesar dos pesares, há esperança. Uma nova alternativa que está sendo proposta é a **identidade auto-soberana** (do inglês, _self-sovereign identity_). Basicamente essa proposta diz que você deve ser **dono** da sua identidade digital, e carregar ela com **você**, e só mostrar as partes que você escolher, para quem você escolher. Por exemplo, ao fazer login em um serviço, você pode deixar que ele saiba o seu nome e que você tem mais de 18 anos.
Pra fazer isso, uma das propostas é você ter uma digital wallet, que pode ser, por exemplo, um app android no qual você guarda a(s) sua(s) identidade(s).

Ok, até aqui blz, vc vai lá e coloca nome:rafael, formação:engenheiro. Mas como que eu confio que isso tá certo? E se vc colocar título:presidenteDoBrasil, como eu verifico se isso aí é verdadeiro? Bom basicamente o Sovrin se propõe a resolver isso, ao criar
 (1) um banco de dados distribuído (aka blockchain) onde "instituições confiáveis" podem atestar a validade dos atributos nome:rafael, formação:engenheiro
 (2) uma estrutura de governança para agregar várias dessas "instituições confiáveis"

No caso os links que eu mandei acima, quer dizer que a W3C, a entidade que padronizou HTML, HTTP, etc., está trabalhando para padronizar novas tecnologias para permitir a realização do item (1). Assim como um dia inventaram URLs, estão inventando um negócio chamado DID para servir de id único e criptograficamente verificável. Assim como um dia inventaram o HTML, estão inventando um tal Verifiable Credential, que é tipo um objeto com seus dados (e.g., nome, formação) criptograficamente assinado e que usa os tais DID.

Tudo isso com o objetivo de criar uma camada nova de identidade na internet: no futuro vc vai ter uma digital wallet com atributos assinados (verifiable credentials) e identificáveis (com DID). Como vc será dono dessa carteira, e vc controlará quem recebe esses dados, vc será auto-soberano sobre a sua identidade digital.
