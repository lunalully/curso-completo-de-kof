# Trilha 12 · Aula 4 — Ferramentas da plataforma

> A distribuição viaja com tudo que você precisa para depurar.

## As ferramentas

| Ferramenta | Uso |
|------------|-----|
| `kof check` | type-check sem emitir código (pega erros de compilação) |
| `kof run` | compila e roda (mostra erros de runtime com linha) |
| `kof test [--tag] [--timeout]` | testes (`test "nome", "tag" { }`) — regressão |
| `kof debug [--dap] [--attach] [--target native]` | breakpoints e call stack (0.4.9+: nativo com DWARF) |
| `kof profile [--methods]` | CPU, RSS, GC; `--methods` mostra os métodos quentes (JFR no JVM; `--cpu-prof` + source map no JS) |
| `kof bench [--baseline f] [--fail-on-regression] [--quick]` | benchmark com baseline e gate de regressão |
| `kof inspect` | IR antes/depois da otimização (o que o compilador fez) |
| `kof lsp` | diagnostics em tempo real no editor |
| `kof info` | ambiente (versões, targets) |

## O fluxo de depuração completo

```bash
kof check bug.kf          # 1. erro de compilação? corrige aqui
kof run bug.kf            # 2. erro de runtime? leia a linha
kof test testes/          # 3. erro de lógica? regressão
kof profile bug.kf --methods   # 4. lento? veja os métodos quentes
kof debug bug.kf          # 5. ainda não achou? breakpoints
```

## Regra

> Editor e compilador sempre concordam: o editor consome o LSP que usa o
> **mesmo** frontend do compilador. Nunca duplique o parser num editor.