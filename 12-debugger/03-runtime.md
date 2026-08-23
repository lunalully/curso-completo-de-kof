# Trilha 12 · Aula 3 — Erros de runtime

## Os erros de runtime comuns

| Erro | Ocorre quando | Mensagem típica |
|------|---------------|-----------------|
| Divisão por zero | `x / 0` | `/ by zero` |
| Array out of bounds | `arr[i]` com `i` fora | `Index N out of bounds for length M` |
| Null pointer | acesso a objeto null | `Cannot invoke ... because ... is null` |

Todos podem ser capturados com `try/catch (String e)`:

```kof
try {
    var arr = new Int[2]
    arr[5] = 1
} catch (String e) {
    println("erro: " + e)
}
```

## Propriedade

Em Kof, exceções são **Strings** e **propagam pelos frames** — a função que
chama a que quebrou vê o erro no `catch`:

```kof
void nivel3() { throw "falhou no nivel 3" }
void nivel2() { nivel3() }
main() {
    try {
        nivel2()
    } catch (String e) {
        println("capturado no topo: " + e)
    }
}
```

## Debug de null

`readFile` de arquivo inexistente retorna `null` — **verifique antes de usar**:

```kof
var c = readFile("x.txt")
if (c == null) {
    println("trate antes de usar")
} else {
    println(c)
}
```

## Exercício

Capture os 3 erros acima num programa e imprima as mensagens (veja a solução
`03-erros.kf`).