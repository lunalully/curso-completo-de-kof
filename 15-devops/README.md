# Trilha 15 — DevOps com Kof

> **Trilha isolada (última).** Pré-requisito: todas as anteriores. Ao terminar,
> você automatiza build, teste, empacotamento e release de software Kof — com
> a filosofia: *build, teste e release são capacidades da plataforma*.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-aulas.md](01-aulas.md) | build multi-target, CI, release, containers, observabilidade |

## Estado real

| Ferramenta | Estado |
|------------|--------|
| `kof build/run/serve/check/test` | ✅ |
| `kof bench`, `kof profile` | ✅ tooling |
| `kof debug`, `kof lsp`, `kof info` | ✅ |
| `scripts/package.sh`, `bump-version.sh`, `changelog.sh`, `build-webview.sh` | ✅ no repo |
| GitHub Actions (build+test no PR; release por tag) | ✅ no repo |
| `VERSION` + `<revision>` no pom.xml | ✅ fonte única |

## Como estudar

1. Leia as aulas.
2. Faça os [exercícios](exercicios.md).
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

1. Compile um projeto para os 3 targets (`jvm`, `native`, `js`) e verifique a saída.
2. Monte um workflow de CI (build + `kof test`) para GitHub Actions.
3. Execute `scripts/package.sh`-like (empacote seu projeto num tarball).
4. Documente a estratégia de release (VERSION → tag → artefato).

Build multi-target + pipeline documentada → trilha concluída.