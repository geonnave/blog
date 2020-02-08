---
title: "Identidade Auto Soberana"
date: 2020-01-14T12:59:02-08:00
draft: true
---

Existe um problema hoje na internet que, se pararmos pra pensar, é bem absurdo que ainda não esteja resolvido: **Como identificar pessoas**?

{{< figure src="/img/internet-dog.jpg" width="500" title="\"na internet, ninguém sabe que você é um cão\"" >}}

Pense sobre o que acontece quando você faz login no email, twitter, facebook, etc.: você usa um usuário-e-senha diferente (assim espero) para cada site. Em cada site, essa é sua identidade: tom@bombadil.com e 12345678. Pode ser que o site solicite seu nome, RG e CPF, mas a não ser que seja um banco ou algo do tipo, geralmente é fácil fornecer valores falsos.

Claro, por um lado a anonimicidade da internet é ótima. Podemos, teoricamente, acessar qualquer conteúdo sem ser rastreados ou ter nossa identidade real exposta. Por outro lado, quando realmente precisamos nos identificar, usamos mecanismos arcaicos, que acarretam em problemas como:

* Preocupação em não repetir senhas, o que nos leva a usar gerenciadores de senhas.
* Repetição dos mesmos dados em dezenas de sites, o que fere nossa privacidade e tem péssima usabilidade.
* Logins sociais, tais como "login com google", permitem que o provedor de identidade (por exemplo, o google) rastreie nossos logins.

Como se já não bastasse, há ainda um item mais crítico: **Você não é dono da sua identidade na internet!** Quem é o dono é o website onde você se registrou. Imagine se você tivesse um RG digital e ele ficasse no servidor do governo; toda vez que você precisasse usá-lo, você teria que pedir ao governo. Se por qualquer razão esse governo, ou qualquer website, parar de manter o servidor funcionando, a sua identidade vai embora!

# Mas, por que?

<!-- Podemos entender o por quê de estarmos usando um modelo simplório de identidade digital com um pouco de história. -->

Isso aconteceu, principalmente, porque a internet foi criada para conectar computadores, e não pessoas. Endereços MAC, IP, nomes de domínio, todos esses sistemas são excelentes para nomear máquinas no contexto de uma rede. Não criamos um equivalente para pessoas.

Então o problema começou quando as pessoas começaram a ter projetos e dados que elas queriam chamar de "meu", em computadores que eram compartilhados. Eis então que a solução mais simples e rápida, e talvez uma das maiores gambiarras da internet, foi criada: usuário e senha.

Como já temos algumas décadas desse paradigma instalado, e de alguma forma funcionando, simplesmente estamos acostumados com ele. Mas você já imaginou se houvesse outra solução? Se por exemplo, você pudesse provar, de forma **totalmente digital**, que você é realmente o "Tom Bombadil" com seu RG, na hora de abrir a conta em um banco (não vale tirar foto do RG físico :p)? E sem que o "RG digital" precise estar no servidor de ninguém?

# Identidade Auto-Soberana

Uma proposta para resolver esse problema é conhecida como **identidade auto-soberana** (do inglês, _self-sovereign identity_). Bastante detalhada neste artigo do Christopher Allen [1], a ideia é que você deve ser o dono da sua identidade digital e carregar ela com você, basicamente do jeito que você já faz com sua carteira física. O funcionamento, inclusive, seria bem parecido: você teria uma carteira digital, por exemplo um app no smartphone, onde você poderia guardar todos os seus "documentos". Quando um site ou serviço precisar da sua identificação, você apresenta o documento necessário.

O Christopher Allen, que foi um dos criadores do TLS, apresenta 10 princípios da Identidade Auto-Soberana, que eu resumo abaixo, com comentários em itálico:

* Existência: Usuários devem existir de forma independente. _a sua identidade digital depende apenas do seu próprio "eu"; a existência ou não de outras entidades é insignificante_.
* Controle: Usuários devem controlar suas identidades. _o usuário decide o que faz com sua identidade; outros podem endossar o João, dizendo que ele realmente é o João, mas só o João controla o que faz com essa informação_.
* Acesso: Usuários devem ter acesso aos seus próprios dados. _ninguém deve armazenar dados sobre o usuário sem que ele saiba e tenha acesso_.
* Transparência: Sistemas e algoritmos devem ser transparentes. _sistemas e algoritmos que estabelecem redes de identidades devem ser abertos_.
* Persistência: Identidades devem ser longevas. _uma identidade deve durar até enquanto o usuário quiser; mesmo que o usuário modifique parâmetros de segurança, a identidade deve persistir; isso não invalida o direito-de-ser-esquecido_.
* Portabilidade: Informação e serviços sobre a identidade devem ser portáveis. _o usuário deve poder levar sua identidade para outro lugar (por exemplo outra carteira digital)_.
* Consentimento: Usuários devem concordar com o uso da sua identidade. _o compartilhamento de identificadores de uma pessoa requer aprovação da mesma_.
* Minimização: Divulgação de atributos deve ser minimizada. _por exemplo, se um serviço requer apenas prova de maioridade, um usuário não deveria precisar mostrar a data exata de nascimento_.
* Proteção: Os direitos do usuário devem ser protegidos. _quando houver conflito em envolvendo a identidade e um sistema de identificação, o usuário deve ter prioridade_.

