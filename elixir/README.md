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
    - [Enum e Stream](#enumestream)
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

```
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

```
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

```
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

```
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

```
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

```
x = 4 
4
```

A situação acima não é apenas uma atribuição, mas sim uma operação de match. Quanto x deve valer para que a operação x = 4 seja verdade? X deve ser 4 e por isso esse valor é atribuído a x.

**Uma atribuição só pode acontecer no lado esquerdo da expressão**
```
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

```
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

```
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

```
iex(7)> {:ok, result} = {:ok, 13}
{:ok, 13}

iex(8)> result
13
```

Pin operator (não permite reatribuição de valor):

```
iex(10)> x = 4
4

iex(11)> ^x = 4
4

iex(12)> ^x = 5
** (MatchError) no match of right hand side value: 5
```

Com funções anônimas:

```
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