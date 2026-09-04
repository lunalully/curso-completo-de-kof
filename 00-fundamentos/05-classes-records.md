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

## JSON com records

Records são suportados por `json.encode` / `json.decode<T>` no **JVM**:

```kof
var p = Point(3, 4)
var j = json.encode(p)   // {"x":3,"y":4}
var d = json.decode<Point>("{\"x\": 10, \"y\": 20}")
```

**Limitação:** JSON de objetos/records no target Native não é suportado
JSON de objetos/records agora funciona nos 3 targets (JSN002 fechado). Primitivos/lists funcionam em ambos.

## Exercícios

1. Crie um `record Livro(String titulo, String autor, Int ano)`.
2. Crie `class Biblioteca` com `List<Livro>` e métodos `adicionar` e `tamanho`.
3. Modele `Animal`/`Cachorro`/`Gato` com `speak()` virtual e instancie cada um.
4. Refatore uma classe com getters/setters para record ou campos públicos.