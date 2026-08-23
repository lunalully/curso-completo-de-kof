# Módulo 08 · Aula 4 — Testes

> Testar é parte da plataforma: `assert` + `kof test` — sem framework externo.

## assert

```kof
assert(condicao, "mensagem de falha")
```

O programa sai com exit code ≠ 0 quando o assert falha.

## kof test

```bash
kof test <arquivo.kf|dir> [--target jvm|native]
```

Roda o programa; **PASS se exit code 0**, FAIL caso contrário. Ideal para
arquivos de teste com vários `assert`.

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

main() {
    assert(soma(2, 3) == 5, "2+3 deve ser 5")
    assert(soma(-1, 1) == 0, "soma de opostos")
    assert(capitalizar("kof") == "Kof", "capitalizar")
    assert(capitalizar("kof") != "kof", "nao deve ser lowercase")

    var u = User("Mel", 26)
    assert(u.name == "Mel", "campo do record")
    assert(u.age == 26, "idade")

    println("todos os testes passaram")
}
```

```bash
kof test testes.kf      # PASS (exit 0)
```

## Convenção de organização

- Um arquivo de teste por módulo/domínio (`testes/<nome>-test.kf`).
- `main()` com asserts e um `println` final de sucesso.
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