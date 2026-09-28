# Trilha 11 · Aula 1 — Fundamentos

## assert

```kof
assert(condicao, "mensagem de falha")
```

- Se `condicao` é falsa → o programa (ou o bloco `test`) falha.
- Com blocos `test`, cada teste reporta PASS/FAIL individualmente.

## Blocos `test` (0.1.0+)

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

## Tags, setup/teardown e timeout (0.5.0)

Tags são **strings extras** no `test` — zero sintaxe nova:

```kof
test "soma positivos", "smoke", "matematica" {
    assert(soma(2, 3) == 5)
}
```

```bash
kof test testes/ --tag smoke       # roda só os testes com a tag (compile-time)
kof test testes/ --timeout 30      # mata teste que passar de 30s
```

- `--tag` filtra o catálogo **em compile-time** — JVM/Native/JS executam
  exatamente o mesmo conjunto. Tag sem match diz
  `no tests with tag 'x' (of N)` e passa (nunca silencioso).
- `setup()`/`teardown()` são funções zero-arg comuns: se o `setup` lança, o
  teste é **pulado** pelo nome; o `teardown` roda via `finally` mesmo com
  teste falhando.
- `--timeout` é aplicado pelo runner (no JS é melhor esforço, documentado).

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