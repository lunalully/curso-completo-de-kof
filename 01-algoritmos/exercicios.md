# Trilha 01 · Exercícios

> ★ básico · ★★ intermediário · ★★★ avançado. Rode `kof check` + `kof run`.

## Busca (aula 02)

- ★ [01-busca-linear.kf](solucoes/01-busca-linear.kf) — `buscarIndice(List<Int>, alvo)` e `contem(List<Int>, alvo)`.
- ★★ [02-busca-binaria.kf](solucoes/02-busca-binaria.kf) — `buscaBinaria` iterativa e recursiva.
- ★★ [03-ultimo-indice.kf](solucoes/03-ultimo-indice.kf) — `ultimoIndice(lista, alvo)`.
- ★★ `contar(lista, alvo)` — quantas vezes o alvo aparece.
- ★★★ [04-busca-string.kf](solucoes/04-busca-string.kf) — busca binária em `List<String>` (compare com `<`).

## Ordenação (aula 03)

- ★ [05-bubble.kf](solucoes/05-bubble.kf) — bubble sort com troca.
- ★★ [06-selection.kf](solucoes/06-selection.kf) — selection sort.
- ★★ [07-insertion.kf](solucoes/07-insertion.kf) — insertion sort.
- ★★★ [08-merge.kf](solucoes/08-merge.kf) — merge sort (dividir e conquistar).
- ★★★ [09-bubble-otimizado.kf](solucoes/09-bubble-otimizado.kf) — bubble que para cedo quando já está ordenado (flag).

## Recursão (aula 04)

- ★ [10-fatorial.kf](solucoes/10-fatorial.kf) — recursivo e iterativo.
- ★★ [11-fib.kf](solucoes/11-fib.kf) — recursivo, iterativo e com memoização (lista acumuladora).
- ★★ [12-inverter-string.kf](solucoes/12-inverter-string.kf) — recursivo.
- ★★★ [13-mdc.kf](solucoes/13-mdc.kf) — algoritmo de Euclides recursivo.

## Complexidade (aula 05)

- ★ [14-classifique.kf](solucoes/14-classifique.kf) — escreva funções O(1), O(n), O(n²), O(log n) e documente cada uma.
- ★★ [15-medir.kf](solucoes/15-medir.kf) — cronometre bubble vs merge para n=2000/5000 com `now()`.
- ★★★ [16-potencia.kf](solucoes/16-potencia.kf) — `potencia(base, exp)` ingênua (O(n)) e por **exponenciação rápida** (O(log n)), e prove a diferença de passos.

## Desafio integrador

[17-integrador.kf](solucoes/17-integrador.kf) — implemente `record Aluno(String nome, Int nota)`,
ordene uma `List<Aluno>` por nota (merge adaptado) e encontre o aluno com a
maior nota usando busca. Conte quantos passos cada parte faz.