# Trilha 15 · Aulas

## Aula 01 — Build multi-target

```bash
kof build app.kf --target jvm     --output out-jvm
kof build app.kf --target native  --output out-native
kof build app.kf --target js      --output out-js
kof run app.kf                    # compila e roda (JVM)
kof check app.kf                  # type-check rápido (CI)
kof test testes/                  # testes (PASS/FAIL por bloco test)
```

- O **mesmo código** gera JVM bytecode, ELF nativo x86-64 e ES Modules.
- Gaps de target (`DB001`, `CONC001`, `SECN00x`, `WEB001`...) são reportados
  em compile-time — o build falha com diagnóstico claro, nunca silencioso.
- No CI: compile todos os targets para **provar** que o código é multi-target.

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
        with: { distribution: temurin, java-version: '21' }
      - uses: actions/setup-python@v5   # mvn pode ser instalado pelo script
      - run: mvn -q clean package -DskipTests
      - run: bin/kof version
      - run: bin/kof check src/           # type-check
      - run: bin/kof test testes/          # testes
```

Regras do projeto:
- `main` nunca aponta para estado que não compila.
- Release só se `mvn clean test` e `mvn clean package` passarem.
- PR: build + testes + verificações estáticas.

## Aula 03 — Versionamento e release

- **Fonte única:** arquivo `VERSION` na raiz.
- `scripts/bump-version.sh` sincroniza `VERSION` → `pom.xml` (`<revision>`)
  → resource empacotado.
- Formato `MAJOR.MINOR.PATCH`; releases sem sufixo (`0.1.0`), ciclos de
  desenvolvimento com sufixo de estágio (`0.1.1-alpha`).
- Tags `kof-<versão>`; commit de bump com `[skip ci]`.
- `scripts/package.sh` empacota `bin/ lib/ jdk/ tooling/ editor/ docs/ VERSION`
  em tarball + `SHA256SUMS`.
- Artefatos: `kof-<v>-linux-x86_64.tar.gz`, `...-windows-x86_64.zip`,
  `...-macos-x86_64.tar.gz`.

```bash
./scripts/bump-version.sh 0.1.1
./scripts/package.sh
# gera kof-0.1.1-linux-x86_64.tar.gz + SHA256SUMS
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

> **Nota:** o Kof `serve` precisa do JAR + o programa + driver JDBC no
> classpath (para `kof.db`). Ajuste o `CMD` para o seu serviço.

## Aula 05 — Observabilidade

- `/health` em cada serviço (módulo 13).
- `kof.log` com níveis (`KOF_LOG_LEVEL`), requestId.
- `kof bench` / `kof profile` para performance.
- Planejado: `kof.metrics` / `kof.observability`.

## Checkpoint

1. `kof build` para os 3 targets no seu projeto.
2. Workflow CI com `kof check` + `kof test`.
3. `scripts/package.sh`-like (tarball + SHA256).
4. Documente: VERSION → tag → release → container.