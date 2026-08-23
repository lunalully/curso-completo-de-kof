# Trilha 08 · Exercícios

> ★ básico · ★★ intermediário · ★★★ avançado (refatorações).

## Filosofia e idioms (aulas 01-02)

- ★ Para cada intenção abaixo, escreva o código Kof: spawn, rota web, JSON decode, cor nomeada.
- ★★ [01-idioms.kf](solucoes/01-idioms.kf) — implemente os idioms BAD→GOOD: if-expr, `==` em strings, `List<T>`, função top-level, exceção vs sentinela.
- ★★★ [02-refatorar.kf](solucoes/02-refatorar.kf) — receba um "código Java-like" (getters/setters, utility class, sentinelas) e reescreva idiomático.

## Anti-patterns (aula 03)

- ★ Liste os 8 anti-patterns do curso com um exemplo real de cada.
- ★★ [03-detecte.kf](solucoes/03-detecte.kf) — escreva versões BAD (estado duplicado, sentinela, camadas de cerimônia) e a versão GOOD ao lado.
- ★★★ audite um código seu com o checklist do módulo 09 aula 8.

## Testes (aula 04)

- ★★ [04-testes.kf](solucoes/04-testes.kf) — arquivo de teste com `assert` para: soma, capitalizar, busca binária, pilha, palíndromo. Rode `kof test`.
- ★★★ [05-testes-excecao.kf](solucoes/05-testes-excecao.kf) — teste divisão por zero e pilha vazia usando `try/catch` + `assert`.

## Config (aula 05)

- ★★ [06-config.kf](solucoes/06-config.kf) — leia `config.str/int/bool/has` com fallbacks.
- ★★★ [07-config-env.kf](solucoes/07-config-env.kf) — sobrescreva por env `KOF_<KEY>` e por `KOF_PROFILE` (crie `kof.prod.config`).

## Logging (aula 06)

- ★★ [08-log.kf](solucoes/08-log.kf) — os 4 níveis + `KOF_LOG_LEVEL` variado.
- ★★★ [09-log-seguro.kf](solucoes/09-log-seguro.kf) — logue uma chave com `secrets.redact` (requer módulo 04).

## Desafio integrador

[10-integrador.kf](solucoes/10-integrador.kf) — biblioteca de utilidades idiomática:
- funções top-level (nada de utility class),
- `record` + classe com estado,
- erros com exceção (nada de sentinela),
- testes com `assert` para tudo,
- config + log no `main`.