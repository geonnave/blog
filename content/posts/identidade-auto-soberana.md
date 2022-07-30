---
title: "Identidade Auto Soberana"
date: 2020-02-08T10:35:00-00:00
draft: false
tags: [identidade auto-soberana, privacidade, segurança, internet]
---

A Internet tem um problema...

{{< figure src="/img/internet-dog.jpg" width="500" title="\"na internet, ninguém sabe que você é um cão\" (fonte: https://en.wikipedia.org/wiki/On_the_Internet,_nobody_knows_you%27re_a_dog)" >}}

É sério, e é bem absurdo se pararmos para pensar, mas em pleno 2020 ainda não temos um bom jeito de **identificar pessoas** na Internet!

Pense sobre o que acontece quando você faz login no email, twitter, facebook, etc.: você usa um usuário-e-senha diferente (assim espero) para cada site. Em cada site, essa é sua identidade: tom@bombadil.com e 12345678. Pode ser que o site solicite seu nome, RG e CPF, mas a não ser que seja um banco ou algo do tipo, geralmente é fácil fornecer valores falsos.

Claro, por um lado a anonimicidade da internet é ótima. Podemos, teoricamente, acessar qualquer conteúdo sem ser rastreados ou ter nossa identidade real exposta. Por outro lado, quando realmente precisamos nos identificar, usamos mecanismos arcaicos, que acarretam em problemas como:

* Preocupação em não repetir senhas, o que nos leva a usar gerenciadores de senhas.
* Duplicação dos mesmos dados em dezenas de sites, o que fere nossa privacidade e tem péssima usabilidade.
* Logins sociais, tais como "login com google", que permitem que o provedor de identidade (o google, por exemplo) rastreie nossos logins.

Como se já não bastasse, há ainda um item mais crítico: **Você não é dono da sua identidade na internet!** Quem é o dono é o website onde você se registrou. Imagine se você tivesse um RG digital e ele ficasse no servidor do governo; toda vez que você precisasse usá-lo, você teria que pedir ao governo. Além disso permitir que você seja rastreado, se por qualquer razão esse governo parar de manter o servidor funcionando, a sua identidade vai embora! Se o exemplo parece absurdo com entidades públicas, é tão ou mais absurdo com empresas privadas.

# Mas, por que?

Isso aconteceu, principalmente, porque a internet foi criada para conectar computadores, e não pessoas. Endereços MAC, IP, nomes de domínio, todos esses sistemas são excelentes para nomear máquinas no contexto de uma rede. No entanto, não criamos um equivalente para pessoas.

Então o problema começou quando as pessoas começaram a ter projetos e dados que elas queriam chamar de "meu", salvos em computadores que eram compartilhados. Eis então que a solução mais simples e rápida, e talvez uma das maiores gambiarras da computação, foi criada: usuário e senha.

Como já temos algumas décadas desse paradigma instalado, e de alguma forma funcionando, simplesmente estamos acostumados com ele. Mas você já imaginou se houvesse outra solução? Se por exemplo, você pudesse provar, de forma totalmente **digital e independente**, que você é realmente o "Tom Bombadil"? Isso seria útil, por exemplo, na hora de abrir a conta em um banco digital (não vale tirar foto do RG físico :p). E claro, muito importante, isso deve ser possível sem que o "RG digital" esteja no servidor de ninguém, portanto evitando que seu uso seja rastreado.

# Identidade Auto-Soberana

Uma proposta para resolver esse problema é conhecida como **identidade auto-soberana** (do inglês, _self-sovereign identity_). Bastante detalhada em um artigo do Christopher Allen (em inglês) [1], a ideia é que você deve ser o dono da sua identidade digital e carregar ela com você, basicamente do jeito que você já faz com sua carteira física. O funcionamento, inclusive, seria bem parecido: você usa um app de **"carteira digital"** no smartphone para guardar todos os seus documentos digitais. Quando um site ou serviço precisar da sua identificação, você apresenta, com segurança criptográfica, o documento digital necessário.

O Christopher Allen, que foi um dos criadores do TLS, propôs 10 princípios da Identidade Auto-Soberana, que eu resumo abaixo, com comentários em itálico:

