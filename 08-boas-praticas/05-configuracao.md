# Módulo 08 · Aula 5 — Configuração (kof.config)

> Configuração é capacidade da plataforma: **arquivo > env > profile > default**,
> com leitura tipada.

## A API

```kof
config.str("app.name", "fallback")      // String
config.int("server.port", 8080)         // Int
config.long("server.timeout", 30000)    // Long
config.bool("app.debug", false)         // Bool
config.has("chave")                     // Bool
config.env("KOF_DIRECT")                // lê env direto (ou null)
```

## Fontes, em ordem de precedência

```text
1. Arquivo explícito   (env KOF_CONFIG)      → vence tudo
2. Variável de ambiente (KOF_<CHAVE>)        → ex.: KOF_SERVER_PORT
3. Arquivo de profile  (kof.<KOF_PROFILE>.config)
4. Arquivo default     (kof.config no dir de trabalho)
5. Fallback no código
```

## Exemplo

```kof
main() {
    println(config.str("database.url", "jdbc:h2:mem:test"))
    println(config.int("server.port", 8080))
    println(config.bool("app.debug", false))
}
```

```bash
KOF_SERVER_PORT=9090 KOF_APP_DEBUG=true kof run app.kf
# 9090  true  (e o default para database.url)
```

## Arquivo kof.config

```text
# kof config
server.port=18001
app.name = webapp
app.debug=false
```

## Por que config tipada?

- Erros de tipo em compile-time (não runtime).
- Nomes consistentes entre env e arquivo.
- Segredos continuam no ambiente via `kof.security.secrets` (não em config
  comitada).

## Regra

- Nada de hard-code de configuração no código.
- Segredos → `secrets.get` (env), não `config.str`.
- Use `config` para valores que variam entre ambientes (dev/prod).

## Exercícios

1. Crie um `kof.config` com `server.port` e leia com `config.int`.
2. Sobrescreva por env (`KOF_SERVER_PORT`) e confirme a precedência.
3. Use `KOF_PROFILE=prod` com `kof.prod.config` e teste o profile.
4. Integre config no CRUD do módulo 06 (url do banco + porta).