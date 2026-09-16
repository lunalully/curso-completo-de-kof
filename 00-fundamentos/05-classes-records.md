# Módulo 00 · Aula 5 — Classes, Records e Interfaces

> Dados → record. Comportamento → classe. Lógica → função.

## Classe básica

Campos são declarados **sem** `var`/`val` e sem `;`:

```kof
class User {
    String name
    Int age
}
```

Uma classe **sem construtor** tem construtor padrão **vazio** — os campos se
preenchem por atribuição (`var u = User(); u.name = "Mel"`). Para construir
com argumentos `User("Mel", 26)` você **precisa** do construtor primário
(abaixo) ou de um `constructor(...)` explícito; chamar com args uma classe
sem eles é erro (na 0.4.x o `check` ainda deixa passar e falha em runtime —
bug real, evite).

## Construtor primário (idiomático)

```kof
class User(String name, Int age) {
    greeting(): String {
        return "Hello " + name
    }
}
```

Os parâmetros do construtor primário viram **campos reais** — nenhum
`this.name = name` necessário. Construção sem `new`:

```kof
var user = User("Mel", 26)
```

`new User(...)` ainda é aceito (retrocompatível), mas `User(...)` é a forma
recomendada.

## Construtor explícito (retrocompatível)

```kof
class User {
    String name
    Int age

    public constructor(String name, Int age) {
        this.name = name
        this.age = age
    }
}
```

## Sobrecarga de construtores (0.2.4+)

Construtores podem ser sobrecarregados — múltiplos construtores com
assinaturas diferentes na mesma classe:

```kof
class User {
    String name
    Int age

    public constructor(String name, Int age) {
        this.name = name
        this.age = age
    }

    public constructor(String name) {
        this.name = name
        this.age = 0
    }
}

main() {
    var u1 = User("Mel", 26)
    var u2 = User("Kof")
}
```

## Records — dados imutáveis com zero cerimônia

```kof
record Point(Int x, Int y)
```

O compilador gera: construtor canônico, accessors (`p.x()`), e no JVM também
`toString`/`equals`/`hashCode`.

```kof
var p = Point(3, 4)
println(p.x())               // 3
println(p)                   // Point[x=3, y=4] (no JVM)
```

**Regra:** para "representar um objeto de dados", a resposta padrão é record,
não uma classe com getters.

```kof
// RUIM — cerimônia sem semântica
class User {
    String name
    Int age
    public constructor(String name, Int age) {
        this.name = name
        this.age = age
    }
    public getName(): String { return name }
    public getAge(): Int { return age }
}

// BOM — record
record User(String name, Int age)
```

## Campos públicos vs getters/setters

Kof **não tem** as convenções JavaBeans. Campo público é idiomático:

```kof
class User {
    String name
}

// uso
u.name = "Mel"
println(u.name)
```

Getter/setter só quando houver uma razão real de encapsulamento.

## Herança e dispatch virtual

```kof
class Animal {
    String name
    public constructor(String name) {
        this.name = name
    }
    public speak(): String {
        return "animal"
    }
}

class Dog extends Animal {
    public constructor(String name) {
        super(name)
    }
    public speak(): String {
        return "dog"
    }
}

main() {
    Animal a = new Dog()
    println(a.speak())   // "dog" — dispatch virtual
}
```

- `super(args)` é a primeira instrução do construtor da subclasse.
- Override é implícito (mesmo nome de método).

## Interfaces

```kof
interface Speaker {
    speak(): String
}

class Dog implements Speaker {
    public speak(): String {
        return "woof"
    }
}
```

## Modificadores de acesso

`public`, `private`, `protected`, `static`, `final`.

> **0.3.7 — SEM046 (SG-013):** `private`/`protected` são validados em compile-time. Acesso fora do escopo → diagnóstico, nunca acesso silencioso.

## Dimensões múltiplas (0.3.5+ — KofNewMultiArray)

```kof
var m = new Int[2][3]          // todas as dims alocadas (MULTIANEWARRAY JVM, Array.newInstance interp)
println(m.length)               // 2
println(m[0].length)            // 3
var cubo = new Int[2][3][4]
```

> Antes só a 1ª dim era alocada. Agora `new T[a][b][c]` cria TODAS as dimensões (bug 71 fechado, 5 alvos).

## JSON com records (✅ 3 targets — JSN002 fechado)

```kof
var p = Point(3, 4)
var j = json.encode(p)   // {"x":3,"y":4}
var d = json.decode<Point>("{\"x\": 10, \"y\": 20}")
println(d.x())            // 10 — funciona em JVM/Native/JS
```

> Desde 0.3.0, JSON de objetos/records funciona nos 3 targets. **Na 0.4.x:**
> `json.decode<Map<String,T>>` ganhou decoder real em JVM/Script/JS (no
> Native é gap honesto `JSN004`), e `json.decode<List<Record>>` também é
> `JSN004` no Native — para esses casos compile para JVM/JS. Record no topo
> funciona nos 4 targets.

## Imutabilidade de records (0.3.5+ — SEM038)

```kof
record Point(Int x, Int y)
var p = Point(3, 4)
p.x = 99   // ERRO de compilação SEM038: record é imutável
this.x = 99 // permitido SÓ dentro do construtor de record
```

> `val` também é imutável (SEM037). `record` + `val` são garantias de compile-time — violar vira diagnóstico, não runtime.

## Exercícios

1. Crie um `record Livro(String titulo, String autor, Int ano)`.
2. Crie `class Biblioteca` com `List<Livro>` e métodos `adicionar` e `tamanho`.
3. Modele `Animal`/`Cachorro`/`Gato` com `speak()` virtual e instancie cada um.
4. Refatore uma classe com getters/setters para record ou campos públicos.