* Existência: Usuários devem existir de forma independente. _a sua identidade digital depende apenas do seu próprio "eu"; a existência ou não de outras entidades é irrelevante para sua identidade existir_.
* Controle: Usuários devem controlar suas identidades. _o usuário decide o que faz com sua identidade; outros podem endossar o João, dizendo que ele realmente é o João, mas só o João controla o que faz com essa informação_.
* Acesso: Usuários devem ter acesso aos seus próprios dados. _ninguém deve armazenar dados sobre o usuário sem que ele saiba e tenha acesso_.
* Transparência: Sistemas e algoritmos devem ser transparentes. _os mecanismos que estabelecem identidades devem ser abertos_.
* Persistência: Identidades devem ser longevas. _uma identidade deve durar até quando o usuário quiser; mesmo que o usuário modifique parâmetros de segurança, a identidade deve persistir; isso não invalida o direito-de-ser-esquecido_.
* Portabilidade: Informação e serviços sobre a identidade devem ser portáveis. _o usuário deve poder levar sua identidade para outro lugar (por exemplo, outra carteira digital)_.
* Consentimento: Usuários devem concordar com o uso da sua identidade. _o compartilhamento de identificadores de uma pessoa requer aprovação da mesma_.
* Minimização: Divulgação de atributos deve ser minimizada. _por exemplo, se um serviço requer apenas prova de maioridade, um usuário não deveria precisar mostrar a data exata de nascimento_.
* Proteção: Os direitos do usuário devem ser protegidos. _quando houver conflito em envolvendo a identidade e um sistema de identificação, o usuário deve ter prioridade_.

Bom, mas indiferente das reflexões filosóficas, que são importantes no contexto geral, na prática você precisa ao menos saber que um modelo centrado no indivíduo, e com privacidade e segurança elevados, não só é desejado: é possível.

# Mas como?

Em resumo, dois novos padrões, desenvolvidos ao longo dos últimos 5 anos, estão sendo recomendados pelo Consórcio da World Wide Web (W3C) para habilitar identidade auto-soberana para todos. Se você não conhece o W3C, trata-se da organização que gerencia padrões na Web, tais como HTTP, HTML, e outros.

Bem, o primeiro padrão para realização da identidade auto-soberana é o Identificador Decentralizado (do inglês _Decentralized Identifier_), abreviado como DID [2]. Um DID contém chaves públicas e uma lista de serviços, e ele permite a criação de canais criptograficamente seguros para comunicação.

Já o segundo padrão, denominado Credenciais Verificáveis (do inglês _Verifiable Credentials_), ou VCs, permite a criação de "documentos digitais" [3]. Diferente do DID, que é mais baixo nível, uma VC pode ser usada, por exemplo, para provar que você tem os atributos nome=tom e sobrenome=bombadil. Agora, para que outras pessoas possam acreditar nessa sua "reinvindicação" (o termo usado em inglês é _claim_), basta que uma entidade confiável, tal como o governo, assine embaixo da sua VC. 
<!-- Na prática você terá várias VCs, e poderá escolher quais delas, ou quais partes delas, você irá compartilhar com cada serviço online. -->

Se você é da área técnica e está perdido/a, curioso, ou está captando que já existem algumas propostas _parecidas_ com as VCs, relaxa, em um post futuro irei detalhar mais os DIDs e as VCs.

# Sério que é só isso?

Bem, não. Mas pelo menos eu consegui escrever 80% do texto sem apelar para _buzzwords_ 😃. Falta mais um ingrediente no sistema: _Blockchain_.

Mesmo que, lá em 1970, houvesse um consenso (trocadilho sutil não-intencional) geral sobre os problemas do modelo usuário-e-senha, seria difícil resolver. O principal motivo é que, até 2008, qualquer sistema de identidade necessariamente precisaria ser gerenciado por um sistema centralizado. Por exemplo, se o governo quisesse assinar a sua credencial, ele teria que colocar a chave pública dele em um diretório centralizado, para que outros pudessem verificar.

Como Blockchain é uma solução técnica para o problema de armazenamento decentralizado e confiável, ela se torna uma peça importante para habilitar a identidade auto-soberana. Em particular, entidades confiáveis podem colocar sua chave pública em uma Blockchain, permitindo assim uma verificação distribuída por outros nós da rede.

# Agora sim, conclusão

Essa postagem foi uma visão geral sobre o conceito de identidade auto-soberana. Espero que você tenha percebido os problemas da identidade hoje na Internet, e que a identidade de cada pessoa pertence apenas a ela mesma. Também espero ter despertado alguma curiosidade sobre o assunto, em especial os novos novos padrões, Decentralized Identifiers e Verifiable Credentials, que pretendo explicar melhor em postagens futuras.

Particularmente, estou muito animado com o assunto, especialmente por haver uma comunidade enorme trabalhando para trazer identidade auto-soberana à realidade [4][5]. Quem sabe em poucos anos teremos carteiras completamente digitais, e o conceito de login e senha será transformado em coisa do passado. 😃

# Referências

[1] http://www.lifewithalacrity.com/2016/04/the-path-to-self-soverereign-identity.html

[2] https://www.w3.org/TR/did-core/

[3] https://www.w3.org/TR/vc-data-model/

[4] https://internetidentityworkshop.com/

[5] https://www.weboftrust.info/
