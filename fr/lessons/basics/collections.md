---
version: 1.2.3
title: Collections
---

Les Listes, Tuples, Listes à mots clé, et Maps.

{% include toc.html %}

## Listes

Les listes sont de simples collections de valeurs pouvant inclure différents types. Les listes peuvent aussi inclure plusieurs fois la même valeur.

```elixir
iex> [3.14, :pie, "Apple"]
[3.14, :pie, "Apple"]
```

Elixir implémente les listes sous forme de liste chainée. Par conséquent, calculer la taille d'une liste est une opération `O(n)`. Pour cette raison, il est généralement plus rapide d'ajouter un élément en début de liste plutôt qu'à la fin:

```elixir
iex> list = [3.14, :pie, "Apple"]
[3.14, :pie, "Apple"]
# Ajout en début de liste (rapide)
iex> ["π" | list]
["π", 3.14, :pie, "Apple"]
# Ajout en fin de liste (lent)
iex> list ++ ["Cherry"]
[3.14, :pie, "Apple", "Cherry"]
```

### Concaténation de listes

La concaténation de listes s'effectue grace à l'opérateur `++/2`:

```elixir
iex> [1, 2] ++ [3, 4, 1]
[1, 2, 3, 4, 1]
```

Une remarque à propos du format de nom utilisé plus haut (`++/2`): en Elixir (et Erlang, sur lequel Elixir est construit), une fonction où un nom d'opérateur ont deux composantes: le nom donné (ici `++`) et son _arité_. L'arité est centrale quand on parle de code Elixir (et Erlang). C'est le nombre d'arguments qu'une fonction donnée prend (deux, dans ce cas). L'arité et le nom donné sont combinés avec une barre oblique (slash) `/`. Nous en reparlerons plus tard; cette remarque vous aidera à comprendre la notation pour le moment.

### Soustraction de listes

Le support de la soustraction se fait via l'opérateur `--/2`. On peux soustraire une valeur inexistante sans problème:

```elixir
iex> ["foo", :bar, 42] -- [42, "bar"]
["foo", :bar]
```

Attention aux valeurs dupliquées. Pour chaque élément à droite, sa première occurence est retirée à gauche:

```elixir
iex> [1,2,2,3,2,3] -- [1,2,3,2]
[2, 3]
```

**Note:** Les soustractions de listes utilisent une [comparaison stricte](../basics/#comparaison) pour comparer les valeurs.

### Head / Tail

Il est commun d'utiliser head et tail lorsque nous utilisons des listes. Head (la tête) est le premier élément de la liste et tail (la queue) une liste contenant les éléments restants. Elixir nous fournis deux fonctions, `hd` et `tl`:

```elixir
iex> hd [3.14, :pie, "Apple"]
3.14
iex> tl [3.14, :pie, "Apple"]
[:pie, "Apple"]
```

En plus des fonctions ci-dessus, vous pouvez utiliser le [pattern matching](../pattern-matching/) et l'opérateur pipe `|` pour déstructurer une liste. Nous verrons cet idiome dans les leçons suivantes:

```elixir
iex> [head | tail] = [3.14, :pie, "Apple"]
[3.14, :pie, "Apple"]
iex> head
3.14
iex> tail
[:pie, "Apple"]
```

## Tuples

Les tuples sont similaires aux listes mais sont stockés de manière adjacente en mémoire. On peux calculer leur taille plus rapidement mais cela rend leur modification plus coûteuse, le nouveau tuple devant être copié entièrement en mémoire. Les tuples sont définis avec des accolades :

```elixir
iex> {3.14, :pie, "Apple"}
{3.14, :pie, "Apple"}
```

Les tuples sont souvent utilisés comme une façon de retourner des informations additionnelles d'une fonction. Leur utilité deviendra plus apparente lorsque nous verrons le [pattern matching](../pattern-matching/): :

```elixir
iex> File.read("path/to/existing/file")
{:ok, "... contents ..."}
iex> File.read("path/to/unknown/file")
{:error, :enoent}
```

## Listes à mots clé (Keyword lists)

Les listes à mots clé et les maps sont les collections associatives d'Elixir. En Elixir, une liste à mots clé est un type de liste spécial contenant uniquement des tuples dont le premier élément est un atome (Keyword). Elles sont aussi performantes que les listes classiques :

```elixir
iex> [foo: "bar", hello: "world"]
[foo: "bar", hello: "world"]
iex> [{:foo, "bar"}, {:hello, "world"}]
[foo: "bar", hello: "world"]
```

Les trois caractéristiques des listes à mots clé :

- Les clés sont des atomes.
- Les clés sont ordonnées.
- Les clés peuvent ne pas être uniques.

Pour ces raisons, les listes à mots clé sont la structure de données habituelle pour passer des options aux fonctions.

## Maps

En Elixir les maps sont la structure privilégiée pour les données de type clé-valeur. Contrairement aux listes à keywords, elles permettent d'avoir des clés de n'importe quel type et ne sont pas ordonnées. On définis une map avec la syntaxe `%{}` :

```elixir
iex> map = %{:foo => "bar", "hello" => :world}
%{:foo => "bar", "hello" => :world}
iex> map[:foo]
"bar"
iex> map["hello"]
:world
```

Depuis Elixir 1.2, les variables sont autorisées comme clés d'une Map :

```elixir
iex> key = "hello"
"hello"
iex> %{key => "world"}
%{"hello" => "world"}
```

Si nous ajoutons un doublon à une Map, il remplace la valeur précédente :

```elixir
iex> %{:foo => "bar", :foo => "hello world"}
%{foo: "hello world"}
```

Il existe une syntaxe spéciale pour les Maps ne contenant que des clés de type atome :

```elixir
iex> %{foo: "bar", hello: "world"}
%{foo: "bar", hello: "world"}

iex> %{foo: "bar", hello: "world"} == %{:foo => "bar", :hello => "world"}
true
```

De plus, il existe aussi une syntaxe spéciale pour acceder aux clés de type atome:

```elixir
iex> map = %{foo: "bar", hello: "world"}
%{foo: "bar", hello: "world"}
iex> map.hello
"world"
```

Une autre propriété intéréssante des maps est qu'elles fournissent leur propre syntaxe de mise à jour:

```elixir
iex> map = %{foo: "bar", hello: "world"}
%{foo: "bar", hello: "world"}
iex> %{map | foo: "baz"}
%{foo: "baz", hello: "world"}
```
