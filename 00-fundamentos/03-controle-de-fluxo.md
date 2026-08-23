# Módulo 00 · Aula 3 — Controle de Fluxo

> Condicionais, loops e o **if como expressão** — um dos idiomas favoritos de Kof.

## if / else

```kof
if (x > 5) {
    println("maior")
} else {
    println("menor")
}
```

## if como expressão (idiomático)

```kof
var status = if (ativo) "online" else "offline"
```

O if-expr **produz um valor**; os dois branches devem produzir valores
compatíveis. Evite declarar variável e mutar dentro de branches:

```kof
// RUIM — mutação intermediária
var status = ""
if (ativo) {
    status = "online"
} else {
    status = "offline"
}

// BOM — expressão
var status = if (ativo) "online" else "offline"
```

## Loops

```kof
// while
var i = 0
while (i < 5) {
    println(i)
    i = i + 1
}

// for clássico
for (var j = 0; j < 3; j = j + 1) {
    println(j)
}

// do-while (roda pelo menos uma vez)
do {
    println("ao menos uma vez")
} while (condicao())
```

## for-in (coleções e arrays)

```kof
var items = listOf("a", "b", "c")
for (var item in items) {
    println(item)
}

var nums = new Int[3]
nums[0] = 5
for (var n in nums) {
    println(n)
}
```

> Use `for-in` sempre que a ordem dos índices não for necessária. Índice
> manual sem necessidade é esforço desperdiçado.

## switch

```kof
switch (x) {
    case 1:
        println("um")
        break
    case 2:
        println("dois")
        break
    default:
        println("outro")
}
```

## break / continue

`break` sai do loop; `continue` pula para a próxima iteração. Usados com
parcimônia — a intenção geralmente é um `for-in`.

## Exercícios

1. Imprima todos os pares de 0 a 100 com `for`.
2. Imprima a tabuada do 7 com `while`.
3. Dado um `Int n`, devolva `"par"` ou `"impar"` usando **if-expr**.
4. Some os elementos de um array com `for-in`.