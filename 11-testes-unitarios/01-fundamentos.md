# Trilha 11 · Aula 1 — Fundamentos

## assert

```kof
assert(condicao, "mensagem de falha")
```

- Se `condicao` é falsa → o programa (ou o bloco `test`) falha.
- Com blocos `test`, cada teste reporta PASS/FAIL individualmente.

## Blocos `test` (0.1.0-beta)

```kof
Int soma(Int a, Int b) {
    return a + b
}

test "soma positivos" {
    assert(soma(2, 3) == 5, "2+3 deve ser 5")
}

test "soma opostos" {
    assert(soma(-1, 1) == 0, "opostos")
}
```

```bash
kof test teste-soma.kf    # PASS soma positivos / PASS soma opostos
```

- O runner é sintetizado em compile-time; cada teste roda isolado
  (falha em um não impede os outros) e o resumo mostra
  `N passed, M failed`.
- Exit code ≠ 0 se houver qualquer falha — pronto para CI.
- Funciona nos 3 targets: `--target jvm|native|js`.

## Estilo legado (exit code)

Arquivos sem blocos `test` mantêm o contrato antigo: `main()` com asserts,
PASS = exit 0. Útil para smoke tests de programa inteiro.

## Convenção

- Um bloco `test` por comportamento; um arquivo por unidade
  (`busca-binaria-test.kf`, `pilha-test.kf`).
- Pasta `testes/` e rodar `kof test testes/`.

## Testando exceções

Como Kof não tem asserção de exceção embutida, use `try/catch`:

```kof
test "divisao por zero lanca" {
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