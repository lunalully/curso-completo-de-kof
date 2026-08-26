# Projeto 5 — Monitor de Segurança

> **Objetivo:** um "canário" de segurança: uma API com honeypot que registra
> ataques, bloqueia e reporta — aplicando toda a trilha de cibersegurança.

## Requisitos

- Módulos: 04, 06, 08 (logging/config), 09 (todo).
- Conceitos: honeypot, detecção, resposta, audit trail.

## O que o monitor faz

1. **Honeypot** — rotas falsas (`/admin-old`, `/backup.zip`) que nunca
   deveriam existir; qualquer acesso é ataque e é logado.
2. **Detecção** — brute force (rate limit) e padrões suspeitos
   (payloads de SQLi na query, paths com `..`).
3. **Bloqueio** — IP que excede o limite é bloqueado (denylist em memória).
4. **Audit trail** — toda ação importante registrada com quem/quando.
5. **Reporte** — endpoint `/reporte` (admin) que resume os eventos.

## Esqueleto

```kof
record Evento(Long quando, String tipo, String ip, String detalhe)

class Monitor {
    List<Evento> eventos
    List<String> bloqueados

    constructor() {
        eventos = listOf()
        bloqueados = listOf()
    }

    void registrar(String tipo, String detalhe) {
        eventos.add(Evento(now(), tipo, header("x-real-ip"), detalhe))
    }

    Bool estaBloqueado(String ip) {
        return bloqueados.contains(ip)
    }

    void bloquear(String ip) {
        if (!bloqueados.contains(ip)) {
            bloqueados.add(ip)
            registrar("bloqueio", "ip " + ip + " bloqueado")
        }
    }
}

main() {
    var monitor = Monitor()
    var app = web.app()
    auth.secret(secrets.get("JWT_SECRET", "dev-secret"))

    // honeypot: rotas falsas
    app.get("/admin-old") {
        monitor.registrar("honeypot", "acesso a /admin-old")
        monitor.bloquear(header("x-real-ip"))
        return "{\"error\": \"not found\"}"
    }

    app.get("/backup.zip") {
        monitor.registrar("honeypot", "acesso a /backup.zip")
        return "{\"error\": \"not found\"}"
    }

    // detecção de payload suspeito
    app.use {
        var q = query("q")
        if (q.contains("' OR") || q.contains("--") || q.contains("..")) {
            monitor.registrar("suspeito", "payload em q: " + q)
            return "{\"error\": \"bad request\"}"
        }
        if (monitor.estaBloqueado(header("x-real-ip"))) {
            return "{\"error\": \"blocked\"}"
        }
        return null
    }

    app.get("/reporte") {
        var admin = auth.hasRole("admin")
        if (!admin) {
            return "{\"error\": \"forbidden\"}"
        }
        return json.encode(monitor.eventos)
    }

    app.listen(8080)
}
```

## Critérios de aceite

- [ ] Acessar `/admin-old` gera evento + bloqueio do IP
- [ ] Payload suspeito na query é detectado e rejeitado
- [ ] IP bloqueado recebe `blocked`
- [ ] `/reporte` lista os eventos (admin)
- [ ] Logs (`kof.log`) complementam o audit trail

## Exercício final (integrador)

Combine tudo: rode o **pentest** (módulo 09 aula 7) contra o monitor e
prove que cada ataque é **detectado**, **registrado** e **respondido**.

## Entregável

Reporte de incidente simulado: timeline dos eventos, decisões tomadas pelo
monitor e o que ainda faltaria (persistência do denylist, TLS, cliente HTTP)
como itens de roadmap honestos.