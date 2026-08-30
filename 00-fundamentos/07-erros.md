# Módulo 00 · Aula 7 — Erros e Exceções

> Exceções são **Strings**. Simples, diretas e por intenção.

## Lançar e capturar

```kof
throw "mensagem de erro"
```

```kof
try {
    throw "boom"
} catch (String e) {
    println("caught: " + e)
} finally {
    println("finally")     // roda em todos os caminhos
}
```

## Múltiplos catches

```kof
try {
    arriscado()
} catch (String e) {
    println("erro de string: " + e)
} catch (Int e) {
    println("erro numerico: " + e)
}
```

> No target **Native**, o *primeiro* catch captura (sem dispatch entre
> múltiplos catches). No JVM, funciona como esperado.

## When to use exceção

- Erro que interrompe o fluxo e é tratado em outro ponto.
- `finally` para cleanup que deve rodar em todos os caminhos.

## When NOT to use exceção

- Fluxo normal → `if`.
- Validação simples → `if` + retorno.
- **Ausência como valor** → use `String?` / `Int?` com guarda `if (x != null)` (0.2.0+).
  Não invente sentinelas quando um tipo anulável expressa a intenção melhor.

## BAD — sentinela (string vazia como "não encontrado")

```kof
String find(String key) {
    for (var entry in entries) {
        if (entry.key == key) {
            return entry.value
        }
    }
    return ""
}
```

Quando `""` significa "não encontrado", o consumidor precisa checar por
convenção — dado e erro são indistinguíveis.

## GOOD — exceção carrega a informação

```kof
String find(String key) {
    for (var entry in entries) {
        if (entry.key == key) {
            return entry.value
        }
    }
    throw "not found: " + key
}

// uso
try {
    var v = find("x")
} catch (String e) {
    println("falhou: " + e)
}
```

## Propagação

A exceção atravessa frames em ambos os targets:

```kof
void inner() {
    throw "de-dentro"
}

String outer() {
    try {
        inner()
        return "nao"
    } catch (String e) {
        return "peguei: " + e
    }
}
```

## finally

```kof
try {
    trabalho()
} finally {
    limpar()
}
```

`finally` roda no caminho normal, no caminho capturado e na propagação.

## Limitações honestas

- Sem stack traces no Native.
- Exceções são valores simples (Strings) — ainda não há modelo de objeto de
  exceção.
- Quando o domínio exige **ausência como valor** (não como erro), a sentinela
  documentada pode ser um `WORKAROUND` aceitável — mas marque-o como tal.

## Exercícios

1. Escreva `dividir(Int a, Int b)` que lança `"divisao por zero"` quando `b == 0`.
2. Capte e imprima o erro, garantindo que `finally` rode sempre.
3. Refatore uma função que retorna `-1`/`""` como erro para usar exceção.