Indiferente das reflexões filosóficas (que são importantes), na prática você precisa ao menos saber que um modelo centrado no indivíduo, e com privacidade e segurança elevados, é possível.

# Mas como?

Em resumo, dois novos padrões, desenvolvidos ao longo dos últimos 5 anos, estão sendo recomendados pelo Consórcio da World Wide Web (W3C), para habilitar identidade auto-soberana para todos. Se você não conhece o W3C, trata-se da organização que gerencia padrões web como HTTP, HTML, e outros.

Bem, o primeiro padrão para realização da identidade auto-soberana é o identificador decentralizado (do inglês _Decentralized Identifier_), abreviado como DID. Um DID contém chaves públicas e uma lista de serviços, o que permite a criação de canais criptograficamente seguros para comunicação.

Já o segundo, denominado Credenciais Verificáveis (do inglês _Verifiable Credentials_), ou VCs, permite o estabelecimento de confiança em termos humanos. Ou seja, você pode ter uma VC que prova que você tem os dados nome=tom e sobrenome=bombadil. Agora, para outras pessoas acreditarem nessa sua "reinvindicação", basta que o governo assine embaixo da sua VC. 
<!-- Na prática você terá várias VCs, e poderá escolher quais delas, ou quais partes delas, você irá compartilhar com cada serviço online. -->

Se você é da área técnica e está perdido/a, ou está captando que já existem algumas propostas parecidas, relaxa, em um post futuro irei detalhar mais os DIDs e as VCs.

# Sério que é só isso?

Bem, não. Mas pelo menos eu consegui escrever mais de 5000 caracteres sem apelar para _buzzwords_ 😃. Falta mais um ingrediente no sistema: _Blockchain_.

Mesmo que, lá em 1970, houvesse um consenso (trocadilho não-intencional) geral sobre os problemas do modelo usuário-e-senha, seria difícil resolver. O principal motivo é que, até 2008, qualquer sistema de identidade necessariamente precisaria ser gerenciado por um sistema centralizado. Por exemplo, se o governo quisesse assinar a sua credencial, ele teria que colocar a chave pública dele em um diretório centralizado, para que outros pudessem verificar.

Como Blockchain é uma solução técnica para o problema de armazenamento decentralizado e confiável, ela se torna uma peça importante para habilitar a identidade auto-soberana. Em particular, entidades confiáveis podem colocar sua chave pública em uma Blockchain, o que será validado por todos os nós da rede.

# Agora sim, conclusão

Essa postagem foi uma visão geral sobre o conceito de identidade auto-soberana. Espero que você tenha percebido os problemas da identidade hoje na Internet, e como a identidade de cada indivíduo pertence apenas a si próprio. Também espero ter despertado alguma curiosidade sobre os novos padrões, Decentralized Identifiers e Verifiable Credentials, que pretendo explicar melhor em postagens futuras.

Eu realmente estou muito animado com o assunto, especialmente por haver uma comunidade enorme trabalhando para trazer à realidade. Quem sabe em poucos anos teremos carteiras completamente digitais, e problemas de vazamentos de dados sensíveis serão coisa do passado.




<!-- 
Ok, até aqui blz, vc vai lá e coloca nome:rafael, formação:engenheiro. Mas como que eu confio que isso tá certo? E se vc colocar título:presidenteDoBrasil, como eu verifico se isso aí é verdadeiro? Bom basicamente o Sovrin se propõe a resolver isso, ao criar
 (1) um banco de dados distribuído (aka blockchain) onde "instituições confiáveis" podem atestar a validade dos atributos nome:rafael, formação:engenheiro
 (2) uma estrutura de governança para agregar várias dessas "instituições confiáveis"

No caso os links que eu mandei acima, quer dizer que a W3C, a entidade que padronizou HTML, HTTP, etc., está trabalhando para padronizar novas tecnologias para permitir a realização do item (1). Assim como um dia inventaram URLs, estão inventando um negócio chamado DID para servir de id único e criptograficamente verificável. Assim como um dia inventaram o HTML, estão inventando um tal Verifiable Credential, que é tipo um objeto com seus dados (e.g., nome, formação) criptograficamente assinado e que usa os tais DID.

Tudo isso com o objetivo de criar uma camada nova de identidade na internet: no futuro vc vai ter uma digital wallet com atributos assinados (verifiable credentials) e identificáveis (com DID). Como vc será dono dessa carteira, e vc controlará quem recebe esses dados, vc será auto-soberano sobre a sua identidade digital.
 -->


[1] http://www.lifewithalacrity.com/2016/04/the-path-to-self-soverereign-identity.html
