# Trilha 12 · Exercícios

> ★ básico · ★★ intermediário · ★★★ avançado.

## Mentalidade (aula 01)

- ★ Explique a diferença entre: erro de compilação, erro de runtime e bug lógico (com um exemplo de cada).
- ★★ Escreva um programa que compila mas tem um bug de lógica (ex.: soma errada) — sem olhar a saída, preveja o que vai imprimir.

## CLI de depuração (aula 02)

- ★★ [01-bug-indice.kf](solucoes/01-bug-indice.kf) — programa com índice fora do intervalo; rode `kof run` e leia a mensagem.
- ★★ [02-bug-null.kf](solucoes/02-bug-null.kf) — acesso a null (ex.: `readFile` de arquivo inexistente) e tratamento.
- ★★★ rode `kof debug` num programa e registre: breakpoint por linha, continue, call stack.

## Erros de runtime (aula 03)

- ★★ [03-erros.kf](solucoes/03-erros.kf) — capture e imprima a mensagem de: divisão por zero, array out of bounds, null pointer.
- ★★★ [04-trace.kf](solucoes/04-trace.kf) — programa com funções aninhadas que lança; observe o stack no runtime.

## Ferramentas (aula 04)

- ★★ [05-check-test.kf](solucoes/05-check-test.kf) — programa com testes; rode `kof check` (deve aceitar) e `kof test` (PASS).
- ★★★ use `kof lsp` num editor e observe os diagnostics aparecerem em tempo real.

## Desafio integrador

[06-integrador.kf](solucoes/06-integrador.kf) — **encontre o bug**:
o programa abaixo tem um bug lógico (a média está errada). Depure:
1. Rode e observe a saída errada.
2. Adicione logs (`log.debug`) nos pontos-chave.
3. Corrija e valide com `kof test`.

```kof
Double media(List<Int> dados) {
    var soma = 0
    for (var i = 0; i < dados.size; i = i + 1) {
        soma = soma + dados.get(i)
    }
    return soma / dados.size - 1   // bug: subtração indevida
}
```