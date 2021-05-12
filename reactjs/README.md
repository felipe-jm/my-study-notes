# ReactJS

<!-- TOC -->

- [ReactJS](#reactjs)
  - [Definição](#definição)
  - [Casos de uso](#casos-de-uso)
  - [Componentes](#components)
  - [Propriedades](#propriedades)
  - [Estados](#estados)
  - [Imutabilidade](#imutabilidade)
  - [Renderização](#renderização)
    - [Igualdade Referencial](#igualdade-referencial)
    - [Memo](#memo)
    - [useMemo](#usememo)
    - [useCallback](#useCallback)
    - [Bundle](#bundle)
    - [Code Splitting e Lazy Loading](#code-splitting-e-lazy-loading)
    - [Boas práticas](#boas-práticas)
    - [Virtualização](#virutalização)

<!-- /TOC -->

## Definição

ReactJS é uma biblioteca para criar interfaces de usuário. ReactJS utiliza uma junção de html + javascript para renderizar suas interfaces, o chamado JSX.

```jsx
export function App() {
  return <h1>Hey that's jsx</h1>;
}
```

## Casos de uso

O ReactJS é amplamente utilizado no mercado. Netflix, Uber, Twitch, Facebook, Instagram...

## Componentes

Tudo no React são components. Podem ser vistos principalmente como pedaços de códigos reaproveitáveis(botões, cards..).

```jsx
export function App() {
  return <h1>I'm a component</h1>;
}
```

## Propriedades

Propriedades são dados que podem ser passados de um componente pai para um componente filho.

```jsx
import Son from "components/Son";

export function Father() {
  return <Son />;
}

export function Son({ title }) {
  return <>{title}</>;
}
```

## Estados e Imutabilidade

Da maneira que o React funciona, se uma variável é alterada, a interface não é renderiza novamente, não acontecendo nenhuma mudança visual:

```jsx
export function Counter() {
  let counter = 0;

  function increment(counter) {
    counter += 1;
  }

  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={increment}>Click me</button>
    </div>
  );
}
```

Click! Click! Click! Nada acontece!

O React funciona assim porque se ele observasse todas as variáveis o tempo todo, verificar se elas mudaram para aí sim renderizar novamente a interface, a aplicação poderia começar a ficar lenta de acordo com o número de variáveis. E aí que surgem os estados.

Estados são maneiras de criar váriaveis que o React irá monitorar e, quando esse estado for alterado, ele irá renderizar a interface novamente.

```jsx
import { useState } from "react";

export function Counter() {
  const [counter, setCounter] = useState(0);

  function increment() {
    setCounter(counter + 1);
  }

  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={increment}>Click me</button>
    </div>
  );
}
```

It works! :tada:

## Imutabilidade

Ainda relacionado a estados, não é possível no React alterar um estado da seguinte maneira:

```jsx
import { useState } from "react";

export function Counter() {
  const [counter, setCounter] = useState(0);

  function increment() {
    counter += 1;
  }

  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={increment}>Click me</button>
    </div>
  );
}
```

Para se alterar um estado, deve-se usa o setState atribuindo um valor totalmente novo ao estado.

```jsx
import { useState } from "react";

export function Counter() {
  const [counter, setCounter] = useState(0);

  function increment() {
    setCounter(counter + 1);
  }

  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={increment}>Click me</button>
    </div>
  );
}
```

# Renderização

Renderização: fluxo de comparação de componentes entre uma versão anterior com uma nova para então exibir a nova versão do componente em tela.

Momentos em que o React irá realizar uma nova renderização em tela:

## Pai para filho

Quando o componente pai sofrer uma nova renderização (alteração de contexto, estado, propriedade), o componente filho será rerenderizado automaticamente.

```tsx
<Pai>
  <Filho />
</Pai>
```

## Propriedade

Quando a alteração de uma propriedade ocorrer, o React irá renderizar novamente o componente.

```tsx
<Pai>
  <Filho title="Title" />
</Pai>
```

## Hooks (useState, useContext, useReducer..)

```tsx
function Component() {
  const [state, setState] = useState();

  setState("New State"); // isso irá disparar uma nova atualização no componente
}
```

## Fluxo de renderização

1. Gerar uma nova versão do componente que precisa ser renderizado;
2. Comparar essa nova versão com a versão anterior já salva na página;
3. Se houverem alterações, o React renderiza essa nova versão em tela (altera somente o que precisa em tela);
4. O React altera somente o necessário em cada parte da tela devido a seu [algoritmo de reconciliação](https://pt-br.reactjs.org/docs/reconciliation.html);

# Igualdade Referencial

Igualdade referencial é um conceito importante do javascript e também muito utilizado no React.

Basicamente:

```js
console.log({} === {});
false;

console.log([] === []);
false;
```

No javascript, dois objetos aparentemente iguais são considerados diferentes por possuírem diferentes endereços de memória.

## Qual a influência da Igualdade Referencial no React?

Dentro de components, durante o processo de renderização e de comparação realizado pelo React para definir se um componente foi alterado e deve ser renderizado novamente, se existirem objetos, arrays ou funções, estes serão comparados com suas versões anteriores que aparentemente são iguals, porém ocupam diferentes espaços de memória. [Igualdade Referencial](#igualdade-referencial)

# Memo

Com o **memo**, é possível fazer com que um componente filho não seja renderizado novamente caso suas propriedades não tenham sido alteradas.

```tsx
import { memo } from "react";

type ProductItemProps = {
  product: {
    id: number;
    price: number;
    title: string;
  };
};

// shallow compare - comparação rasa
// {} === {} // false
// Referential equality - Igualdade referencial

const ProductItemComponent = ({ product }: ProductItemProps) => (
  <div>
    {product.title} - <strong>{product.price}</strong>
  </div>
);

export const ProductItem = memo(
  ProductItemComponent,
  /* when passing a function as second argument, you can tell the condition that
  defines if the argument has changed */
  (prevProps, nextProps) => {
    return Object.is(prevProps.product, nextProps.product);
  }
);
```

## Quando utilizar o memo:

1. Pure Functional Components (componentes que não estão conectados com algo externo, com dados que podem mudar)
2. Renders too often (components que renderizam demais)
3. Re-renders with same props (rerenderizam tendo as mesmas props)
4. Medium to big size (o memo não traz grandes ganhos para componentes muito pequenos)

# useMemo

O propósito do useMemo é memorizar um valor e evitar que alguma coisa que ocupe muito processamento seja refeita quando o componente renderizar.

Deve ser utilizado para:

1. Cálculos pesados
2. Igualdade referencial (quando a agente repasse aquele informação a um componente filho)

```tsx
import { useMemo } from "react";

import { ProductItem } from "components/ProductItem";

type Product = {
  id: number;
  price: number;
  title: string;
};

type SearchResultsProps = {
  results: Product[];
};

export const SearchResults = ({ results }: SearchResultsProps) => {
  const totalPrice = useMemo(() => {
    return results.reduce((total, product) => total + product.price, 0);
  }, [results]);

  return (
    <div>
      <h2>{totalPrice}</h2>

      {results.map((product) => (
        <ProductItem product={product} />
      ))}
    </div>
  );
};
```

# useCallback

O propósito do useCallback é memorizar uma função. É importante utilizá-lo quando um componente possui funções e estas são repassadas para componentes filhos, levando a um rerender do filho devido a questão de [Igualdade Referencial](#igualdade-referencial). O mesmo se aplica a contextos que possuem funções utilizadas em vários componentes.

```jsx
import { FormEvent, useCallback, useState } from "react";

import { SearchResults } from "components/SearchResults";

const Home = () => {
  const [search, setSearch] = useState("");
  const [results, setResults] = useState([]);

  async function handleSearch(event: FormEvent) {
    event.preventDefault();

    if (!search.trim()) {
      return;
    }

    const response = await fetch(`http://localhost:3333/products?q=${search}`);
    const data = await response.json();

    setResults(data);
  }

  const addToWishlist = useCallback((id: number) => {
    console.log(id);
  }, []);

  return (
    <div>
      <h1>Search</h1>

      <form onSubmit={handleSearch}>
        <input
          type="text"
          value={search}
          onChange={(e) => setSearch(e.target.value)}
        />
        <button type="submit">Buscar</button>
      </form>

      <SearchResults results={results} onAddToWishlist={addToWishlist} />
    </div>
  );
};

export default Home;
```

# Boas práticas

Evitar realizar cálculos durante o momento da renderização do componente. (preferir realizar formatações e cálculos durante obtenção dos dados)

```tsx
async function handleSearch(event: FormEvent) {
  event.preventDefault();

  if (!search.trim()) {
    return;
  }

  const response = await fetch(`http://localhost:3333/products?q=${search}`);
  const data = await response.json();

  const formatter = new Intl.NumberFormat("pt-BR", {
    style: "currency",
    currency: "BRL",
  });

  const products = data.map((product) => ({
    ...product,
    priceFormatted: formatter.format(product.price),
  }));

  const totalPrice = data.reduce((total, product) => total + product.price, 0);

  setResults({ totalPrice, data: products });
}
```

# Bundle

Arquivo final gerado com todo o javascript necessário para a aplicação rodar. A cada import o tamanho do bundle é aumentado.

# Code Splitting e Lazy Loading

Técnicas utilizadas para reduzir o bundle e somente carregar importações quando essas forem necessárias.

Lazy loading na prática:

Exemplo com Next.js (suporta server side):

```tsx
import { memo, useState } from "react";
import dynamic from "next/dynamic";

import { AddProductToWishlistProps } from "components/AddProductToWishlist";

const AddProductToWishlist = dynamic<AddProductToWishlistProps>(
  () =>
    import("./AddProductToWishlist").then((mod) => mod.AddProductToWishlist),
  {
    loading: () => <span>Carregando...</span>,
  }
);

// utilizar componente normalmente...
```

Exemplo somente com React (apenas client side):

```tsx
import { memo, useState, lazy } from "react";
import dynamic from "next/dynamic";

import { AddProductToWishlistProps } from "components/AddProductToWishlist";

const AddProductToWishlist = lazy(() => import("./AddProductToWishlist"));

// utilizar componente normalmente...
```

# Virtualização

Técnica para não renderizar todos os elementos de uma tela, assim elementos não visíveis(não no scroll atual) serão carregados conforme o scroll do usuário.

Exemplo utilizando o react-virtualized:

```tsx
import { List, ListRowRenderer } from "react-virtualized";

import { ProductItem } from "components/ProductItem";

type Product = {
  id: number;
  price: number;
  priceFormatted: string;
  title: string;
};

type SearchResultsProps = {
  totalPrice: number;
  results: Product[];
  onAddToWishlist: (id: number) => void;
};

export const SearchResults = ({
  totalPrice,
  results,
  onAddToWishlist,
}: SearchResultsProps) => {
  const rowRenderer: ListRowRenderer = ({ index, key, style }) => (
    <div key={key} style={style}>
      <ProductItem product={results[index]} onAddToWishlist={onAddToWishlist} />
    </div>
  );

  return (
    <div>
      <h2>{totalPrice}</h2>

      <List
        height={300}
        rowHeight={30}
        width={900}
        overscanRowCount={5}
        rowCount={results.length}
        rowRenderer={rowRenderer}
      />
    </div>
  );
};
```
