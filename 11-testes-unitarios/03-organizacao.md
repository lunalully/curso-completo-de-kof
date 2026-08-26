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
- Cada bloco `test "nome" { }`: um comportamento, asserts com mensagem.
- Testes de exceção: `try/catch` + `assert` dentro do bloco.

## Rodar a suíte

```bash
kof test testes/          # roda todos os arquivos da pasta
kof test testes/pilha-test.kf   # um arquivo só
```

`kof test` imprime `PASS nome`/`FAIL nome: mensagem` por bloco `test`
e um resumo (`N passed, M failed`); exit code ≠ 0 se houver falha.

## Regra

> Separe testes de código. Se um teste falha, você sabe *onde* olhar — sem
> caçar num arquivo gigante.