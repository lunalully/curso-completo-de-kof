# Trilha 15 · Aulas

## Aula 01 — Build multi-target

```bash
kof build app.kf --target jvm          --output out-jvm [--fat] [--print-sizes]
kof build app.kf --target native       --output out-native
kof build app.kf --target native.risc  --output out-risc
kof build app.kf --target native.arm   --output out-arm
kof build app.kf --target js           --output out-js
kof build app.kf --target android --apk --min-sdk 26 --target-sdk 35
kof run app.kf [--target ...] [--release] [--profile host|freestanding]
kof check app.kf|dir [--target t] [--json]  # type-check rápido (CI)
kof test testes/ [--target jvm|native|js] [--tag t] [--timeout s]
kof bench [paths...] [--baseline f] [--fail-on-regression]
kof profile app.kf [--target jvm|native|js] [--methods]
kof inspect app.kf [--json]           # estatísticas IR
kof fmt app.kf [-w]                   # formatador (parser real, idempotente)
kof md check|format a.md              # Kofmd (MD002)
```

- O **mesmo código** gera JVM bytecode, ELF nativo x86-64/RISC-V/ARM64, ES Modules, APK Android e C subset (`kof c` agora também com `--target riscv64|aarch64`).
- Gaps de target (`DB001`, `SECN00x`, `WEB001`, `ORM001`, `AND001`...) são reportados
  em compile-time — o build falha com diagnóstico claro, nunca silencioso.
- `--release` remove metadata de debug (SourceFile/LineNumberTable no JVM, source map no JS).
- `--fat` embute dependências/drivers no artefato JVM; `--print-sizes` mostra o custo por artefato.
- No CI: compile todos os targets para **provar** que o código é multi-target.

## Aula 01b — Deploy oficial (0.4.5+)

O `kof deploy` empacota release de verdade (fat jar + manifesto + checksums +
tar.gz) e, na 0.5.0, também os **fontes** do projeto:

```bash
kof deploy dist --target jvm --name meu-servico --version 1.2.0
kof deploy dist --target native --publish    # publica GitHub Release (0.4.7+)
```

- Multi-target desde a 0.4.6; `--target js` gera pacote self-contained (0.4.5).
- Fontes viajam entre o artefato e o `RELEASE.md` (#566, 0.5.0) — o tar.gz de
  deploy não carrega mais entradas acidentais.
- Pacotes `kofdeps` do registry entram com `kof deps add owner/repo[@ver]`
  (GitHub Releases) e a proveniência é verificável:
  `kof deps resolve` + `KOF_DEPS_TRUST=enforce` (`REG005-008`).

## Aula 02 — Integração contínua (GitHub Actions)

Padrão do projeto Kof (`.github/workflows/`):

```yaml
name: build-test
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with: { distribution: temurin, java-version: '25' }   # toolchain do repo
      - uses: actions/setup-python@v5   # mvn pode ser instalado pelo script
      - run: mvn -q clean package -DskipTests
      - run: bin/kof version
      - run: bin/kof check src/            # type-check
      - run: bin/kof test testes/ --timeout 60   # testes com limite
      - run: bin/kof test testes/ --tag smoke    # suíte rápida opcional
```

Regras do projeto:
- `main` nunca aponta para estado que não compila.
- Release só se `mvn clean test` e `mvn clean package` passarem.
- PR: build + testes + verificações estáticas.
- A partir da 0.5.0 a pipeline oficial separa `test-and-bump` de
  `package-and-release` (matriz linux/windows/macOS, JDK 25 embutido) — o
  pacote é montado a partir do commit de bump, com sanity check de `VERSION`.

## Aula 03 — Versionamento e release

- **Fonte única:** arquivo `VERSION` na raiz.
- `scripts/bump-version.sh` sincroniza `VERSION` → `pom.xml` (`<revision>`)
  → resource empacotado.
- Formato `MAJOR.MINOR.PATCH`; releases sem sufixo, ciclos de desenvolvimento
  com sufixo de estágio (`0.5.0-beta`).
- Tags `kof-<versão>`; commit de bump com `[skip ci]`.
- `scripts/package.sh` empacota `bin/ lib/ jdk/ tooling/ editor/ docs/ VERSION`
  em tarball + `SHA256SUMS`.
- Artefatos: `kof-<v>-linux-x86_64.tar.gz`, `...-windows-x86_64.zip`,
  `...-macos-arm64.tar.gz` (JDK Temurin 25 embutido; tooling API 21).

```bash
./scripts/bump-version.sh 0.5.0
./scripts/package.sh
# gera kof-0.5.0-linux-x86_64.tar.gz + SHA256SUMS
```

## Aula 04 — Containers e deploy

Um serviço Kof num container (multi-stage, sem JDK no runtime final basta o
JDK durante o build):

```dockerfile
FROM eclipse-temurin:21 AS build
WORKDIR /src
COPY . .
RUN mvn -q clean package -DskipTests && mkdir -p /app/lib \
    && cp kof-cli/target/kof-cli-*.jar /app/lib/kof.jar

FROM eclipse-temurin:21
WORKDIR /app
COPY --from=build /app/lib /app/lib
COPY app.kf /app/app.kf
EXPOSE 8080
CMD ["bash", "-c", "java -jar /app/lib/kof.jar serve /app/app.kf --port 8080"]
```

> **Nota:** use a **distribuição oficial** (com JDK embutido) como base da
> imagem final — `kof serve` cuida do resto; desde a 0.5.0 o driver JDBC do
> scheme é provisionado automaticamente (ou embutido com `kof build --fat`).

## Aula 05 — Observabilidade

- `/health` em cada serviço (módulo 13).
- `kof.log` com níveis (`KOF_LOG_LEVEL`), requestId.
- `kof.observability` (`health/readiness/liveness`, `counter/increment/gauge`, `requestId/correlationId`, spans W3C) — ✅ 4 alvos (cross fechado 26/09).
- `kof bench` / `kof profile` / `kof inspect` para performance e IR.

## Checkpoint

1. `kof build` para os 3 targets no seu projeto.
2. Workflow CI com `kof check` + `kof test`.
3. `scripts/package.sh`-like (tarball + SHA256).
4. Documente: VERSION → tag → release → container.