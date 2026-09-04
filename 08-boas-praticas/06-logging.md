# Módulo 08 · Aula 6 — Logging (kof.log)

> Observabilidade por intenção: níveis, off e JSON estruturado.

## Níveis

```kof
log.debug("detalhe")
log.info("info")
log.warn("cuidado")
log.error("boom")
```

- Nível controlado por env `KOF_LOG_LEVEL`: `debug < info < warn < error < off`
  (default `info`).
- `info`/`debug` → stdout; `warn`/`error` → stderr.
- Formato: `YYYY-MM-DD HH:MM:SS.mmm LEVEL mensagem`.

## Uso em aplicações

```kof
main() {
    log.info("iniciando servidor")
    var app = web.app()

    app.use {
        log.debug(method() + " " + path())
        return null
    }

    app.get("/") {
        log.info("rota / chamada")
        return "kof serve online"
    }

    log.error("nunca deve acontecer: " + algoQueFalhou())
    app.listen(8080)
}
```

## Logging seguro

Combine com `kof.security.secrets.redact`:

```kof
var apiKey = secrets.get("API_KEY")
log.info("conectando com " + secrets.redact(apiKey))
// NUNCA: log.info("chave=" + apiKey)
```

## Boas práticas

1. **Nunca logue segredos** — use `secrets.redact`.
2. **Níveis corretos**: `debug` para detalhe, `info` para eventos, `warn`
   para anormal, `error` para falhas.
3. **Contexto** nas mensagens: `log.error("falha ao salvar usuario " + id)`.
4. Em produção: `KOF_LOG_LEVEL=warn` para reduzir ruído.

## Estado por target

- JVM: ✅ completo (níveis, off).
- Kof.log agora funciona nos 3 targets (LOG001 fechado, 0.2.8-beta).

## Exercícios

1. Loggue os 4 níveis e observe stdout/stderr com `KOF_LOG_LEVEL` variado.
2. Adicione logging de cada request (método + path) no servidor.
3. Redija um segredo antes de logar.
4. Configure `KOF_LOG_LEVEL=warn` e verifique que info/debug somem.