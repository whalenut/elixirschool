%{
  version: "1.3.0",
  title: "Grundläggande Elixir",
  excerpt: """
  Komma igång, grundläggande datatype och grundläggande operationer.
  """
}
---

## Getting Started

### Installera Elixir

Instrucktioner för hur du installerar Elixir i ditt operativsyster kan du hitta i installationsguiden [Installing Elixir](http://elixir-lang.org/install.html).

Efter att du har installerat Elixir kan du enkelt se vilken version som 

    % elixir -v
    Erlang/OTP {{ site.erlang.OTP }} [erts-{{ site.erlang.erts }}] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:10] [hipe] [kernel-poll:false] [dtrace]

    Elixir {{ site.elixir.version }}

### Prova det interaktiva läget

Elixir installeras med IEx, ett interaktivt skal som tillåter oss evaluera Elixiruttryck.

Vi startar `iex` till att börja med:

    Erlang/OTP {{ site.erlang.OTP }} [erts-{{ site.erlang.erts }}] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:10] [hipe] [kernel-poll:false] [dtrace]

    Interactive Elixir ({{ site.elixir.version }}) - press Ctrl+C to exit (type h() ENTER for help)
    iex>

Notera: I Windows PowerShell, måste du skriva in `iex.bat`.

Vi börjar med att testa några enkla uttryck:

```elixir
iex> 2+3
5
iex> 2+3 == 5
true
iex> String.length("The quick brown fox jumps over the lazy dog")
43
```

Oroa dig inte om du inte förstår varje uttryck ännu.

## Grundläggande datatyper

### Heltal (integers)

```elixir
iex> 255
255
```

Stöd för binära, octala och hexadecimala tal är inbyggt:

```elixir
iex> 0b0110
6
iex> 0o644
420
iex> 0x1F
31
```

### Flyttal (floats)

Flyttal i Elixir kräver att det finns ett decimaltecken efter åẗminstone en siffra, de har 64-bitar dubbel precision och stöder `e` för exponentiella värden.

```elixir
iex> 3.14
3.14
iex> .14
** (SyntaxError) iex:2: syntax error before: '.'
iex> 1.0e-10
1.0e-10
```

### Booleska värden (booleans)

Elixir stödjer `true` (sant) och `false` (falskt) som booleska värden, allt räknas som sant förutom `false` och `nil`:

```elixir
iex> true
true
iex> false
false
```

### Atomer (atoms)

En atom är en konstant vars namn är dess värde.
Om du är bekant med Ruby, är dessa ekvivalenta med symboler i det språket.

```elixir
iex> :foo
:foo
iex> :foo == :bar
false
```

DE booleska värdena `true` och `false` är även atomerna `:true` och `:false`.

```elixir
iex> is_atom(true)
true
iex> is_boolean(:true)
true
iex> :true === true
true
```

Modulnamn är också atomer i Elixir. `MinApp.MinModul` är en giltig atom även om ingen sådan modul har definierats ännu.

```elixir
iex> is_atom(MyApp.MyModule)
true
```

Atomer används även för att referera till moduler i Erlangbibliotek, inklusive de inbyggda.

```elixir
iex> :crypto.strong_rand_bytes 3
<<23, 104, 108>>
```

### Strängar (strings)

Strängar i Elixir är UTF-8 kodade och omges av citationstecken:

```elixir
iex> "Hello"
"Hello"
iex> "dziękuję"
"dziękuję"
```

Strängar hanterar radbryt och bryt-sekvenser.

```elixir
iex> "foo
...> bar"
"foo\nbar"
iex> "foo\nbar"
"foo\nbar"
```

Elixir innehåller även mer komplexa datatyper.
Vi kommer att lära oss mer om dess när vi lär oss om [collections](/en/lessons/basics/collections) och [functions](/en/lessons/basics/functions).

## Grundläggande operationer

### Aritmetiska

Elixir stödjer de grundläggande operatorerna `+`, `-`, `*`, och `/` som en kan förvänta sig.
Det är viktigt att komma ihåg att `/` alltid returnerar ett flyttal.

```elixir
iex> 2 + 2
4
iex> 2 - 1
1
iex> 2 * 5
10
iex> 10 / 5
2.0
```

Om du behöver heltalsdivision eller vill räkna ut resten (dvs. modulo) kommer Elixir med två funktioner som hjälper dig.

```elixir
iex> div(10, 5)
2
iex> rem(10, 3)
1
```

### Booleska

Elixir har följande booleska operatorer `||`, `&&`, och `!`.
Dessa stödjer alla typer:

```elixir
iex> -20 || true
-20
iex> false || 42
42

iex> 42 && true
true
iex> 42 && nil
nil

iex> !42
false
iex> !false
true
```

Det finns ytterligare tre operatorer vars första argument _mǻste_ var ett booleskt värde (`true` eller `false`).

```elixir
iex> true and 42
42
iex> false or true
true
iex> not false
true
iex> 42 and true
** (ArgumentError) argument error: 42
iex> not 42
** (ArgumentError) argument error
```

Notera att Elixirs `and` och `or` egentligen använder Erlangs `andalso` och `orelse`.

### Jämförelse

Elixir kommer med alla de jämförelseoperatorer som vi är vana vid: `==`, `!=`, `===`, `!==`, `<=`, `>=`, `<`, och `>`.

```elixir
iex> 1 > 2
false
iex> 1 != 2
true
iex> 2 == 2
true
iex> 2 <= 3
true
```

För en strikt jämförelse mellan heltal och flyttal, använd `===`:

```elixir
iex> 2 == 2.0
true
iex> 2 === 2.0
false
```

Ett viktigt kännetecken hos Elixir är att alla typer kan jämföras med varandra, det är särskilt användbart i sortering. Vi behöver inte komma ihåg ordningen men det är viktigt att känna till den:

```elixir
number < atom < reference < function < port < pid < tuple < map < list < bitstring
```

Detta kan leda till några intressanta om än giltiga jämförelser som du kanske inte finner i andra språk.

```elixir
iex> :hello > 999
true
iex> {:hello, :world} > [1, 2, 3]
false
```

### Sträninterpolation

Om du har använt Ruby kommer Elixirs stränginterpolation att kännas bekant:

```elixir
iex> name = "Sean"
"Sean"
iex> "Hello #{name}"
"Hello Sean"
```

### Strängkonkatenering

Strängkonkatenering använder `<>` operatorn:

```elixir
iex> name = "Sean"
"Sean"
iex> "Hello " <> name
"Hello Sean"
```
