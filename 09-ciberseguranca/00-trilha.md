# Módulo 09 — Trilha Completa de Cibersegurança com Kof

> **Objetivo:** formar um profissional capaz de **construir** software seguro
> e **auditar** código Kof — com a plataforma `kof.security` e a filosofia
> "secure by default".

## Níveis da trilha

| Nível | Foco | Aulas |
|-------|------|-------|
| **Básico** | Fundamentos de segurança e mentalidade | 01, 02 |
| **Intermediário** | Criptografia aplicada e identidade | 03, 04 |
| **Avançado** | Defesa em profundidade: hardening, secure coding | 05, 06 |
| **Expert** | Atacar seu próprio código e auditar | 07, 08, 09 |

## A filosofia da trilha

> **Secure by default.** As escolhas seguras são automáticas na plataforma —
> PBKDF2 600k, HS256 fixo, salt aleatório, comparação constant-time. A trilha
> ensina *por quê* cada escolha existe e *como abusar* de quem as ignora.

```text
construir seguro → kof.security → auditar → corrigir → repetir
```

## Sumário de aulas

| Aula | Tema | Nível |
|------|------|-------|
| [01-fundamentos.md](01-fundamentos.md) | CIA, threat model, princípios | Básico |
| [02-owasp-top10.md](02-owasp-top10.md) | OWASP Top 10 mapeado para Kof | Básico |
| [03-crypto-aplicada.md](03-crypto-aplicada.md) | Criptografia na prática | Intermediário |
| [04-identidade.md](04-identidade.md) | AuthN, AuthZ, JWT, sessões | Intermediário |
| [05-hardening.md](05-hardening.md) | Headers, CSRF, CORS, rate limit | Avançado |
| [06-secure-coding.md](06-secure-coding.md) | SQLi, XSS, validação de entrada | Avançado |
| [07-pentest.md](07-pentest.md) | Atacando seu próprio código | Expert |
| [08-auditoria.md](08-auditoria.md) | Checklists e auditoria | Expert |
| [09-labs.md](09-labs.md) | Laboratórios hands-on | Expert |

## O que você constrói ao longo da trilha

Um **"guardião de API"** — um conjunto de middlewares e verificações que
protege a API do módulo 06 contra os ataques mais comuns, auditado aula a
aula.

> **Aviso importante:** os laboratórios (07/09) são sobre **seu próprio**
> código, em ambiente local, para aprendizado defensivo. Atacar sistemas de
> terceiros sem autorização é ilegal.