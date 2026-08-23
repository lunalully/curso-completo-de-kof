# Trilha 02 · Exercícios

> ★ básico · ★★ intermediário · ★★★ avançado.

## Arrays (aula 01)

- ★ [01-inverter-array.kf](solucoes/01-inverter-array.kf) — inverta `Int[]` in-place.
- ★ [02-maior-menor.kf](solucoes/02-maior-menor.kf) — maior e menor em uma passada.
- ★★ [03-pares-array.kf](solucoes/03-pares-array.kf) — conte os pares.
- ★★ [04-copiar-array.kf](solucoes/04-copiar-array.kf) — copie e prove independência.
- ★★★ [05-soma-matriz.kf](solucoes/05-soma-matriz.kf) — matriz `List<List<Int>>`, some a diagonal.

## List (aula 02)

- ★ [06-reverter-lista.kf](solucoes/06-reverter-lista.kf) — reverta uma `List<Int>`.
- ★ [07-unico.kf](solucoes/07-unico.kf) — remova duplicatas (List + contains).
- ★★ [08-frequencia-lista.kf](solucoes/08-frequencia-lista.kf) — `List<Entry(char, Int)>` de frequência.
- ★★ [09-particionar.kf](solucoes/09-particionar.kf) — pares e ímpares em duas listas.
- ★★★ [10-map-associacao.kf](solucoes/10-map-associacao.kf) — classe `Dicionario` com `List<Entry>` (put/get/contem).

## Pilha (aula 03)

- ★ [11-pilha.kf](solucoes/11-pilha.kf) — pilha de Int completa (empilhar/desempilhar/topo/vazia).
- ★★ [12-balanceado.kf](solucoes/12-balanceado.kf) — parênteses balanceados (40/41).
- ★★ [13-decimal-binario.kf](solucoes/13-decimal-binario.kf) — decimal → binário com pilha.
- ★★★ [14-avaliar-posfixa.kf](solucoes/14-avaliar-posfixa.kf) — avaliador RPN (`"3 4 +"` → 7).

## Fila (aula 04)

- ★ [15-fila.kf](solucoes/15-fila.kf) — fila FIFO com List.
- ★★ [16-fila-circular.kf](solucoes/16-fila-circular.kf) — fila circular O(1) sobre array.
- ★★ [17-buffer-teclas.kf](solucoes/17-buffer-teclas.kf) — buffer de 5 teclas com fila circular.
- ★★★ [18-bfs-fila.kf](solucoes/18-bfs-fila.kf) — BFS de um grafo usando fila.

## Grafos (aula 05)

- ★★ [19-grafo.kf](solucoes/19-grafo.kf) — `Grafo` com `List<Aresta>`: vertices, arestas, vizinhos, saoVizinhos.
- ★★★ [20-dfs.kf](solucoes/20-dfs.kf) — DFS com pilha.
- ★★★ [21-componentes.kf](solucoes/21-componentes.kf) — conte componentes conexas.
- ★★★ [22-caminho.kf](solucoes/22-caminho.kf) — `conectados(a, b)` via DFS.

## Hash (aula 06)

- ★★ [23-tabela.kf](solucoes/23-tabela.kf) — `Tabela` com put/get/contem/tamanho.
- ★★★ [24-tabela-balde.kf](solucoes/24-tabela-balde.kf) — hash por primeira letra (26 baldes).
- ★★★ [25-frequencia-palavras.kf](solucoes/25-frequencia-palavras.kf) — frequência de palavras de uma string com Tabela.

## Desafio integrador

[26-integrador.kf](solucoes/26-integrador.kf) — a **agenda de contatos** do
checkpoint: busca por nome + pilha de desfazer + fila de notificações +
índice hash por letra inicial. Tudo em um programa que roda e demonstra cada
estrutura.