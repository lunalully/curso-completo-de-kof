# Trilha 11 · Aula 1 — Fundamentos

## assert

```kof
assert(condicao, "mensagem de falha")
```

- Se `condicao` é falsa → o programa sai com exit code ≠ 0.
- `kof test` considera **PASS** (exit 0) / **FAIL** (exit ≠ 0).

## Um teste simples

```kof
Int soma(Int a, Int b) {
    return a + b
}

main() {
    assert(soma(2, 3) == 5, "2+3 deve ser 5")
    assert(soma(-1, 1) == 0, "opostos")
    println("soma: PASS")
}
```

```bash
kof test teste-soma.kf    # PASS
```

## Convenção

- `main()` com vários `assert` e um `println` final.
- Um arquivo por unidade (`busca-binaria-test.kf`, `pilha-test.kf`).
- Pasta `testes/` e rodar `kof test testes/`.

## Testando exceções

Como Kof não tem asserção de exceção embutida, use `try/catch`:

```kof
main() {
    try {
        dividir(1, 0)
        assert(false, "deveria ter lancado")
    } catch (String e) {
        assert(e.contains("zero"), "mensagem do erro")
    }
}
```

## Regra de ouro

> Teste **comportamento**, não implementação. E lembre: código que compila
> ≠ código idiomático — os testes também devem ser idiomáticos.