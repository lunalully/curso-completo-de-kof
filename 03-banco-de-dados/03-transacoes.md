# Módulo 03 · Aula 3 — Transações

> Várias operações atômicas: ou **todas** acontecem, ou **nenhuma**. A
> intenção em Kof é um bloco: `transaction { ... }`.

## Commit automático

```kof
main() {
    var db = db.connect("jdbc:h2:mem:test;DB_CLOSE_DELAY=-1")
    db.execute(db, "create table t(x int)")

    transaction {
        db.execute(db, "insert into t values (1)")
        db.execute(db, "insert into t values (2)")
    }

    var rows = db.query(db, "select count(*) as n from t")
    println(rows.get(0))     // {"n":2}
    db.close(db)
}
```

## Rollback em erro

Se algo **lança exceção** dentro do bloco, tudo é desfeito:

```kof
main() {
    var db = db.connect("jdbc:h2:mem:test;DB_CLOSE_DELAY=-1")
    db.execute(db, "create table t(x int)")

    try {
        transaction {
            db.execute(db, "insert into t values (1)")
            throw "boom"
        }
    } catch (String e) {
        println("caught")          // caught
    }

    var rows = db.query(db, "select count(*) as n from t")
    println(rows.get(0))           // {"n":0} — nada foi gravado
    db.close(db)
}
```

## A regra

- `transaction { ... }` usa a **última conexão** aberta.
- Commit automático no fim do bloco; rollback em exceção.
- É a forma Kof de expressar atomicidade — sem `@Transactional`.

## Exemplo real: transferência bancária

```kof
main() {
    var db = db.connect("jdbc:h2:mem:bank;DB_CLOSE_DELAY=-1")
    db.execute(db, "create table contas(id int, saldo int)")

    transaction {
        db.execute(db, "insert into contas values (1, 1000)")
        db.execute(db, "insert into contas values (2, 100)")
    }

    transaction {
        // débito + crédito são uma unidade
        db.execute(db, "update contas set saldo = saldo - 200 where id = 1")
        db.execute(db, "update contas set saldo = saldo + 200 where id = 2")
    }

    var contas = db.query(db, "select * from contas order by id")
    println(contas.get(0))    // {"id":1,"saldo":800}
    println(contas.get(1))    // {"id":2,"saldo":300}
    db.close(db)
}
```

## Exercícios

1. Faça uma transação que insira em duas tabelas (usuario + perfil) e confirme
   que ambas foram gravadas.
2. Force um erro no meio e confirme o rollback (contagem = 0).
3. Escreva a transferência bancária completa com validação de saldo:
   se saldo insuficiente, lance exceção dentro do `transaction`.