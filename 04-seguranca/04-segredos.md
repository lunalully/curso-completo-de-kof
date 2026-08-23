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

## Anti-padrões

- Segredo hard-coded no código → compromete todo o repo.
- Segredo em log de erro → vaza na observabilidade.
- Segredo em claims/token → base64 não é cifra.
- Usar `==` para comparar tokens → use `security.constantTimeEquals`.

## Exercícios

1. Leia uma chave do ambiente com `secrets.get` e um fallback.
2. Loggue uma conexão usando `secrets.redact`.
3. Configure `KOF_LOG_LEVEL=debug` e observe a diferença de saída.
4. (Auditoria) Varra seu código por segredos hard-coded e refatore para
   `secrets.get`.