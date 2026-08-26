# Trilha 11 · Aula 4 — TDD (Red-Green-Refactor)

> TDD = escrever o teste **antes** do código. O teste falha primeiro (Red),
> você implementa o mínimo para passar (Green), e então refatora mantendo
> verde (Refactor).

## Ciclo

```text
Red   → escreva o teste; rode e VEJA FALHAR
Green → implemente o mínimo; rode e veja PASSAR
Refactor → melhore o código mantendo o teste verde
```

## Exemplo: `ehPrimo`

### 1. RED — teste primeiro (vai falhar)

```kof
Bool ehPrimo(Int n) {
    return false   // stub para ver falhar
}

test "2 eh primo" {
    assert(ehPrimo(2), "2 primo")
}

test "3 eh primo" {
    assert(ehPrimo(3), "3 primo")
}

test "4 nao eh primo" {
    assert(!ehPrimo(4), "4 nao primo")
}

test "1 nao eh primo" {
    assert(!ehPrimo(1), "1 nao primo")
}
```

```bash
kof test ehprimo-test.kf   # FAIL (stub)
```

### 2. GREEN — implemente

```kof
Bool ehPrimo(Int n) {
    if (n <= 1) {
        return false
    }
    for (var i = 2; i * i <= n; i = i + 1) {
        if (n % i == 0) {
            return false
        }
    }
    return true
}
```

```bash
kof test ehprimo-test.kf   # PASS
```

### 3. REFACTOR — mantenha verde

Melhore legibilidade/complexidade e rode o teste de novo. Se quebrou, você
mudou o comportamento — refaça.

## Regra

> O teste define o **comportamento esperado**. Red garante que o teste pega
> o bug; Green garante que a implementação resolve; Refactor garante que a
> melhoria não quebra nada.