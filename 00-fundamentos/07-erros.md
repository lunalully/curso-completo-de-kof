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

## Tipos de catch são validados (0.4.3+)

Exceções Kof são `String`; o compilador recusa capturas impossíveis:

```kof
try {
    throw "boom"
} catch (String e) {
    println("capturado: " + e)
}
```

- `catch (Int e)` → `SEM067` (primitivo não é lançável).
- `catch` de tipo não-lançável → `SEM068`.
- Para exceções vindas de interop Java, capture o tipo de exceção real
  (`Throwable`/`Exception` do `java.lang` resolvem sem import, 0.5.0).

> A exceção atravessa frames e threads nos 4 alvos (per-thread exception
> frame no Native desde a 0.4.1); `await` relança a exceção da task.

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
  exceção (interop Java pode lançar exceções do JDK).
- Quando o domínio exige **ausência como valor** (não como erro), use o tipo
  anulável (`String?`/`Int?` + narrowing) ou `Troolean` — não invente
  sentinela; a linguagem já expressa "pode não existir".

## Exercícios

1. Escreva `dividir(Int a, Int b)` que lança `"divisao por zero"` quando `b == 0`.
2. Capte e imprima o erro, garantindo que `finally` rode sempre.
3. Refatore uma função que retorna `-1`/`""` como erro para usar exceção.