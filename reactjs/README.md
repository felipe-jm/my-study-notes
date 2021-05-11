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
