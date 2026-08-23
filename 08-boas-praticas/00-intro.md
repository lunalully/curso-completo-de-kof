# Módulo 08 — Boas Práticas

> **Objetivo:** o "padrão ouro" do código Kof — filosofia, idioms,
> anti-patterns, testes, configuração e observabilidade. Baseado no corpus
> oficial (`training/`) do projeto Kof.

## Os princípios

1. **Menos código, mesma capacidade.**
2. **Tipagem forte** — o compilador pega erros antes de rodar.
3. **Intenção acima de cerimônia** — `spawn f()` não `new Thread(...)`.
4. **Um frontend, múltiplos backends** — o código não muda, o target muda.
5. **Direto para o target** — sem camadas de transpile.
6. **Interoperabilidade** — Java/Spring válidos, nunca dependência.
7. **Sem mágica desnecessária.**
8. **Ferramentas importam** — `kof check`, `kof test`, `kof lsp`, `kof debug`.

## Sumário de aulas

| Aula | Tema |
|------|------|
| [01-filosofia.md](01-filosofia.md) | Como pensar em Kof |
| [02-idioms.md](02-idioms.md) | Padrões idiomáticos (BAD/GOOD/WHY) |
| [03-anti-patterns.md](03-anti-patterns.md) | O que NÃO fazer |
| [04-testes.md](04-testes.md) | `assert`, `kof test` |
| [05-configuracao.md](05-configuracao.md) | `kof.config` |
| [06-logging.md](06-logging.md) | `kof.log` e observabilidade |

## A regra de ouro

> Código que compila ≠ código idiomático Kof. O corpus ensina a diferença.
> E se houver conflito: **implementação → testes → documentação → training**.