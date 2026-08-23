# Módulo 01 — Algoritmos

> **Objetivo:** implementar algoritmos clássicos em Kof, usando apenas o que a
> linguagem oferece hoje (`List<T>`, arrays, `for-in`, if-expr).

## A filosofia aplicada a algoritmos

1. **Escreva a versão idiomática primeiro.** Não micro-otimize antes de medir.
2. **Represente o domínio.** O problema é "buscar numa lista" — a abstração
   (`List<T>`, `contains`) existe.
3. **Meça antes de otimizar** — e só otimize o ponto medido, com comentário
   explicando por quê.

## O que este módulo cobre

| Aula | Tema |
|------|------|
| [01-intro.md](01-intro.md) | O que é um algoritmo e como pensar |
| [02-busca.md](02-busca.md) | Busca linear e binária |
| [03-ordenacao.md](03-ordenacao.md) | Bubble, selection, insertion e merge sort |
| [04-recursao.md](04-recursao.md) | Recursão: fatorial, Fibonacci, dividir e conquistar |
| [05-complexidade.md](05-complexidade.md) | Notação Big-O e análise |

## Convenções do módulo

- Todo algoritmo é escrito como **função top-level** (lógica sem estado).
- Para sequências dinâmicas usamos `List<T>`; para acesso indexado de
  tamanho fixo, arrays.
- **Não existe `sort()` embutido nem `map`/`filter`** — implementamos à mão
  para aprender e porque a stdlib ainda não oferece.
- Resultados sempre verificáveis: rode cada exemplo com `kof run`.

## Primeiro exemplo

```kof
// soma de 1..n — iterativa e idiomática
Int somarAte(Int n) {
    var total = 0
    for (var i = 1; i <= n; i = i + 1) {
        total = total + i
    }
    return total
}

main() {
    println(somarAte(100))   // 5050
}
```