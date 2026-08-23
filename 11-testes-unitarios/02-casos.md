# Trilha 11 · Aula 2 — Casos de borda e valores limite

## Onde os bugs moram

Bugs clássicos aparecem nas **fronteiras**:

| Caso | Exemplo |
|------|---------|
| Conjunto vazio | `media([])`, `buscarIndice([], x)` |
| Um único elemento | `mediana([5])` |
| Primeiro/último elemento | `buscaBinaria` com alvo no fim |
| Zero e negativos | `fatorial(0)`, `dividir(x, 0)` |
| Limite de tamanho | nome com 50 chars (limite) e 51 |

## Teste os dois lados de cada limite

```kof
main() {
    // fatorial: 0, 1 e normal
    assert(fatorial(0) == 1, "fat 0")
    assert(fatorial(1) == 1, "fat 1")
    assert(fatorial(5) == 120, "fat 5")

    // divisão por zero lança
    try {
        dividir(1, 0)
        assert(false, "deveria lancar")
    } catch (String e) {
        assert(true, "lancou")
    }
}
```

## Regra

> Para cada entrada, teste: normal, **limite - 1**, **limite**, **limite + 1**,
> e o caso vazio. É onde o programa quebra de verdade.