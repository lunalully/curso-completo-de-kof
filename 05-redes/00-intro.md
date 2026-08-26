# Módulo 05 — Redes

> **Objetivo:** entender como dados viajam entre máquinas — do modelo TCP/IP
> até HTTP — e como Kof abstrai isso por intenção.

## A filosofia aplicada a redes

Você **não** cria sockets para servir HTTP. A plataforma (`kof.web`, `kof
serve`) faz isso:

```kof
app.get("/hello") {
    return "Hello from Kof"
}
```

O servidor HTTP, o parsing, as virtual threads — tudo pertence à plataforma.
A intenção é a rota.

## Sumário de aulas

| Aula | Tema |
|------|------|
| [01-modelo.md](01-modelo.md) | Modelo TCP/IP e endereçamento |
| [02-http.md](02-http.md) | HTTP: o protocolo da web |
| [03-sockets.md](03-sockets.md) | Sockets (o que a plataforma abstrai) |
| [04-concorrencia.md](04-concorrencia.md) | `spawn`: concorrência por intenção |

## O que a plataforma fornece hoje

| Capacidade | Estado |
|------------|--------|
| Servidor HTTP (`kof serve`, `web.app()`) | ✅ JVM |
| Cliente HTTP (`kof.http`: get/post/put/delete/patch) | ✅ JVM (HTTP002 Native/JS) |
| Concorrência (`spawn`, `spawn` expr + `await`) | ✅ JVM |
| TLS/HTTPS (`web.listenSecure(port)`, `kof.http` HTTPS) | ✅ JVM (WEB002 Native/JS) |
| Sockets crus expostos ao programador | ⏳ planned — não invente |

Se algo não existe, o Kof reporta em compile-time — e o curso marca
`WORKAROUND`/planned.