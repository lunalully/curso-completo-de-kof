# Trilha 04 — Segurança com kof.security

> **Trilha isolada.** Pré-requisito: Trilha 00 (+ 06 para auth web). Ao
> terminar, você protege senhas, dados, tokens e APIs — secure by default.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-senhas.md](01-senhas.md) | `passwords.hash/verify/needsRehash` |
| [02-crypto.md](02-crypto.md) | sha256, HMAC, AES-GCM, random seguro |
| [03-jwt.md](03-jwt.md) | JWT HS256 (create/verify) |
| [04-segredos.md](04-segredos.md) | `secrets.get/redact` + `Secret`/`KeyHandle`, logs seguros |
| [05-auth-web.md](05-auth-web.md) | `auth.*` + `app.security()` (CSRF, sessão, OAuth2 resource server) |

## Estado por target (0.5.0)

JVM completo (incl. `Secret`/`KeyHandle` e `app.security()`); JS com ponte e
`SECN000` para CSRF/headers; Native x86 com gates `SECN002` (ChaCha20) e
`SECN008` (Secret); tudo reportado em **compile-time** — nunca fallback
silencioso.

## Como estudar

1. Leia as aulas na ordem. Rode os exemplos (`kof run`).
2. Faça os [exercícios](exercicios.md).
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

Um programa que:
1. Cadastra um usuário com `passwords.hash`.
2. Faz login verificando com `passwords.verify`.
3. Emite um JWT com `jwt.create` e verifica com `jwt.verify` (iss/aud).
4. Cifra e decifra uma mensagem com AES-GCM.
5. Compara um segredo com `security.constantTimeEquals`.

`kof run` sem erros → trilha concluída.