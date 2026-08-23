# Trilha 14 · Exercícios

> ★ conceitual · ★★ prático · ★★★ aplicado.

## Princípios (aula 01)

- ★ Explique com suas palavras os 4 princípios da clean architecture preservados por Kof.
- ★★ Liste 3 coisas que Kof remove (DI, AOP, annotations) e o que as substitui.

## Domínio (aula 02)

- ★★ [01-reservas.kf](solucoes/01-reservas.kf) — domínio de reservas: `record Reserva`, `class Salas`, `conflita()`.
- ★★ [02-biblioteca.kf](solucoes/02-biblioteca.kf) — domínio de biblioteca com `Livro` (dados), `Acervo` (comportamento), `buscarPorTitulo` (lógica).

## Módulos (aula 03)

- ★★ crie 3 arquivos com `package dominio`, `package aplicacao`, `package infra` e compile o conjunto.
- ★★★ explique a direção de dependência e por que `infra` importa `aplicacao` e não o contrário.

## Hexagonal (aula 04)

- ★★★ [03-hexagonal.kf](solucoes/03-hexagonal.kf) — interface `Armazenamento` + adapter em arquivo + composição no `main()`.
- ★★★ [04-hexagonal-db.kf](solucoes/04-hexagonal-db.kf) — o mesmo port com adapter `kof.db` (requer driver JDBC).

## Gaps (aula 05)

- ★★ escreva o "manifesto" de por que Kof não precisa de container de DI.
- ★★★ refatore um Controller/Service/Repository trivial para a forma Kof.

## Desafio integrador

[05-integrador.kf](solucoes/05-integrador.kf) — **sistema de reservas completo**:
1. `dominio`: `Reserva`, `Salas`, `conflita()`.
2. `aplicacao`: `criarReserva` (valida e adiciona).
3. `infra`: persistência em arquivo (adapter).
4. `main()`: compõe tudo sem container e expõe uma rota `web.app()`.

Compila e roda → trilha concluída.