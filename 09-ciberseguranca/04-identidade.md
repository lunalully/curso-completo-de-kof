# Módulo 09 · Aula 4 — Identidade: Autenticação e Autorização

> **Autenticação** = quem é você. **Autorização** = o que você pode fazer.
> Duas coisas diferentes — a trilha trata as duas.

## Modelo

```text
login → emite JWT → cliente guarda → manda no header → servidor verifica →
    auth.authenticated() → auth.user() → auth.hasRole() → autoriza
```

## Autenticação (AuthN)

Quem é você? `passwords.verify` no login + JWT nas requests seguintes.

```kof
record Credencial(String email, String senha)

main() {
    var app = web.app()
    auth.secret(secrets.get("JWT_SECRET", "dev-secret"))

    app.post("/login") {
        var c = json.decode<Credencial>(body())
        // na prática: busque o hash do usuário no banco
        if (passwords.verify(c.senha, hashDoUsuario(c.email))) {
            var token = jwt.create("{\"sub\":\"" + c.email + "\",\"roles\":[\"admin\"]}",
                                   secrets.get("JWT_SECRET", "dev-secret"))
            return "{\"token\": \"" + token + "\"}"
        }
        return "{\"error\": \"credenciais invalidas\"}"
    }

    app.listen(8080)
}
```

## Autorização (AuthZ)

O que você pode fazer? Roles e permissions:

```kof
app.use {
    if (!auth.authenticated()) {
        return "{\"error\": \"unauthorized\"}"
    }
    return null
}

app.get("/users") {
    return json.encode(listOf("mel", "ada"))
}

app.get("/admin") {
    if (!auth.hasRole("admin")) {
        return "{\"error\": \"forbidden\"}"
    }
    return "{\"ok\": true}"
}

app.delete("/users/:id") {
    if (!auth.hasPermission("write")) {
        return "{\"error\": \"forbidden\"}"
    }
    return "{\"ok\": true}"
}
```

## API de identidade

| Função | Tipo |
|--------|------|
| `auth.authenticated()` | Bool — há token válido |
| `auth.token()` | o token Bearer |
| `auth.claims()` | claims verificadas (JSON) |
| `auth.user()` | `sub` (identificador) |
| `auth.hasRole("admin")` | Bool |
| `auth.hasPermission("write")` | Bool |

## JWT: o que ele garante e o que NÃO garante

| Garante | Não garante |
|---------|-------------|
| quem emitiu (assinatura HS256) | que o token não pode ser roubado |
| validade (exp, iss, aud) | que a sessão não expirou "antes" |
| claims imutáveis (sem alteração) | revogação imediata |

**Implicações práticas:**
- `exp` curto (minutos/horas).
- Para revogação imediata: check com o servidor (token versionado/denylist).
- Não coloque dados sensíveis em claims (base64).

## Senhas e identidade

- Mínimo de tentativas no login → resposta atrasada + log (aula 05/06).
- Mensagem genérica "credenciais invalidas" (não diga se o email existe).
- Verificação constant-time (a plataforma já faz com `passwords.verify`).

## Exercícios

1. Implemente login + rota protegida + rota admin completa.
2. Adicione `hasPermission` em um DELETE.
3. Teste: token ausente, token inválido, role errada → cada um com sua
   resposta.
4. (Reflexão) Por que AuthN e AuthZ devem ser avaliadas em camadas
   separadas?