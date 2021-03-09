# Graphql

<!-- TOC -->

- [Graphql](#graphql)
  - [Definição](#definição)
  - [Vantagens em relação ao REST](#vantagens-em-relação-ao-rest)
  - [Tipos](#tipos)
  - [Exemplo](#exemplo)
    - [Filtros](#filtros)
  - [Resolvers](#resolvers)
  - [Mutations](#mutations)

<!-- /TOC -->

## Definição

O Graphql é uma **query language** que permite ao cliente indicar ao servidor exatamente como ele quer os dados.

## Vantagens em relação ao REST

O Graphql surgiu com o propósito de facilitar a maneira como se buscam dados. Diferentemente do padrão REST, no Graphl existe apenas uma rota: /graphql

Vantagens:

- Não há necessidade de versionar a api (api/v1, api/v2)
- Obter apenas os dados necessários, evitando **overfetching**
- Buscar vários dados na mesma query, sem precisar acessar mais de uma rota

## Estrutura

No Graphql, tudo é um **tipo**(Type) que define a maneira como os dados estão estruturados e que tipo de dados cada coluna de uma tabela, por exemplo, possui. O conjuntos desses tipos formam o Schema.

Game Type (relacionado com plataformas(type Platform), desenvolvedores(type Developer) e categorias(type Category)):

```graphql
type Developer {
  id: ID!
  name: String!
}

type Platform {
  id: ID!
  name: String!
}

type Game {
  id: ID!
  name: String!
  slug: String!
  short_description: String!
  description: String!
  price: Float!
  release_date: Date
  categories: [Category]
  developers: [Developer]
  platforms: [Platform]
}
```

## Exemplo

Consideremos que os types acimas estão estruturados para que um Ecommerce de games possa listar todos os seus jogos

Query de jogos:

```graphql
query getGames {
  games {
    id
    name
    slug
    short_description
    description
    price
    release_date
    categories {
      id
      name
    }
    developers {
      id
      name
    }
    platforms {
      id
      name
    }
  }
}
```

### Filtros

Agora digamos que tenhamos uma página de listagem dos 15 primeiros jogos que estão de graça, para isso precisamos filtrar todos os jogos com o campo **price** igual a 0

Filtro de jogos de graça:

```graphql
query getGames($limit: Int, $where: JSON) {
  games(where: $where, limit: $limit) {
    id
    name
    slug
    short_description
    description
    price
    release_date
    categories {
      id
      name
    }
    developers {
      id
      name
    }
    platforms {
      id
      name
    }
  }
}
```

Assim podemos passar como **Query Variables** um objeto da seguinte maneira:

```json
{
  "limit": 15,
  "where": {
    "price": 0
  }
}
```

e os 15 primeiros jogos gratuitos serão buscados, e além do preço podemos passar outros dados para o **where**

```json
{
  "limit": 15,
  "where": {
    "price": 0,
    "name": "Counter-Strike: Global Offensive"
  }
}
```
