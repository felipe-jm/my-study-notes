# Elixir

<!-- TOC -->

- [Elixir](#elixir)
  - [Paradgima Funcional](#paradigma-funcional)
  - [Vantagens do Elixir](#vantagens-do-elixir)
  - [Definição](#definição)
  - [Conceitos](#conceitos)
    - [Atoms](#atoms)
    - [Lists](#lists)
    - [Tuplas](#tuplas)
    - [Maps](#maps)
    - [Imutabilidade](#imutabilidade)
    - [Pattern Matching](#patternmatching)
    - [Pipe Operator](#pipeoperator)
  - [Concorrência e Paralelismo](#concorrência-e-paralelismo)
  - [Phoenix](#phoenix)

<!-- /TOC -->

# Paradigma Funcional

- Temos somente funções e isso é bom!
- Imutabilidade
  - Redução de efeitos colaterais
  - Concorrência e Paralelismo
- Mais fácil de testar e crescer a codebase com qualidade
- Te torna um dev melhor!

# Vantagens do Elixir

- Elixir é funcional
- Código simples, explícito e legível
- Erlang VM (BEAM)
  - Alta disponibilidade e escalabilidade
  - Em 2018, rodava 90% de todo o tráfego da Internet
- Pode usar todo poder computacional disponível

# Definição

Elixir é uma linguagem funcional que roda em cima da máquina virtual da Erlang. É totalmente compatível com Erlang, mas traz uma sintaxe diferente e muitas features novas.

# Atoms

Atoms são como strings constantes

```elixir
:elixir
:ok
:error

:elixir == :elixir
true

:elixir == :typescript
false

is_atom(:elixir)
true

is_atom("elixir")
false
```

# Lists

São listas encadeadas (Linked lists)

```elixir
iex(1)> [1, 2, 3.5, "string"]
[1, 2, 3.5, "string"]

iex(2)> [1, 2, 3] ++ [4, 5, 6]
[1, 2, 3, 4, 5, 6]

iex(3)> [1, 2, 3] -- [4, 5, 6]
[1, 2, 3]

iex(4)> [1, 2, 3] -- [1, 3]
[2]

# Acessa a head de uma lista
iex(5)> hd([1, 2, 3])
1

# Acessa a tail de uma lista
iex(6)> tl([1, 2, 3])
[2, 3]

Não existe acesso direto em uma lista encadeada
iex(1)> [1, 2, 3]
[1, 2, 3]

iex(2)> x = [1, 2, 3]
[1, 2, 3]

iex(3)> x[0]
** (ArgumentError) the Access calls for keywords expect the key to be an atom, got: 0
    (elixir 1.11.0) lib/access.ex:311: Access.get/3
iex(3)>
```

# Tuplas

São como listas, porém são armazenadas continuamente na memória, podendo ter seus elementos acessados separadamente. São mais utilizadas para retornos de funções.

```elixir
iex(1)> {1, 2, 3}
{1, 2, 3}

iex(2)> x = {1, 2, 3, 4, 5, 6}
{1, 2, 3, 4, 5, 6}

iex(3)> elem(x, 4)
5
iex(4)> put_elem(x, 4 , "elixir")
{1, 2, 3, 4, "elixir", 6}

iex(5)> put_elem(x, 8, "test")
** (ArgumentError) argument error
    :erlang.setelement(9, {1, 2, 3, 4, 5, 6}, "test")

iex(5)> put_elem(x, 7, "test")
** (ArgumentError) argument error
    :erlang.setelement(8, {1, 2, 3, 4, 5, 6}, "test")

iex(5)> put_elem(x, 5, "test")
{1, 2, 3, 4, 5, "test"}
```

# Maps

```elixir
iex(3)> %{a: 1, b: 2, c: 3}
%{a: 1, b: 2, c: 3}

iex(4)> %{"a" => 1, "b" => 2, "c" => 3}
%{"a" => 1, "b" => 2, "c" => 3}

iex(5)> meu_map = %{a: 1, b: 2, c: 3}
%{a: 1, b: 2, c: 3}

iex(6)> meu_map.a
1

iex(7)> meu_map.b
2

iex(8)> meu_map = %{"a" => 1, "b" => 2, "c" => 3}
%{"a" => 1, "b" => 2, "c" => 3}

iex(9)> meu_map.a
** (KeyError) key :a not found in: %{"a" => 1, "b" => 2, "c" => 3}

iex(9)> meu_map["a"]
1

iex(10)> Map.get(meu_map, "a")
1

iex(11)> Map.put(meu_map, "d", 5.5)
%{"a" => 1, "b" => 2, "c" => 3, "d" => 5.5}

iex(12)> Map.put(meu_map, "c", 3.5)
%{"a" => 1, "b" => 2, "c" => 3.5}

iex(13)> x = %{c: 5, d: 8, e: 9}
%{c: 5, d: 8, e: 9}

iex(14)> %{x | d: 6}
%{c: 5, d: 6, e: 9}

iex(15)> %{x |d: 6, f: 610}
** (KeyError) key :f not found in: %{c: 5, d: 6, e: 9}
    (stdlib 3.14) :maps.update(:f, 610, %{c: 5, d: 6, e: 9})
    (stdlib 3.14) erl_eval.erl:259: anonymous fn/2 in :erl_eval.expr/5
    (stdlib 3.14) lists.erl:1267: :lists.foldl/3
```

# Imutabilidade

Imutabilidade é uma maneira de ter certeza de que o valor de uma variável nunca será alterado de maneira implícita. Uma função nunca pode alterar o valor de uma variável utilizada como parâmetro.

```elixir
iex(1)> x = "Eu sou IMUTÁVEL"
"Eu sou IMUTÁVEL"

iex(2)> String.downcase(x)
"eu sou imutável"

iex(3)> x
"Eu sou IMUTÁVEL"

iex(4)> x = String.downcase(x)
"eu sou imutável"

iex(5)> x
"eu sou imutável"
```

# Pattern Matching

```elixir
x = 4
4
```

A situação acima não é apenas uma atribuição, mas sim uma operação de match. Quanto x deve valer para que a operação x = 4 seja verdade? X deve ser 4 e por isso esse valor é atribuído a x.

**Uma atribuição só pode acontecer no lado esquerdo da expressão**

```elixir
iex(1)> x = 4
4

iex(2)> x
4

iex(3)> 4 = x
4

iex(4)> 5 = x
** (MatchError) no match of right hand side value: 4

iex(4)> x = 5
5

iex(5)> 5 = x
5
```

Com listas:

```elixir
iex(1)> [a, b, c] = [1, 2, 3]
[1, 2, 3]

iex(2)> a
1

iex(3)> b
2

iex(4)> c
3

iex(5)> [head | tail] = [1, 2, 3, 4, 5]
[1, 2, 3, 4, 5]

iex(6)> head
1

iex(7)> tail
[2, 3, 4, 5]
```

Com maps:

```elixir
iex(1)> %{c: valor} = %{a: 1, b: 2, c: 3, d: 4}
%{a: 1, b: 2, c: 3, d: 4}

iex(2)> valor
3

iex(3)> %{c: valor, a: valor_a} = %{a: 1, b: 2, c: 3, d: 4}
%{a: 1, b: 2, c: 3, d: 4}

iex(4)> valor
3

iex(5)> valor_a
1
```

Com tuplas:

```elixir
iex(7)> {:ok, result} = {:ok, 13}
{:ok, 13}

iex(8)> result
13
```

Pin operator (não permite reatribuição de valor):

```elixir
iex(10)> x = 4
4

iex(11)> ^x = 4
4

iex(12)> ^x = 5
** (MatchError) no match of right hand side value: 5
```

Com funções anônimas:

```elixir
iex(13)> multiply = fn a, b -> a * b end
#Function<43.79398840/2 in :erl_eval.expr/5>

iex(14)> multiply.(2,3)
6

iex(15)> read_file = fn
...(15)> {:ok, result} -> "Success #{result}"
...(15)> {:error, reason} -> "Error #{reason}"
...(15)> end
#Function<44.79398840/1 in :erl_eval.expr/5>

iex(16)> read_file.(File.read("test.txt"))
"Success meu_arquivo_texto\n"

iex(17)> read_file.(File.read("eunaoexisto.txt"))
"Error enoent"
```

# Pipe Operator

Maneira simplificada de realizar operações em sequência sobre uma variável. Digamos que a gente precise formatar uma string, removendo espaços em brancos e colocando tudo em letras minúsculas.

**Sem Pipe Operator**:

```elixir
iex(1)> string = " aaABbbbaaaBBaA "
" aaABbbbaaaBBaA "

iex(2)> string = String.trim(string)
"aaABbbbaaaBBaA"

iex(3)> string = String.downcase(string)
"aaabbbbaaabbaa"

iex(4)> string
"aaabbbbaaabbaa"
```

**Com Pipe Operator**:

```elixir
iex(6)> " aaABbbbaaaBBaA " |> String.trim() |> String.downcase()
"aaabbbbaaabbaa"
```

# Concorrência e Paralelismo

Dois irmãos jogando videogame. :video_game:

## Concorrência

Se houver somente um videogame, os irmãos terão que concorrer pelo videogame, combinando de uma hora um deles jogar e outra hora o outro.

## Paralelismo

Se houver dois videogames, os irmão poderão jogar ao mesmo tempo. Jogando de forma paralela.

## Processos

- Os desafios de um sistema web atual
- Os processos devem ser isolados

Criando um processo:

```elixir
iex(2)> pid = spawn fn -> IO.puts("I am a process") end
I am a process
#PID<0.112.0>

iex(3)> Process.alive?(pid)
false

iex(4)> pid_iex = self()
#PID<0.107.0>

iex(5)> send pid_iex, {:ok, "Mensagem de sucesso!"}
{:ok, "Mensagem de sucesso!"}

iex(6)> receive do
...(6)> {:ok, msg} -> msg
...(6)> {:error, _} -> "Oops!"
...(6)> end
"Mensagem de sucesso!"

iex(7)> send pid_iex, {:error, "Mensagem de erro"}
{:error, "Mensagem de erro"}

iex(8)> receive do
...(8)> {:ok, msg} -> msg
...(8)> {:error, _} -> "Oops!"
...(8)> end
"Oops!"

iex(9)> spawn fn -> send(pid_iex, {:ok, "deu certo?"}) end
#PID<0.126.0>

iex(10)> receive do
...(10)> {:ok, msg} -> msg
...(10)> end
"deu certo?"
```

## Processos na BEAM

- Escalabilidade
- Tolerância à falhas
- Distruibção

- Processo do sistema operacional vs Processo da BEAM.
- Os processos são muito leves
- Criados em questões de microsegundos e utiliza poucos kbytes. Processos do SO gastam alguns Megabytes.

- Cada processo é uma thread com execução concorrente
- Um scheduler por núcleo da CPU
  - A máquina virtual roda em um único processo do sistema operacional
  - Cada scheduler pode criar milhares de processos. O limite teórico é de 134 milhões!

![Beam Process](https://res.cloudinary.com/dqcqifjms/image/upload/v1615382370/felipejung/beam_process_oypp84.jpg)
