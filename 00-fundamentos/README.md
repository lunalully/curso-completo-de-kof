# Trilha 00 — Fundamentos de Kof

> **Trilha isolada.** Comece aqui. Sem pré-requisitos. Ao terminar, você
> escreve, roda e debuga programas Kof básicos com fluência.

## O que você vai dominar

| Aula | O que você aprende | Nível da trilha |
|------|--------------------|-----------------|
| [01-introducao.md](01-introducao.md) | O que é Kof, filosofia, CLI | entrada |
| [02-sintaxe-e-tipos.md](02-sintaxe-e-tipos.md) | declarações, tipos, operadores, strings, arrays, **tipos anuláveis** | base |
| [03-controle-de-fluxo.md](03-controle-de-fluxo.md) | if-expr, loops, for-in, switch, **pattern matching** | base |
| [04-funcoes-e-lambdas.md](04-funcoes-e-lambdas.md) | funções top-level, lambdas, **parâmetros padrão**, limites | base |
| [05-classes-records.md](05-classes-records.md) | classes, records, herança, interfaces, JSON | base |
| [06-colecoes.md](06-colecoes.md) | `List<T>`, `listOf`, iteração, **map/filter/reduce** | base |
| [07-erros.md](07-erros.md) | exceções (Strings), try/catch/finally | base |
| [08-json-io.md](08-json-io.md) | JSON, `kof.io`, `kof.time` (**scheduler every/at**, **readLine**) | base |

## Como estudar

1. Leia as aulas na ordem. **Rode todos os exemplos** (`kof run`).
2. Faça os [exercícios](exercicios.md) por nível: ★ → ★★ → ★★★.
3. Confira as [soluções](solucoes/) **depois** de tentar.
4. Ao final, passe no **checkpoint** abaixo.

## Ambiente

```bash
kof version          # 0.2.6-beta (release)
kof info             # ambiente completo
kof run aula.kf      # roda (--target jvm|native|js|native.risc|native.arm)
kof check aula.kf    # type-check rápido
```

## Checkpoint (avalie-se)

Escreva um único programa que:
1. Declara um `record Produto(String nome, Double preco)`.
2. Declara uma classe `Carrinho` com `List<Produto>` e método `total()`.
3. Tem uma função top-level `formatar(Double v): String`.
4. Usa **if-expr**, `for-in`, **exceção** quando o carrinho está vazio.
5. Salva e lê a lista em `carrinho.txt` (kof.io) e serializa um produto com JSON.

Rode `kof run` e `kof check`. Sem erros → **trilha concluída**.

## Certificado

- Básico: ★ e ★★ de todos os módulos das aulas.
- Intermediário: ★★★ + checkpoint.
- Siga para a [Trilha 01](../01-algoritmos/README.md).