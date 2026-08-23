# Trilha 14 — Arquitetura com Kof

> **Trilha isolada.** Pré-requisito: Trilhas 00, 06, 08, 13. Ao terminar, você
> projeta sistemas seguindo a filosofia Kof — *domínio primeiro, complexidade
> na plataforma, camadas só quando resolvem problema real*.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-aulas.md](01-aulas.md) | princípios, domínio, módulos, hexagonal, gaps |

## Como estudar

1. Leia as aulas na ordem.
2. Faça os [exercícios](exercicios.md).
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

1. Modele um domínio (ex.: reservas) com records + classes + funções top-level.
2. Separe em pacotes: `dominio`, `aplicacao`, `infra`.
3. Explique (escrito) por que Kof não precisa de container de DI.
4. Refatore um "código com camadas de cerimônia" (Controller/Service/Repo sem
   necessidade) para a forma Kof.

Domínio modelado + refatoração → trilha concluída.