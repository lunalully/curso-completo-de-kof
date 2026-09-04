# Trilha 15 · Exercícios

> ★ básico · ★★ intermediário · ★★★ avançado.

## Build (aula 01)

- ★ [01-hello-multi.kf](solucoes/01-hello-multi.kf) — compile para `jvm`, `native` e `js`; verifique a saída de cada um.
- ★★ [02-multi-target.kf](solucoes/02-multi-target.kf) — programa com `kof.io`; compile para jvm E native e rode os dois binários.
- ★★★ compile um programa que usa `spawn` para native e documente que funciona via pthread (CONC001 fechado, 0.2.8-beta).

## CI (aula 02)

- ★★ [03-workflow.yml](solucoes/03-workflow.yml) — escreva um workflow GitHub Actions: build + `kof check` + `kof test`.
- ★★★ adapte o workflow para rodar os testes nos targets jvm e native.

## Release (aula 03)

- ★★ documente o fluxo: editar `VERSION` → `bump-version.sh` → `package.sh` → tag → release.
- ★★★ [04-pipeline.md](solucoes/04-pipeline.md) — desenhe a pipeline completa do projeto Kof (do commit à release), com os scripts usados.

## Containers (aula 04)

- ★★ escreva um `Dockerfile` multi-stage para um serviço `web.app()`.
- ★★★ [05-dockerfile](solucoes/05-dockerfile) — Dockerfile + comando de run para o serviço de pedidos (módulo 13).

## Observabilidade (aula 05)

- ★★ [06-health.kf](solucoes/06-health.kf) — serviço com `/health` + `kof.log` + `KOF_LOG_LEVEL` configurável.

## Desafio integrador

[07-integrador.kf](solucoes/07-integrador.kf) — **projeto pronto para deploy**:
1. Um serviço Kof com rota de negócio + `/health`.
2. `kof.config` para porta (env no container).
3. Testes (`kof test`) que passam.
4. Workflow CI (check + test) + Dockerfile.
5. Documente a estratégia de release (VERSION → tag → artefato).

Tudo compilando e documentado → **trilha concluída — você completou o curso**.