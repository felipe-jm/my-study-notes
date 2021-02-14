# Graphql

<!-- TOC -->

- [Graphql](#graphql)
  - [Definição](#definição)
  - [Vantagens em relação ao REST](#vantagens-em-relação-ao-rest)
  - [Exemplo](#exemplo)

<!-- /TOC -->

## Definição

O Graphql é uma **query language** que permite o cliente indicar ao servidor exatamente como eles quer os dados.

## Vantagens em relação ao REST

O Graphql surgiu com o propósito de facilitar a maneira como se buscam dados. Diferentemente do padrão REST, no Graphl existe apenas uma rota: /graphql

Vantagens:

- Não há necessidade de versionar a api (api/v1, api/v2)
- Obter apenas os dados necessários, evitando **overfetching**

## Exemplo

Considerando um cenário de um ecommerce de games, temos nosso jogos e cada jogo possui algumas características como: name, price, genre, publisher, developer, platforms.

Query de jogos:

```ts
{
  query getGames {
    game: {
      name,
      price,
      genre,
      publisher,
      developer,
      platforms
    }
  }
}
```
