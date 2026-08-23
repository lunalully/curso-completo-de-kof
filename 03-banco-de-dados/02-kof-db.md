# Módulo 03 · Aula 2 — A API kof.db

> Tudo em `kof.db`, com intenção direta. Funciona no target **JVM**.

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

## Rodando com o driver JDBC

Como `kof run` compila e executa na JVM, o driver precisa estar no classpath:

```bash
# compile
kof build app.kf --target jvm --output out
# execute com o driver H2
java -cp "out:h2.jar" Default.Main
```

> Nos testes oficiais do projeto, o subprocesso roda com o jar do H2 no
> classpath. Este é o padrão para programas que usam `kof.db`.

## Exercícios

1. Crie uma tabela `produtos` e insira 3 produtos.
2. Consulte com `db.query` (sem tipo) e imprima cada linha JSON.
3. Consulte com `db.query<Produto>` e imprima os campos.
4. Faça um UPDATE e confirme com um SELECT.