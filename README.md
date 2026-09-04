# Curso Completo de Kof

> **Uma linguagem. Um compilador. Vários mundos.**
> *Menos código. Mais intenção.*

Este curso ensina a linguagem de programação **Kof** do zero até o nível
"padrão ouro" em desenvolvimento de software: algoritmos, estrutura de dados,
banco de dados, segurança, redes, HTTP, servidores, frontend e boas práticas —
tudo guiado pela **filosofia Kof**:

> Represente o domínio, não a implementação acidental. A complexidade pertence
> à plataforma. Intenção acima de cerimônia.

Todo código deste curso é **verificável** com o compilador Kof real:

```bash
kof run <arquivo>.kf [--target jvm|native|js|native.risc|native.arm] [--release]
kof check <arquivo>.kf      # type-check sem emitir código
kof build <dir> --target jvm|native|js|native.risc|native.arm|kofc [--release] [--output <dir>]
kof serve app.kf            # sobe um servidor HTTP (web.app + TLS)
kof test <arquivo.kf|dir> [--target jvm|native|js]  # PASS/FAIL por teste
kof debug app.kf            # sessão DAP (breakpoints, call stack)
kof bench [paths...] [--target jvm|native|js] [--iterations N]  # benchmarks
kof profile <file.kf> [--target jvm|native|js]   # CPU, RSS, GC
kof inspect <file.kf> [--json]  # estatísticas de IR
kof fmt <file.kf|dir> [-w]  # formatador (parser real, idempotente)
kof script <file.ks|kf> [--target jvm|native|js]  # KofScript (let top-level, repl)
kof repl                    # REPL KofScript interativo
kof c <file.c>              # KofC: subset C -> ELF nativo
kof info [--json]           # ambiente completo
kof lsp                     # Language Server Protocol
kof install <dir>           # instala distribuição local
kof version
```

---

## Mapa da trilha

| Módulo | Conteúdo | Pré-requisito |
|--------|----------|---------------|
| [`00-fundamentos`](00-fundamentos/) | Sintaxe, tipos, fluxo, funções, classes, records, coleções, erros, JSON | nenhum |
| [`01-algoritmos`](01-algoritmos/) | Busca, ordenação, recursão, complexidade | 00 |
| [`02-estruturas-de-dados`](02-estruturas-de-dados/) | Arrays, List, pilha, fila, grafos, hash (com a stdlib real) | 00, 01 |
| [`03-banco-de-dados`](03-banco-de-dados/) | SQL, `kof.db`, transações, CRUD | 00, 06 |
| [`04-seguranca`](04-seguranca/) | Senhas, crypto, JWT, segredos, auth web | 00 |
| [`05-redes`](05-redes/) | Modelo TCP/IP, HTTP, concorrência com `spawn` | 00 |
| [`06-http-servidores`](06-http-servidores/) | `web.app()`, rotas, middleware, REST, projeto completo | 00, 03, 04, 05 |
| [`07-frontend`](07-frontend/) | KofJS, `kof.ui`, janelas, render web | 00 |
| [`08-boas-praticas`](08-boas-praticas/) | Filosofia, idioms, anti-patterns, testes, config, logging | todos |
| [`09-ciberseguranca`](09-ciberseguranca/) | **Trilha completa de cibersegurança com Kof** | 04, 06, 08 |
| [`10-ciencia-de-dados`](10-ciencia-de-dados/) | Estatística, correlação, regressão, k-NN em Kof puro | 00, 01, 02 |
| [`11-testes-unitarios`](11-testes-unitarios/) | `assert`, `kof test`, casos de borda, TDD | 00, 01, 08 |
| [`12-debugger`](12-debugger/) | `kof debug` (DAP), erros de runtime, ferramentas | 00, 11 |
| [`13-microservicos`](13-microservicos/) | Serviços, HTTP entre serviços, gateway, config | 03, 06, 08, 09 |
| [`14-arquitetura`](14-arquitetura/) | Clean architecture na filosofia Kof, hexagonal sem container | 00, 06, 08, 13 |
| [`15-devops`](15-devops/) | Build multi-target, CI, release, containers, observabilidade | todas |
| [`projetos`](projetos/) | Projetos práticos de ponta a ponta | todos |
| [`exemplos`](exemplos/) | Programas compiláveis para copiar e estudar | todos |

---

## Como estudar

1. Comece pelo módulo 00 e faça todos os exemplos (rode `kof run`).
2. Não pule os exercícios — eles são o coração do aprendizado.
3. Mantenha a regra do corpus oficial: *código que compila ≠ código
   idiomático*. O curso ensina os dois.
4. Ao final, faça os projetos em `projetos/`, a trilha de cibersegurança em
   `09-ciberseguranca/` e feche com `15-devops/` (build multi-target + release).

## Estado das soluções

