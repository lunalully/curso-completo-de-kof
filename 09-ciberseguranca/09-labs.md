# Módulo 09 · Aula 9 — Laboratórios (hands-on)

> Exercícios práticos que consolidam a trilha. Cada laboratório tem: objetivo,
> passos, verificação e uma pergunta de reflexão.

## Lab 1 — Guardião de senhas

**Objetivo:** provar que a plataforma torna o hash de senha à prova de
força bruta trivial.

```kof
main() {
    var hash = passwords.hash("supersecreta")
    println(hash)

    // tente: quantas combinações por segundo você consegue testar?
    // A resposta define o custo de um ataque de força bruta.
    println(passwords.verify("supersecreta", hash))
    println(passwords.verify("errada", hash))
}
```

**Verificação:** dois hashes da mesma senha são diferentes (salt).
**Reflexão:** por que 600k iterações de PBKDF2 derrotam GPU brute force?

## Lab 2 — JWT roleta

**Objetivo:** entender o que o JWT protege e o que não protege.

```kof
main() {
    var segredo = "segredo-do-servidor"
    var token = jwt.create("{\"sub\":\"mel\",\"roles\":[\"user\"]}", segredo)
    println(token)

    // 1. token com segredo errado → falha
    // 2. token com payload adulterado → falha (assinatura)
    // 3. token com 'alg':'none' → a plataforma ignora o alg (HS256 fixo)
    try {
        jwt.verify(token, "outro-segredo", "kof", "api")
        println("aceito (erro!)")
    } catch (String e) {
        println("rejeitado: " + e)
    }
}
```

**Reflexão:** por que confusão de algoritmo (`alg`) é uma das falhas mais
famosas do JWT — e por que o Kof a elimina?

## Lab 3 — API blindada

**Objetivo:** montar a API do "guardião" com todas as defesas.

Combine: `auth` + rate limit + CSRF + validação + erro genérico + logging.

```kof
record User(Int id, String name, String email)
record Credencial(String email, String senha)

main() {
    var app = web.app()
    auth.secret(secrets.get("JWT_SECRET", "dev-secret"))

    app.use {
        if (auth.authenticated() || path() == "/login") {
            return null
        }
        return "{\"error\": \"unauthorized\"}"
    }

    app.post("/login") {
        var c = json.decode<Credencial>(body())
        if (c.email == "mel@kof.dev" && c.senha == "admin") {
            var token = jwt.create("{\"sub\":\"" + c.email + "\",\"roles\":[\"admin\"]}", secrets.get("JWT_SECRET", "dev-secret"))
            return "{\"token\": \"" + token + "\"}"
        }
        return "{\"error\": \"credenciais invalidas\"}"
    }

    app.get("/users") {
        return json.encode(listOf(User(1, "Mel", "mel@kof.dev")))
    }

    app.get("/admin") {
        var admin = auth.hasRole("admin")
        if (!admin) {      // WORKAROUND: atribua antes (nota #4)
            return "{\"error\": \"forbidden\"}"
        }
        return "{\"ok\": true}"
    }

    app.listen(8080)
}
```

**Verificação:** repita o pentest da aula 07 — tudo deve ser bloqueado.

## Lab 4 — Logs que contam história

**Objetivo:** detectar um ataque pelos logs.

Adicione ao Lab 3:

```kof
app.use {
    log.info(method() + " " + path() + " ip=" + header("x-real-ip"))
    return null
}
```

**Tarefa:** rode o brute force do Lab do pentest, depois leia o log e
responda: quem atacou, quando, quantas vezes, de onde? Você consegue
**reconstruir a timeline** do ataque pelos logs?

## Lab 5 — Audit trail (trilha de auditoria)

**Objetivo:** registrar quem fez o quê.

```kof
app.delete("/users/:id") {
    var admin = auth.hasRole("admin")
    if (!admin) {          // WORKAROUND: atribua antes (nota #4)
        return "{\"error\": \"forbidden\"}"
    }
    log.info("admin removeu id " + param("id"))
    return "{\"ok\": true}"
}
```

> `auth.user()` ainda quebra em handler (nota #5) — logue o papel/claims,
> não o `sub`, até o bug fechar.

**Reflexão:** o que um *audit trail* bem feito permite? (responsabilização,
detecção, reconstrução de incidente).

## Lab 6 — Guardião de arquivos

**Objetivo:** proteger acesso a arquivos por usuário.

```kof
// cada usuário só lê os próprios arquivos: /arquivos/:id?
```

Valide que o dono do arquivo == `auth.user()`. Nunca confie num `id` do
cliente sem verificar propriedade (IDOR — Insecure Direct Object Reference).

## Conclusão da trilha

Você já sabe:

- **Construir** com `kof.security` (senhas, crypto, JWT, segredos, auth).
- **Defender** em profundidade (middleware, hardening, rate limit, CSRF).
- **Escrever** código seguro (bind, validação, saída segura).
- **Atacar** seu próprio código (pentest) e **auditar** com checklist.

**Certificação da trilha:** entregue um reporte de auditoria completo
(aula 08) da API do "guardião" — com achados, evidências e correções.