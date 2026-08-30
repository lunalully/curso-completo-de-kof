# Módulo 09 · Aula 6 — Secure Coding

> Escrever código que resiste a ataques. As três frentes: **injeção**,
> **validação de entrada** e **saída segura**.

## 1. SQL Injection

**Vulnerável:**

```kof
// NUNCA faça isso
var sql = "select * from users where name = '" + nome + "'"
db.query(db, sql)
```

Se `nome = "' OR '1'='1"`, o atacante lista todos os usuários.

**Seguro — bind com `?`:**

```kof
db.query<User>(db, "select * from users where name = ?", nome)
```

**Regra de ouro:** nenhum valor vindo do usuário vai concatenado em SQL.
Sempre `?`.

## 2. Validação de entrada

Valide **tudo** que vem da rede, no handler, antes de usar:

```kof
app.post("/users") {
    var u = json.decode<User>(body())

    if (u.name.length == 0) {
        return "{\"error\": \"nome obrigatorio\"}"
    }
    if (u.name.length > 50) {
        return "{\"error\": \"nome muito longo\"}"
    }
    if (u.id < 0) {
        return "{\"error\": \"id invalido\"}"
    }

    db.execute(db, "insert into users values (?, ?)", u.id, u.name)
    return "{\"ok\": true}"
}
```

**Valide no formato do domínio:** tamanho, tipo, intervalo, formato (email).

## 3. XSS (Cross-Site Scripting)

**O que é:** injetar `<script>` que roda no browser de outras pessoas.
Acontece quando conteúdo do usuário é renderizado sem escape.

**Em Kof/JS:** os widgets renderizam **texto** (não HTML cru) — essa é a
defesa embutida. **Nunca** monte HTML concatenando entrada do usuário:

```kof
// PERIGOSO (se houver forma de renderizar HTML cru)
var html = "<b>" + nomeDoUsuario + "</b>"

// SEGURO — texto, não markup
label.text = nomeDoUsuario
```

Se você precisar de HTML dinâmico, escape `<`, `>`, `&`, `"`, `'`.

## 4. Saída segura e informação vazada

- Erros genéricos no cliente, detalhes no log.
- Não exponha versões (`0.2.3-beta`), stack traces, SQL, drivers.
- Não coloque segredos em respostas ou logs.

```kof
try {
    var dados = db.query(db, sql)
    return json.encode(dados)
} catch (String e) {
    log.error("falha em /dados: " + e)
    return "{\"error\": \"erro interno\"}"
}
```

## 5. Path traversal e manipulação de caminho

Ao usar `kof.io` com caminhos do usuário, normalize e valide:

```kof
Bool caminhoSeguro(String caminho) {
    var normalizado = Path(caminho).normalize().path()
    return !normalizado.startsWith("..")
}
```

> **Nota:** se a sua versão de `Path` não expor `path()`/`normalize()`,
> valide manualmente (negar `..`, barras iniciais). Teste sempre.

## 6. Comparações secretas

```kof
// inseguro
if (tokenEnviado == tokenEsperado) { }

// seguro
if (security.constantTimeEquals(tokenEnviado, tokenEsperado)) { }
```

## Checklist de secure coding

- [ ] SQL sempre com `?`
- [ ] Entrada validada (tipo, tamanho, intervalo, formato)
- [ ] Saída é texto (nunca HTML cru não escapado)
- [ ] Erros genéricos + log detalhado
- [ ] Caminhos normalizados
- [ ] Comparações constant-time para segredos
- [ ] Não vaza versão/stack/driver

## Exercícios

1. Escreva 3 payloads de SQLi e prove que o bind os neutraliza.
2. Adicione validação completa no POST /users do módulo 06.
3. Tente um path traversal numa rota que lê arquivos (se tiver) e corrija.
4. (Desafio) Monte um pequeno "fuzzer" manual: envie entradas malformadas
   para sua API e verifique que ela não quebra nem vaza.