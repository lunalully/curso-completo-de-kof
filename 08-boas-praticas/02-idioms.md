# Módulo 08 · Aula 2 — Idioms

> A **forma idiomática** de cada problema, no padrão BAD/GOOD/WHY do corpus.

## 1. Dados → record, comportamento → classe, lógica → função

```kof
record Produto(String id, String nome, Double preco)

class Carrinho {
    List<Produto> itens
    constructor() {
        itens = listOf()
    }
    void adicionar(Produto p) {
        itens.add(p)
    }
}

Double total(Carrinho c) {
    var soma = 0.0
    for (var item in c.itens) {
        soma = soma + item.preco
    }
    return soma
}
```

## 2. Valor condicional → if-expr

```kof
// BAD
var status = ""
if (ativo) {
    status = "online"
} else {
    status = "offline"
}

// GOOD
var status = if (ativo) "online" else "offline"
```

## 3. Comparação de strings → `==`

```kof
// BAD (Java)
if (nome.equals("Mel")) { }

// GOOD
if (nome == "Mel") { }
```

Em Kof, `==` compara conteúdo. O `.equals()` de Java existe porque Java não
sobrecarrega `==`.

## 4. Sequência → `List<T>`, nunca nós manuais

```kof
// BAD
class Node { Node next; Int value }

// GOOD
class Registry {
    List<Entry> entries
}
```

## 5. Funções utilitárias → top-level

```kof
// BAD — utility class
class StringUtils {
    static String capitalizar(String s) { ... }
}

// GOOD — função top-level
String capitalizar(String s) { ... }
```

## 6. Erro → exceção, não sentinela

```kof
// BAD — "" como "não encontrado"
String find(String k) { ...; return "" }

// GOOD — exceção carrega a informação
String find(String k) { ...; throw "not found: " + k }
```

## 7. Construtor primário e construção sem `new`

```kof
class User(String name, Int age) {
    greeting(): String = "Hello " + name
}

var u = User("Mel", 26)   // idiomatic (sem new)
```

## 8. Concorrência por intenção

```kof
// BAD — NÃO EXISTE Thread/Executor
var t = new Thread(() -> work())

// GOOD
spawn work()
```

## 9. Sem estado duplicado

```kof
// BAD — count é projeção de items.size
class Cart {
    List<Int> items
    Int count
    add(Int id) {
        items.add(id)
        count = items.size
    }
}

// GOOD
class Cart {
    List<Int> items
    add(Int id) {
        items.add(id)
    }
    Int size() {
        return items.size
    }
}
```

## 10. Sem camadas de cerimônia

```kof
// BAD — Controller/Service/Repository sem framework
class UserController { UserService service; ... }

// GOOD — o que o problema exige
String listarUsers() {
    return "users"
}
```

## Exercícios

1. Refatore cada BAD acima para GOOD no seu editor.
2. Escreva um `record` + classe + função top-level para um domínio seu.
3. (Revisão) Releia `training/idioms/` no repo e compare com suas soluções.