Todas as **soluções compilam e rodam** no compilador real 0.2.8-beta
(verificadas nesta trilha). Workarounds para bugs reais do compilador estão
em [`00-fundamentos/99-notas-workarounds.md`](00-fundamentos/99-notas-workarounds.md).

## Estado real da linguagem (versão do curso)

Baseado na **0.2.8-beta** (implementada, testada e verificada no compilador — 910 testes):

| Capacidade | Estado |
|------------|--------|
| Classes, records, interfaces, herança, dispatch virtual | ✅ 3 targets |
| Funções (todas as formas, sem `fun`), lambdas com capturas | ✅ 3 targets |
| if-expr, switch, switch-expressão (`case ... ->`), loops, for-in, default parameters | ✅ 3 targets |
| `List<T>`, `listOf`, arrays `new Int[n]`, `new Long[n]` | ✅ 3 targets |
| `Map<K,V>` (`mapOf`, put/get/remove/keys/values/size), `Set<T>` (`setOf`) | ✅ 3 targets |
| `enum` (+ `values`/`valueOf`/`name`, `==` por conteúdo, switch exaustivo) | ✅ 3 targets |
| Strings (`+`, `==`, API completa, `lastIndexOf`) | ✅ 3 targets |
| Exceções `throw "msg"` / try/catch/finally | ✅ 3 targets |
| `assert`, `test "nome" { }` + `kof test` | ✅ 3 targets |
| JSON `json.encode` / `json.decode<T>` (objetos/records/arrays) | ✅ 3 targets (JSN001/JSN002 fechados) |
| `kof.io` (File/Path/Directory, `readLine()`, `File.readRange()`) | ✅ 3 targets |
| `kof.time` (`now()`, `sleep`, `interval`, `every`/`at` scheduler) | ✅ 3 targets (TIME001 fechado) |
| `kof.web` (`web.app()`, rotas, middleware, `status()`, `headerSet()`, TLS, accept loop nativo) | ✅ JVM/Native (WEB003/WEB004 JS) |
| `kof.web` WebSocket (`app.ws("/chat") { }`) + SSE nativo | ✅ JVM (WEB003/WEB004 Native/JS) |
| `kof.http` client (`http.get/post/put/delete/status/timeout`) | ✅ 3 targets (HTTP002 fechado) |
| `kof.cache` (`cache.get/set/ttl/delete/clear`) | ✅ 3 targets |
| `kof.db` (JDBC, `transaction {}` commit/rollback real, `query<T>`) + SQLite nativo | ✅ JVM/Native (DB001 JS) |
| `kof.orm` (`entity`, CRUD, `where`, migrations, MongoDB, Query DSL tipada) | ✅ JVM/Native (ORM001 JS) |
| `kof.security` (passwords, crypto, jwt, secrets, auth, rateLimit) | ✅ 3 targets |
| `kof.config`, `kof.log` | ✅ 3 targets (LOG001 fechado) |
| `kof.config` interpolação `${key}` | ✅ 3 targets |
| `spawn` (stmt) / `val r = spawn f()` / `await r` / `poll/done/cancel/selectAny` | ✅ 3 targets (CONC001/CONC003 fechados) |
| `process.exit(code)` / `readLine()` | ✅ 3 targets |
| `process.run(cmd, args...)` / `process.spawn(cmd, args...)` | ✅ 3 targets (PROC001 fechado) |
| `kof fmt <file\|dir> [-w]` (formatador, parser real) | ✅ 3 targets |
| `kof.ui` (Color, Palette, Theme, Window, widgets) | ✅ JS/webview |
| Pattern matching `switch case String s`, `case Point(x,y)`, `instanceof` | ✅ 3 targets |
| Null safety `String?`, `Int?` + `if (x != null)` narrowing | ✅ 3 targets |
| `List.map/filter/reduce` (higher-order) | ✅ 3 targets |
| `kof.mq` (pub/sub em memória) | ✅ 3 targets (MQ001 fechado) |
| `kof.validation` (13 predicados) | ✅ 3 targets |
| `kof.observability` (health, counter/gauge/histogram) | ✅ 3 targets |
| `application {}` lifecycle (onStart/onShutdown) | ✅ 3 targets |
| W3C spans (spanStart/spanEnd) | ✅ 3 targets |
| package manager MVP (`kof deps`) | ✅ disponível |

**Gaps de target** são reportados em compile-time com códigos
(`DB001`, `SECN00x`, `WEB001`, `ORM001`, `AND001`) — nunca de forma silenciosa. É parte da filosofia:
a plataforma diz o que consegue fazer.

## Licença

O material deste curso segue a filosofia e os exemplos do projeto
[Kof4j](https://github.com/KofLang/Kof4j) (GPL-3.0). Os exemplos de código
foram verificados contra o compilador real.
