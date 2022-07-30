---
title: "O que são DIDs"
date: 2022-07-30T10:54:55-03:00
draft: false
tags: [identidade auto-soberana, privacidade, segurança, internet]
---

Em um post anterior eu expliquei o conceito de identidade auto-soberana, que pode ser resumido da seguinte forma:

> Identidade portátil vitalícia para qualquer pessoa, organização ou coisa que não dependa de nenhuma autoridade centralizada e nunca possa ser removida.

Neste post explicarei detalhes técnicos, com foco nos Decentralized Identifiers (DIDs).

---

Se você conhece duas pessoas, a forma mais natural de dizer quem é quem é usando seus nomes: José e Maria são pessoas diferentes. É para isso que serve qualquer sistema de identificação; para nos ajudar a distinguir uma coisa (ou pessoa) da outra.
Agora, estamos acostumados a encontrar pessoas com nomes iguais, e está tudo bem, pois nós humanos somos bons em ambiguidades -- diferente dos computadores.

Para evitar nomes repetidos em computadores, há duas opções: centralizar ou randomizar. Centralizar seria ter seu nome aprovado pelo governo. Por exemplo, se você quer nomear seu filho José, você pede ao governo, o qual nega o pedido, pois já existe um José, e agora você terá que ser bem mais criativo. Nomes de domínios, como google.com, funcionam assim, e a entidade que evita duplicações se chama "domain registrar".
Já a segunda opção, a randomização, seria nomear seu filho Wyzlczp: provavelmente ninguém tentou isso até agora (a não ser que você esteja na Polônia).

A grande vantagem dos nomes centralizados, além da garantia de legibilidade e de não haver repetidos, é que você pode criar métodos padronizados sobre eles. Por exemplo, você pode usar a URL http://google.com para resolver um documento HTML.
Por outro lado, nomes randomizados, como os [UUIDs], não se repetem, mas é impossível usá-los para obter um documento associado. Por exemplo, tente abrir a URL a seguir (e falhe): http://ecd05cb2-29cd-4442-b14a-8d52b944d742.

# Os DIDs

Um novo tipo de nome, inventado há pouco mais de 5 anos e chamado de Decentralized Identifier (DID), tem as duas coisas ao mesmo tempo: o DID é randomizado e pode ser usado para obter um documento associado a ele. E mais do que isso: ele é critpograficamente verificável, e não depende de nenhuma entidade centralizada, tal como o governo ou o Google.

Este é um exemplo de DID: `did:example:123456789abcdefghi`. Ele sempre começa com a string `did`, é seguido de um método (`example` nesse caso), e depois de uma string que depende do método, que geralmente é aleatória. Um método DID quer dizer "algumas regras para gerar a string aleatória, e para mexer com DID Documents".

No caso, um DID Document (abreviado como DDo) contém metadados associados a um DID. Por exemplo:

```
{
  "@context": "https://www.w3.org/ns/did/v1",
  "id": "did:example:123456789abcdefghi",
  "authentication": [{
    "id": "did:example:123456789abcdefghi#keys-1",
    "type": "RsaVerificationKey2018",
    "publicKeyPem": "-----BEGIN PUBLIC KEY...END PUBLIC KEY-----\r\n"
  }],
  "service": [{
    "id":"did:example:123456789abcdefghi#vcs",
    "type": "VerifiableCredentialService",
    "serviceEndpoint": "https://example.com/vc/"
  }]
}
```

O que importa aqui:

- @context: é uma anotação sobre o domínio ao qual esse documento pertence
- id: contém o próprio DID
- authentication: contém uma lista de chaves públicas
- service: contém uma lista de endpoints, ou seja links de acesso

## Calma, tô perdido

Vamos lá: um DID é a sua identidade digital. É como a chave primária de uma tabela -- só que você é a tabela. E você mesmo gerou essa chave primária.

## Mas e o DID Document?

O DID Document (DDo) está associado ao DID. Como ele contém chaves públicas e links de acesso, qualquer um que conhecer o meu DDo pode me enviar uma mensagem criptografada.

## Mas alguém conhece o meu DDo?

Bom, vamos assumir que Alice conhece o meu DID. Com isso ela consegue baixar o meu DDo, e usar ele para trocar mensagens seguras.

# Hora da buzzword

Agora que já entendemos DID e DDo, vamos entender como baixar um DDo. Basicamente, uma tecnologia excelente para armazenar DDos é a blockchain, já que ela permite armazenamento confiável e distribuído -- ou seja, sem depender de uma entidade centralizada. Então teremos o seguinte cenário:

1. Alice cria a sua identidade `did:example:zxy(alice)`
2. Alice cria o respectivo DID Document
3. Alice registra esse DDo na blockchain `example`

Agora, se Bob quer falar com Alice, e Bob sabe que o DID dela é `did:example:xyz(alice)`, ele consulta a blockchain `example`, e baixa o DDo da Alice. Ele pode pedir para Alice provar que é dona da chave pública naquele DDo (o que ela faria usando sua chave privada), e então criar um canal seguro de comunicação.