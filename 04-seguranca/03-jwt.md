# Módulo 04 · Aula 3 — JWT

> Tokens JWT para autenticação stateless. O Kof **fixa HS256** — sem confusão
> de algoritmo (uma das principais falhas do JWT).

## Criar e verificar

```kof
main() {
    var claims = "{\"sub\":\"u1\",\"roles\":[\"admin\"],\"iss\":\"kof\",\"aud\":\"api\"}"
    var token = jwt.create(claims, "s3cret")
    println(token)

    var verificadas = jwt.verify(token, "s3cret", "kof", "api")
    println(verificadas.contains("admin"))   // true — claims verificados
}
```

## Semântica real

- `jwt.create(claimsJson, secret)` → token HS256 com `iat` e `exp`.
- `jwt.verify(token, secret[, iss, aud])` → valida **assinatura**, **exp**,
  **iss** e **aud**. Devolve as claims verificadas (string JSON).
- `jwt.secret()` → usa a env `KOF_JWT_SECRET` ou gera uma (32 bytes hex).

## O que o Kof impede por design

| Falha comum de JWT | O que o Kof faz |
|--------------------|-----------------|
| `alg: none` | HS256 fixo — `alg` do token nunca é confiado |
| Confusão de algoritmo (RS256↔HS256) | impossível: só HS256 |
| Sem expiração | `exp` sempre presente |
| Ignorar iss/aud | `verify` valida se passado |
| Segredo fraco | `jwt.secret()` gera/usa forte |

## Exemplo: token em API

```kof
main() {
    // emissão no login
    var token = jwt.create("{\"sub\":\"mel\",\"roles\":[\"admin\"]}", "s3cret")

    // verificação em cada request (módulo 04, aula 05)
    var ok = true
    try {
        var claims = jwt.verify(token, "s3cret", "kof", "api")
        println("autenticado: " + claims)
    } catch (String e) {
        ok = false
        println("rejeitado: " + e)
    }
}
```

## Regras

- Guarde o segredo no ambiente (**nunca** no código) — veja a aula de
  segredos.
- JWT expira: use `exp` pequeno (minutos/horas), não dias.
- Não coloque dados sensíveis nas claims (são base64, não cifradas).

## Exercícios

1. Crie um token, verifique e imprima as claims.
2. Verifique que um token com segredo errado é rejeitado.
3. Verifique que um token expirado (crie com TTL curto ou manipule) falha.
4. Integre JWT no CRUD do módulo 03 (veja a aula 05 de auth web).