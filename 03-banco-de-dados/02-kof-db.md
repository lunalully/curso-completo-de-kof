# Módulo 03 · Aula 2 — A API kof.db

> Tudo em `kof.db`, com intenção direta. Funciona no **JVM** (JDBC),
> no **Native** (SQLite e MySQL/MariaDB wire) e no **JS** (ponte) — com
> gaps honestos onde o backend não existe (`DB001`).

## Conectar

```kof
main() {
    var db = db.connect("jdbc:h2:mem:test;DB_CLOSE_DELAY=-1")
    // ou com credenciais:
    // var db = db.connect("jdbc:h2:mem:test", "sa", "")
    ...
    db.close(db)
}
```

**Novidades 0.5.0 na conexão:**

- **Drivers JDBC automáticos:** `kof run`/`kof build` baixam o driver do
  scheme usado para `~/.kof/deps` (mariadb, sqlite, postgresql, h2, mongo)
  na primeira execução — sem `java -cp` manual, sem `--deps` para driver.
  Offline/falha de download é erro alto com o caminho manual.
- **Schemes curtos normalizam:** `mysql://` e `mariadb://` → `jdbc:mariadb://`,
  `postgres://` → `jdbc:postgresql://` (userinfo vira `user`/`password` do
  query string), `sqlite:...` → `jdbc:sqlite:...`. Sem isso, `DB001`.
- `mongodb://` conecta no JVM/Android; no JS/Native é `DB001` nomeado.
- `CONF001`: ADB/DB cross sem implementação recusa em compile-time — nunca
  valor silenciosamente errado.

## Executar (INSERT/UPDATE/DELETE)

```kof
db.execute(db, "create table users(id int, name varchar(50))")
db.execute(db, "insert into users values (?, ?)", 1, "Mel")
db.execute(db, "insert into users values (?, ?)", 2, "Kof")
```

- `db.execute` devolve o número de linhas afetadas.
- Bind com `?`; args `Int/Long/Bool/String` são convertidos automaticamente.
- Até 4 argumentos por chamada (overloads de aridade fixa).

## Consultar sem tipo → JSON por linha

```kof
var rows = db.query(db, "select * from users where id = ?", 1)
println(rows.get(0))     // {"id":1,"name":"Mel"}
```

`db.query` devolve `List<String>` — cada linha como JSON.

## Consultar tipado → records

```kof
record User(Int id, String name)

var users = db.query<User>(db, "select * from users order by id")
for (var u in users) {
    println(u.id + ": " + u.name)
}
```

`db.query<T>` devolve `List<T>` com bind por nome de coluna. Colunas são
normalizadas para minúsculas (H2/Postgres devolvem maiúsculas).

## Programa completo

```kof
record User(Int id, String name)

main() {
    var db = db.connect("jdbc:h2:mem:app;DB_CLOSE_DELAY=-1")

    db.execute(db, "create table if not exists users(id int, name varchar(50))")
    db.execute(db, "insert into users values (?, ?)", 1, "Mel")
    db.execute(db, "insert into users values (?, ?)", 2, "Ada")

    var users = db.query<User>(db, "select * from users order by id")
    for (var u in users) {
        println(u.id + ": " + u.name)
    }

    var total = db.query(db, "select count(*) as n from users")
    println(total.get(0))     // {"n":2}

    db.close(db)
}
```

## Driver JDBC: a plataforma provisiona (0.5.0)

Antes era preciso baixar o jar e montar o classpath na mão. Desde a 0.5.0 o
`kof run` resolve o driver do scheme automaticamente:

```bash
kof run app.kf          # baixa o driver do scheme (1ª vez) e roda
```

- Versões pinadas (mariadb 3.5.10, sqlite 3.53.4.0, postgresql 42.7.13,
  h2 2.5.250, trio mongo 5.12.0); cache em `~/.kof/deps`.
- Um pin próprio (`kofdeps`) vence o pin da plataforma.
- Sem rede? O erro é alto e explica o drop manual do jar — nunca fallback
  silencioso.
- `kof build --fat` embute os drivers no artefato (JVM).

> MySQL/MariaDB pela rede também roda no **Native** (wire client completo,
> 0.5.0): `connect`/`execute`/`query`/`transaction`, auth
> `mysql_native_password`, binds tipados. Contra auth recusada, a conexão
> lança `mysql: <msg>` — nunca handle morto com SIGSEGV depois.

## Exercícios

1. Crie uma tabela `produtos` e insira 3 produtos.
2. Consulte com `db.query` (sem tipo) e imprima cada linha JSON.
3. Consulte com `db.query<Produto>` e imprima os campos.
4. Faça um UPDATE e confirme com um SELECT.