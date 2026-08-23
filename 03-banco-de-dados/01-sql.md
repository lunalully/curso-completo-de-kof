# Módulo 03 · Aula 1 — Fundamentos de SQL

> SQL é a linguagem de dados. `kof.db` fala SQL direto — então esta aula é
> obrigatória. Conceitos mínimos para sobreviver:

## CREATE TABLE

```sql
create table users (
    id int,
    name varchar(50),
    age int
)
```

## INSERT

```sql
insert into users (id, name, age) values (1, 'Mel', 26)
```

## SELECT

```sql
select * from users
select id, name from users where age > 18 order by name
select count(*) as total from users
```

## UPDATE / DELETE

```sql
update users set age = 27 where id = 1
delete from users where id = 1
```

## Prepared statements (placeholders `?`)

```sql
select * from users where id = ?
insert into users values (?, ?, ?)
```

> **Sempre** use `?` para valores vindos do usuário. Nunca concatene SQL.
> (Este é o início do módulo de segurança.)

## Tipos comuns

- `int`, `bigint`, `varchar(n)`, `text`, `boolean`, `date`, `timestamp`, `double`.

## JOIN (conceito)

```sql
select u.name, o.total
from orders o
join users u on u.id = o.user_id
```

## Exercícios

1. Modele uma tabela `produtos(id, nome, preco)`.
2. Escreva os INSERT/SELECT/UPDATE/DELETE para criar, listar, alterar e
   remover um produto.
3. Escreva uma query com `where` e `order by`.
4. Escreva uma query com `count(*)` e uma com `join`.