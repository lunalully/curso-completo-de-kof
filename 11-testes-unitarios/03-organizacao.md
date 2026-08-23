# Trilha 11 · Aula 3 — Organização de suítes

## Estrutura recomendada

```text
projeto/
├── src/            # código
│   ├── busca.kf
│   └── pilha.kf
└── testes/         # um arquivo por unidade
    ├── busca-test.kf
    └── pilha-test.kf
```

## Convenções

- Um arquivo de teste por unidade: `pilha-test.kf` testa `pilha.kf`.
- Nome claro: `<unidade>-test.kf`.
- Cada `main()` de teste: vários `assert` + um `println("...: PASS")`.
- Testes de exceção: `try/catch` + `assert`.

## Rodar a suíte

```bash
kof test testes/          # roda todos os arquivos da pasta
kof test testes/pilha-test.kf   # um arquivo só
```

`kof test` imprime `PASS`/`FAIL` por arquivo e um resumo
(`N passed, M failed`), usando exit code.

## Regra

> Separe testes de código. Se um teste falha, você sabe *onde* olhar — sem
> caçar num arquivo gigante.