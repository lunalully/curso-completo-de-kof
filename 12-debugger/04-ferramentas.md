# Trilha 12 · Aula 4 — Ferramentas da plataforma

> A distribuição viaja com tudo que você precisa para depurar.

## As ferramentas

| Ferramenta | Uso |
|------------|-----|
| `kof check` | type-check sem emitir código (pega erros de compilação) |
| `kof run` | compila e roda (mostra erros de runtime com linha) |
| `kof test` | testes (`test "nome" { }`, PASS/FAIL por teste) — regressão |
| `kof debug` | servidor DAP (breakpoints, call stack) |
| `kof lsp` | diagnostics em tempo real no editor |
| `kof info` | ambiente (versões, targets) |

## O fluxo de depuração completo

```bash
kof check bug.kf          # 1. erro de compilação? corrige aqui
kof run bug.kf            # 2. erro de runtime? leia a linha
kof test testes/          # 3. erro de lógica? regressão
kof debug bug.kf          # 4. ainda não achou? breakpoints
```

## Regra

> Editor e compilador sempre concordam: o editor consome o LSP que usa o
> **mesmo** frontend do compilador. Nunca duplique o parser num editor.