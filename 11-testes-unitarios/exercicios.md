# Trilha 11 · Exercícios

> ★ básico · ★★ intermediário · ★★★ avançado. Rode tudo com `kof test`.

## Fundamentos (aula 01)

- ★ [01-soma-test.kf](solucoes/01-soma-test.kf) — testes de `soma` e `multiplica`.
- ★ [02-string-test.kf](solucoes/02-string-test.kf) — `capitalizar`, `palindromo`, `contarVogais`.
- ★★ [03-pilha-test.kf](solucoes/03-pilha-test.kf) — pilha: LIFO, topo, vazia, desempilhar vazio (exceção).

## Casos de borda (aula 02)

- ★★ [04-busca-test.kf](solucoes/04-busca-test.kf) — busca binária: alvo no início/fim/meio, ausente (exceção).
- ★★ [05-limites-test.kf](solucoes/05-limites-test.kf) — fatorial 0, 1, e valores grandes; divisão por zero.
- ★★★ [06-carrinho-test.kf](solucoes/06-carrinho-test.kf) — `Carrinho` (módulo 00): adicionar, total, vazio lança.

## Organização (aula 03)

- ★★★ crie `testes/` com 3 arquivos e rode `kof test testes/`.

## TDD (aula 04)

- ★★★ escolha uma função simples (ex.: `ehPrimo`), escreva o teste **primeiro** (veja falhar), implemente, faça passar.

## Desafio integrador

[07-integrador-test.kf](solucoes/07-integrador-test.kf) — suíte para um pequeno
**sistema de estoque**: `record Produto(id, nome, qtd)`, classe `Estoque`
(adicionar, remover, consultar) — com testes de: operações normais, borda
(qtd 0), e exceções (produto inexistente). `kof test` → PASS.