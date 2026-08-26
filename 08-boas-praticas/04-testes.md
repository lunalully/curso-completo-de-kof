# Módulo 08 · Aula 4 — Testes

> Testar é parte da plataforma: `assert` + blocos `test "nome" { }` +
> `kof test` — sem framework externo.

## assert

```kof
assert(condicao, "mensagem de falha")
```

O programa sai com exit code ≠ 0 quando o assert falha.

## Blocos `test` (0.1.0)

```kof
test "soma simples" {
    assert(2 + 2 == 4)
}

test "string igual" {
    assert("kof" == "kof", "strings iguais")
}

main() { /* ignorado pelo kof test */ }
```

```bash
kof test testes.kf              # PASS soma simples / PASS string igual
kof test <arquivo.kf|dir> --target jvm|native|js
```

- Cada bloco vira uma função em compile-time (runner sintetizado, zero
  reflection) e roda **isolado** (try/catch por teste).
- Saída: `PASS nome` / `FAIL nome: mensagem` + resumo; exit code ≠ 0 se
  houver falha.
- Arquivos sem blocos `test` mantêm o contrato antigo (PASS/FAIL pelo exit
  code do programa inteiro).
- `process.exit(code)` termina o processo com código específico.

## kof test

```bash
kof test <arquivo.kf|dir> [--target jvm|native|js]
```

Roda os blocos `test` do arquivo (ou de todos os `.kf` da pasta).

## Um arquivo de teste

```kof
// testes.kf
record User(String name, Int age)

Int soma(Int a, Int b) {
    return a + b
}

String capitalizar(String s) {
    return s.substring(0, 1).toUpperCase() + s.substring(1)
}

test "soma" {
    assert(soma(2, 3) == 5, "2+3 deve ser 5")
    assert(soma(-1, 1) == 0, "soma de opostos")
}

test "capitalizar" {
    assert(capitalizar("kof") == "Kof", "capitalizar")
    assert(capitalizar("kof") != "kof", "nao deve ser lowercase")
}

test "record" {
    var u = User("Mel", 26)
    assert(u.name == "Mel", "campo do record")
    assert(u.age == 26, "idade")
}
```

```bash
kof test testes.kf      # 3 passed of 3 tests
```

## Convenção de organização

- Um arquivo de teste por módulo/domínio (`testes/<nome>-test.kf`).
- Blocos `test "nome" { }` com asserts — um conceito por bloco.
- Rode com `kof test` no CI (o pipeline do Kof roda `mvn clean test`).

## Testando exceções

```kof
Bool lancaErro(Void fn) {
    // WORKAROUND: Kof não tem lambdas de Void genéricas confiáveis
    // — teste a exceção no fluxo direto
    return false
}

main() {
    try {
        dividir(1, 0)
        assert(false, "deveria ter lancado")
    } catch (String e) {
        assert(true, "lancou como esperado")
    }
}
```

> O padrão seguro: envolver a chamada em `try/catch` e assertar o
> comportamento observado.

## Exercícios

1. Escreva testes para: busca binária, bubble sort, pilha, `capitalizar`.
2. Teste um caso de exceção (divisão por zero, pilha vazia).
3. Crie uma pasta `testes/` e rode `kof test` na pasta toda.
4. Adicione um assert de segurança: `passwords.verify` com senha errada.