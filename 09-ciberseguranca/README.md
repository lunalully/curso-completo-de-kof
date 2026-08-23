# Trilha 09 — Cibersegurança com Kof

> **Trilha isolada.** Pré-requisito: Trilhas 04, 06, 08. Ao terminar, você
> constrói software seguro, ataca seu próprio código e audita — com
> `kof.security` e a mentalidade "secure by default".

## O que você vai dominar

| Aula | Tema | Nível |
|------|------|-------|
| [01-fundamentos.md](01-fundamentos.md) | CIA, threat model | básico |
| [02-owasp-top10.md](02-owasp-top10.md) | OWASP Top 10 → Kof | básico |
| [03-crypto-aplicada.md](03-crypto-aplicada.md) | crypto na prática | intermediário |
| [04-identidade.md](04-identidade.md) | AuthN/AuthZ, JWT | intermediário |
| [05-hardening.md](05-hardening.md) | headers, CSRF, CORS, rate limit | avançado |
| [06-secure-coding.md](06-secure-coding.md) | SQLi, XSS, validação | avançado |
| [07-pentest.md](07-pentest.md) | atacar seu código | expert |
| [08-auditoria.md](08-auditoria.md) | checklist e auditoria | expert |
| [09-labs.md](09-labs.md) | laboratórios hands-on | expert |

## Como estudar

1. Leia as aulas na ordem (cada uma já tem exercícios embutidos).
2. Consolide com os [exercícios](exercicios.md) e as [soluções](solucoes/).
3. Faça os **labs** ([09-labs.md](09-labs.md)) em ambiente local.
4. Passe no checkpoint.

## Checkpoint

1. Monte o "guardião de API" do lab 3 (auth + rate limit + validação).
2. Rode o pentest da aula 07 contra ele e **prove** que cada ataque é bloqueado.
3. Entregue um reporte de auditoria (aula 08) com achados, evidências e correções.

⚠️ Tudo em **localhost**, contra **seu** código, com fins defensivos.