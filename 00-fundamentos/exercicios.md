# Trilha 00 · Exercícios

> Níveis: ★ básico · ★★ intermediário · ★★★ avançado. Tente **antes** de
> ver as [soluções](solucoes/). Regra: rode `kof check` + `kof run` em tudo.

## 1. Introdução (aula 01)

- ★ [01-hello.kf](solucoes/01-hello.kf) — imprima "Olá, Kof!" de três formas: `println`, concatenação de duas strings, e uma variável.
- ★ Rode `kof info` e explique 3 campos da saída com suas palavras.

## 2. Sintaxe e tipos (aula 02)

- ★ [02-vogais.kf](solucoes/02-vogais.kf) — função `vogais(String): Int` que conta vogais (use `charAt` e compare com 97/101/105/111/117 — 'a','e','i','o','u').
- ★ [03-palindromo.kf](solucoes/03-palindromo.kf) — função `palindromo(String): Bool` (ignore maiúsculas com `toLowerCase`).
- ★★ [04-quadrados.kf](solucoes/04-quadrados.kf) — array de 10 `Int` com os quadrados de 1..10, imprima a soma.
- ★★ função `contemCaractere(String s, Char c): Bool` usando loop + `charAt`.
- ★★★ [05-numeros-primos.kf](solucoes/05-numeros-primos.kf) — `primosAte(Int n): List<Int>` (crivo de Eratóstenes).

## 3. Controle de fluxo (aula 03)

- ★ [06-pares.kf](solucoes/06-pares.kf) — imprima os pares de 0 a 100.
- ★ [07-tabuada.kf](solucoes/07-tabuada.kf) — tabuada do 7 com `while`.
- ★ [08-par-impar.kf](solucoes/08-par-impar.kf) — `"par"`/`"impar"` com **if-expr**.
- ★★ some os elementos de um array com `for-in`.
- ★★ [09-contagem-regressiva.kf](solucoes/09-contagem-regressiva.kf) — `do-while` que imprime de 10 até 0 e o total de iterações.
- ★★★ FizzBuzz (1..100): "Fizz" múltiplo de 3, "Buzz" de 5, "FizzBuzz" de 15 — use if-expr e um `List<String>`.

## 4. Funções e lambdas (aula 04)

- ★ [10-max-min.kf](solucoes/10-max-min.kf) — `max(a,b)` e `min(a,b)`.
- ★ [11-fatorial-for.kf](solucoes/11-fatorial-for.kf) — fatorial com `for`.
- ★★ [12-lambda.kf](solucoes/12-lambda.kf) — lambda `(x: Int) -> x * 2` e outra `(a,b) -> a+b`.
- ★★ refatore um `class MathUtils { static ... }` para funções top-level.
- ★★★ [13-criptografia-cep.kf](solucoes/13-criptografia-cep.kf) — "cifra de César": `cifrar(String, Int deslocamento): String` usando `charAt` + aritmética de caracteres.

## 5. Classes, records e interfaces (aula 05)

- ★ [14-livro.kf](solucoes/14-livro.kf) — `record Livro(String titulo, String autor, Int ano)`.
- ★★ [15-biblioteca.kf](solucoes/15-biblioteca.kf) — `class Biblioteca` com `List<Livro>`, `adicionar`, `tamanho`, `buscarPorTitulo`.
- ★★ [16-animais.kf](solucoes/16-animais.kf) — `Animal`/`Cachorro`/`Gato` com `speak()` virtual + interface `Speaker`.
- ★★★ refatore uma classe com getters/setters para record/campos públicos.
- ★★★ [17-json-record.kf](solucoes/17-json-record.kf) — serialize `Livro` com `json.encode`, decodifique de volta.

## 6. Coleções (aula 06)

- ★ [18-soma-lista.kf](solucoes/18-soma-lista.kf) — soma de `listOf(1..100)`.
- ★ [19-contem.kf](solucoes/19-contem.kf) — `contem(lista, alvo): Bool`.
- ★★ [20-frequencia.kf](solucoes/20-frequencia.kf) — frequência de letras com `List<record Letra(Char c, Int n)>`.
- ★★ `removerDuplicatas(List<Int>): List<Int>` (List + contains).
- ★★★ [21-ordenar-manual.kf](solucoes/21-ordenar-manual.kf) — implemente bubble sort sobre `List<Int>`.

## 7. Erros (aula 07)

- ★ [22-divisao.kf](solucoes/22-divisao.kf) — `dividir(a,b)` lança `"divisao por zero"` se `b == 0`; capture e imprima.
- ★★ [23-find-excecao.kf](solucoes/23-find-excecao.kf) — `find(lista, alvo)` lança `"nao encontrado: X"`; trate com try/catch.
- ★★ refatore um retorno `-1`/`""` (sentinela) para exceção.
- ★★★ [24-finally.kf](solucoes/24-finally.kf) — prove que `finally` roda nos 3 caminhos (normal, catch, propagação).

## 8. JSON e kof.io (aula 08)

- ★ [25-salvar-ler.kf](solucoes/25-salvar-ler.kf) — salve `"Mel\nKof\n"` em `nomes.txt`, leia e imprima.
- ★★ [26-contador-palavras.kf](solucoes/26-contador-palavras.kf) — conte palavras de um arquivo (`split(" ")`).
- ★★ [27-diretorio.kf](solucoes/27-diretorio.kf) — liste o conteúdo de um diretório.
- ★★★ [28-agenda-json.kf](solucoes/28-agenda-json.kf) — agenda de contatos (record) persistida em JSON num arquivo.

## Desafio integrador

[29-integrador.kf](solucoes/29-integrador.kf) — combine tudo:
`record Tarefa(Int id, String titulo, Bool concluida)` + classe com `List<Tarefa>` +
funções top-level (`concluir`, `pendentes`) + persistência em `tarefas.txt` +
leitura com `kof.io` + exceção para tarefa inexistente.

## Como usar as soluções

```bash
cd solucoes
kof run 29-integrador.kf
```

Cada solução compila e roda no compilador real (0.2.6-beta). Estude a solução
**após** tentar — e reescreva com suas palavras.