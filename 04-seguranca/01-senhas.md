# Módulo 04 · Aula 1 — Senhas

> Armazenar senha é uma responsabilidade de segurança. A resposta em Kof é
> `passwords.hash` / `passwords.verify` — secure by default.

## Por que não sha256?

`sha256(password)` é rápido — e rápido é exatamente o que atacantes querem
(ataque de força bruta em GPU). A solução certa usa **derivação de chave
lenta e com salt**: PBKDF2-HMAC-SHA256 com 600k iterações. Em Kof:

```kof
main() {
    var hash = passwords.hash("hunter2")
    println(passwords.verify("hunter2", hash))   // true
    println(passwords.verify("wrong", hash))     // false
    println(passwords.needsRehash(hash))         // false (parâmetros atuais)
}
```

## O formato (versionado, sem ambiguidade)

```text
pbkdf2$sha256$600000$<salt-b64>$<hash-b64>
```

O salt é **aleatório por senha** e o hash é **verificado em tempo constante**.

## Por que `needsRehash` existe

Quando os parâmetros (iterações) mudam com o tempo, hashes antigos precisam
ser recalculados. `needsRehash(hash)` diz se o hash armazenado ainda usa os
parâmetros atuais:

```kof
if (passwords.needsRehash(hashArmazenado)) {
    var novo = passwords.hash(senhaFornecida)
    // atualize o registro do usuário com `novo`
}
```

## Exemplo completo: cadastro e login

```kof
record Usuario(String email, String senhaHash)

class BancoDeUsuarios {
    List<Usuario> usuarios
    constructor() {
        usuarios = listOf()
    }
    void cadastrar(String email, String senha) {
        usuarios.add(Usuario(email, passwords.hash(senha)))
    }
    Bool login(String email, String senha) {
        for (var u in usuarios) {
            if (u.email == email) {
                return passwords.verify(senha, u.senhaHash)
            }
        }
        return false
    }
}

main() {
    var banco = BancoDeUsuarios()
    banco.cadastrar("mel@kof.dev", "minha-senha")
    println(banco.login("mel@kof.dev", "minha-senha"))   // true
    println(banco.login("mel@kof.dev", "errada"))        // false
}
```

## Anti-padrões

- `sha256(password)` para armazenar → **use `passwords.hash`**.
- Comparar hashes com `==` → **use `passwords.verify`** (constant-time).
- Guardar senha em texto plano → nunca, em nenhuma circunstância.

## Exercícios

1. Cadastre 3 usuários e teste login com senha certa e errada.
2. Verifique que dois hashes da mesma senha são **diferentes** (salt aleatório).
3. Simule rehash: force parâmetros antigos e use `needsRehash` para atualizar.
4. (Reflexão) Por que salt aleatório torna tabelas de hash pré-computadas
   inúteis?