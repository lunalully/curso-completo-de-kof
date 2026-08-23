# Trilha 04 · Exercícios

> ★ básico · ★★ intermediário · ★★★ avançado. Rode tudo com `kof run`.

## Senhas (aula 01)

- ★ [01-hash-verify.kf](solucoes/01-hash-verify.kf) — `passwords.hash` + `verify` (certa/errada).
- ★★ [02-rehash.kf](solucoes/02-rehash.kf) — `needsRehash` e atualização do hash.
- ★★ [03-banco-usuarios.kf](solucoes/03-banco-usuarios.kf) — cadastro + login em memória.
- ★★★ [04-persistir-hash.kf](solucoes/04-persistir-hash.kf) — salve hashes num arquivo (`kof.io`) e faça login lendo do arquivo.

## Crypto (aula 02)

- ★ [05-digests.kf](solucoes/05-digests.kf) — sha256/sha512/hmacSha256 e compare com `constantTimeEquals`.
- ★★ [06-aes-gcm.kf](solucoes/06-aes-gcm.kf) — round-trip + detecção de tamper.
- ★★ [07-assinatura.kf](solucoes/07-assinatura.kf) — `assinatura(msg, chave)` + `verificar` com HMAC.
- ★★★ [08-cifrar-arquivo.kf](solucoes/08-cifrar-arquivo.kf) — cifre um arquivo com AES-GCM, salve a chave em env (`secrets.get`).

## JWT (aula 03)

- ★★ [09-jwt.kf](solucoes/09-jwt.kf) — create/verify com iss/aud; segredo errado falha.
- ★★ [10-jwt-exp.kf](solucoes/10-jwt-exp.kf) — verifique que um token adulterado é rejeitado.
- ★★★ [11-token-api.kf](solucoes/11-token-api.kf) — fluxo login → token → verificação nas "rotas".

## Segredos (aula 04)

- ★ [12-segredos.kf](solucoes/12-segredos.kf) — `secrets.get` com fallback + `secrets.redact`.
- ★★ [13-log-seguro.kf](solucoes/13-log-seguro.kf) — logue uma conexão redigindo a chave (use `log.info`).

## Auth web (aula 05)

- ★★★ [14-auth-web.kf](solucoes/14-auth-web.kf) — API com `app.use` + `auth.authenticated()` + rota admin com `hasRole`. (requer módulo 06; rode com `kof serve`)

## Desafio integrador

[15-integrador.kf](solucoes/15-integrador.kf) — **cofre de senhas**:
- record `Senha(String site, String hash)`.
- `cadastrar(site, senha)` → armazena `passwords.hash`.
- `verificar(site, senha)` → `passwords.verify`.
- Persista em `cofre.txt` com `kof.io`.
- Compare qualquer hash com `constantTimeEquals`.