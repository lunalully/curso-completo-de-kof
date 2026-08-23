# Módulo 04 — Segurança

> **Objetivo:** segurança de verdade com `kof.security` — senhas, crypto, JWT,
> segredos e autenticação web. **Secure by default**: as escolhas seguras são
> automáticas.

## A filosofia

> `passwords.verify(pw, hash)` — nunca primitivas soltas para a aplicação
> montar segurança à mão.

A plataforma decide *como* (PBKDF2 600k, HS256 fixo, salt aleatório,
comparação constant-time). Você expressa a *intenção*.

## Estado por target

| Função | JVM | Native | JS |
|--------|-----|--------|----|
| `passwords.hash/verify/needsRehash` | ✅ | ❌ SECN001 | ✅ |
| `crypto.sha256` / `hmacSha256` | ✅ | ✅ | ✅ |
| `crypto.sha512` | ✅ | ❌ SECN003 | ✅ |
| `crypto.aesGcm` (encrypt/decrypt) | ✅ | ❌ SECN002 | ❌ SECN002 |
| `jwt.create/verify` | ✅ | ❌ | ✅ |
| `secrets.get` / `redact` | ✅ | ✅ | ✅ |
| `security.constantTimeEquals` | ✅ | ✅ | ✅ |
| `auth.*` (contexto web) | ✅ | ❌ | ❌ |

Gaps são **compile-time** (`SECN00x`) — nunca silenciosos.

## Sumário de aulas

| Aula | Tema |
|------|------|
| [01-senhas.md](01-senhas.md) | Hash e verificação de senhas |
| [02-crypto.md](02-crypto.md) | sha256, HMAC, AES-GCM, aleatório seguro |
| [03-jwt.md](03-jwt.md) | Tokens JWT (HS256) |
| [04-segredos.md](04-segredos.md) | Segredos em env e redação para logs |
| [05-auth-web.md](05-auth-web.md) | Autenticação e autorização em APIs |

## Regras de ouro (já na primeira aula)

1. **Nunca** `sha256(password)` — use `passwords.hash`.
2. **Nunca** `==` para comparar tokens/hashes — use `security.constantTimeEquals`.
3. **Nunca** imprima segredos em logs — use `secrets.redact`.
4. **Nunca** confie no `alg` do token — o Kof fixa HS256.