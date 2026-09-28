# Módulo 04 · Aula 4 — Segredos e Logs

> Segredos vivem no **ambiente**, nunca no código, nunca nos logs.

## `secrets.get`

```kof
main() {
    // lê do ambiente; fallback opcional
    var chave = secrets.get("API_KEY")
    var comFallback = secrets.get("API_KEY", "valor-padrao")
}
```

- No JVM lê da env; no Native lê de `/proc/self/environ`; no JS da platform.
- Nunca é logado automaticamente.

## `secrets.redact`

Para colocar segredos em logs com segurança:

```kof
main() {
    var raw = "sk-abcdefghijklmnop"
    println(secrets.redact(raw))     // sk-a********mnop
}
```

Redige o meio e mantém as pontas — informação suficiente para debugging sem
expor o segredo.

## Exemplo: logging seguro

```kof
main() {
    var apiKey = secrets.get("API_KEY", "test-key")
    log.info("conectando com chave " + secrets.redact(apiKey))
    // nunca: log.info("chave = " + apiKey)
}
```

> Combine com `kof.log`: `log.debug/info/warn/error`, níveis controlados por
> `KOF_LOG_LEVEL` (debug < info < warn < error < off; default info).
> `warn`/`error` vão para stderr.

## `Secret` — o valor que não vaza (0.5.0, D-SECRETS)

`secrets.get` devolve uma `String` crua que flui invisível para `println`,
concatenação, JSON e logs. O tipo `Secret` torna o caminho seguro o padrão:

```kof
var chave = secrets.of("sk-live-...")     // ou secrets.secret("API_KEY") — env por nome
println(chave)                            // Secret(*** ) — redigido por construção
log.info("key=" + chave)                  // Secret(*** ) — concat também não vaza
if (chave == secrets.secret("API_KEY")) { }  // igualdade de conteúdo constant-time
var raw = chave.reveal()                  // o ÚNICO export cru — greppable na auditoria
```

- `json.encode(chave)` devolve `"Secret(*** )"` — jamais os campos.
- `reveal()` indo direto para `log.*`/`json.encode` levanta o aviso
  `SECN009` (desmascarar precisa ser visível).
- `KeyHandle` fecha o ciclo das chaves: `secrets.keyFromHex(hex)`
  (`keyFromPem`/`keyFromKeystore`), `crypto.hmacSha256(handle, msg)`,
  `jwt.create(claims, handle)`, `handle.rotate()` revoga o handle antigo.
- **Gate honesto:** `Secret`/`KeyHandle` são JVM; JS/Native/Script/Android
  recusam em compile-time com `SECN008` — nunca stub silencioso.

## Anti-padrões

- Segredo hard-coded no código → compromete todo o repo.
- Segredo em log de erro → vaza na observabilidade.
- Segredo em claims/token → base64 não é cifra.
- Usar `==` para comparar tokens → use `security.constantTimeEquals`
  (ou `Secret`/`KeyHandle`, que já comparam constant-time).
- Fazer `reveal()` para logar ou comparar — a única justificativa é entregar
  o material cru a uma API externa, e isso fica visível na revisão.

## Exercícios

1. Leia uma chave do ambiente com `secrets.get` e um fallback.
2. Loggue uma conexão usando `secrets.redact`.
3. Configure `KOF_LOG_LEVEL=debug` e observe a diferença de saída.
4. (Auditoria) Varra seu código por segredos hard-coded e refatore para
   `secrets.get`.