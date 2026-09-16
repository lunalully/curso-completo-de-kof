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
kof check <arquivo>.kf [--json]  # type-check sem emitir código
kof build <dir> --target jvm|native|js|android [--release] [--output <dir>] [--apk] [--fat] [--print-sizes] [--classpath <jars>]
kof serve app.kf            # sobe um servidor HTTP (web.app + TLS)
kof test <arquivo.kf|dir> [--target jvm|native|js]  # PASS/FAIL por teste
kof debug app.kf            # sessão DAP (breakpoints, call stack)
kof bench [paths...] [--target jvm|native|js] [--iterations N]  # benchmarks
kof profile <file.kf> [--target jvm|native|js]   # CPU, RSS, GC
kof inspect <file.kf> [--json]  # estatísticas de IR
kof fmt <file.kf|dir> [-w]  # formatador (parser real, idempotente)
kof script <file.ks|kf> [--target jvm|native|js]  # KofScript: Kof puro via KofInterpreter (sem let/const)
kof repl                    # REPL KofScript interativo (Kof puro)
kof c <file.c>              # KofC: subset C -> ELF nativo
kof new <tipo>              # esqueletos de projeto (app, web, lib...)
kof decompile <classes...>  # bytecode -> Kof (round-trip)
kof translate <file.java>   # Java -> Kof (tradutor com gaps honestos)
kof migrate / kof compare   # migração e comparação de projetos
kof editor                  # suporte a editores (detect/install/uninstall, IntelliJ incluso)
kof config gen              # geração de configuração
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

Todas as **soluções compilam e rodam** no compilador Kof real **0.4.1-beta**
(180/180 `kof check` + 31/31 fundamentos `kof run`, verificados nesta trilha).
Notas de workarounds (a maioria já fechada pelo compilador) estão em
[`00-fundamentos/99-notas-workarounds.md`](00-fundamentos/99-notas-workarounds.md).

## Estado real da linguagem (versão do curso)

Baseado na **0.4.1-beta** (implementada, testada e verificada no compilador —
suíte de release 1819 testes, run limpo 4-módulos: JVM/Native(x86/riscv64/aarch64)/JS/KofScript):

| Capacidade | Estado |
|------------|--------|
| Classes, records, interfaces, herança, dispatch virtual, sobrecarga de método (SEM061) | ✅ 3 targets |
| Funções (todas as formas, sem `fun`), sobrecarga top-level (SEM047/SEM057), lambdas com capturas | ✅ 3 targets |
| if-expr, switch, switch-expressão (`case ... ->`), loops, for-in, default parameters, atribuições compostas (`+=`, `<<=`) | ✅ 3 targets |
| `List<T>`, `listOf`, arrays `new Int[n]`, `new Long[n]`, multidim `new T[a][b]` | ✅ 3 targets |
| `Map<K,V>` (`mapOf`, put/get/remove/keys/values/`size`), `Set<T>` (`setOf`) | ✅ 3 targets |
| `enum` (`values`/`valueOf`, `==` por identidade, switch exaustivo) | ✅ 3 targets (classe real `name`/`ordinal`/`compareTo` → branch, ainda não na release) |
| Strings (`+`, `==`, `compareTo`, API completa, code units UTF-16) | ✅ 3 targets |
| Exceções `throw "msg"` / try/catch/finally (finally roda no caminho `return` — DD-01) | ✅ 3 targets |
| `assert`, `test "nome" { }` + `kof test` | ✅ 3 targets |
| JSON `json.encode` / `json.decode<T>` (objetos/records/arrays/`Map<String,T>`) | ✅ 3 targets (JSN004 Native: `List<Record>` é gap honesto) |
| `kof.io` (File/Path/Directory, `readLine()`, `File.readRange()`) | ✅ 3 targets |
| `kof.time` (`now`, `sleep`, `interval`, `every`/`at`, calendário civil ISO: `todayIso`/`addDays`/`diffDays`/`isWeekend`) | ✅ 5 alvos (TIME002 fechado) |
| `kof.web` (`web.app()`, rotas, middleware, `status()`, `headerSet()`, `app.security()`) | ✅ JVM · ✅ Native/JS base (WEB002/WEB001 residual: TLS/ws/sse) |
| `kof.web` WebSocket (`app.ws`) + SSE | ✅ JVM (WEB002 Native / WEB001 JS) |
| `kof.http` client (`http.get/post/put/delete/status/timeout` + `spawn`+`await` async no JS) | ✅ 3 targets (HTTP002/§133 fechados) |
| `kof.cache` (`cache.get/set/ttl/delete/clear`) | ✅ 3 targets |
| `kof.db` (JDBC, `transaction {}` commit/rollback real, `query<T>`) + SQLite nativo | ✅ JVM · ✅ Native x86/MySQL (DB001: JS + riscv/aarch em compilação) |
| `kof.orm` (`entity`, CRUD, `where`, migrations, Query DSL tipada) | ✅ JVM · Native (ORM001 JS) |
| `kof.security` (passwords, crypto/AES-GCM, jwt, secrets, auth, rateLimit) | ✅ 3 targets (SECN002 fechado) |
| `kof.config` (interpolação `${key}`), `kof.log` | ✅ 3 targets (LOG001 fechado) |
| `spawn` (stmt) / `val r = spawn f()` / `await r` / `poll/done/cancel/selectAny` | ✅ 3 targets (CONC001/CONC003/§129 fechados) |
| `import kof.supervisor` (OTP puro-Kof: observar/reiniciar/escalate) | ✅ JVM/Script · ✅ Native x86 (OTP001 riscv/aarch · OTP002 JS) |
| `process.exit` / `readLine` / `process.run` / `process.spawn` | ✅ 3 targets (PROC001 fechado) |
| `kof fmt` / `kof check --json` (parser real, idempotente) | ✅ CLI |
| `kof.ui` (Color, Palette, Theme, Window, widgets, layout) | ✅ JS/webview |
| Pattern matching `case String s`, `case Point(x,y)`, `instanceof`, guardas `case T v if` (SG-014) | ✅ 3 targets |
| Null safety `String?`/`Int?` + narrowing `if (x != null)` (SEM048/SEM049) | ✅ 3 targets |
| `List.map/filter/reduce` (inferência contextual de lambda — SG-012) | ✅ 3 targets |
| `kof.mq` (pub/sub em memória) | ✅ 3 targets (MQ001 fechado) |
| `kof.validation` (40+ predicados: isCpf/isCnpj/format*, isIpv4/v6, Luhn, isDomain, isNis) | ✅ 4-5 alvos |
| `kof.observability` (health, counter/gauge/histogram) | ✅ 3 targets (OBS002 fechado) |
| `kof.strings` (escapeJson/unescapeHtml, word converters, pad/repeat, indent/dedent, uncapitalize) | ✅ 5 alvos |
| `kof.encoding` (base64/hex/urlEncode) | ✅ 5 alvos |
| `kof.random` (randomInt/Boolean/double/hex/randomBytesHex/randomString) | ✅ 5 alvos (S10 fechado) |
| `kof.net` (URI parse + fetch) | ✅ 6 alvos (NET001 fechado) |
| `kof.uuid` v4 + v7 (RFC 9562) + `isUuid` | ✅ 5 alvos (UUID001 cross) |
| `kof.math` Double (`sqrt`/`pow`/`lerp`/`roundTo`/`isInteger`/`isNaN`/`parse*`/`parse*OrDefault`) | ✅ 5 alvos (MATH001 fechado) |
| arrays multidimensionais `new T[a][b]` | ✅ 5 alvos (MULTIANEWARRAY) |
| `application {}` lifecycle (onStart/onShutdown) | ✅ 3 targets |
| W3C spans (spanStart/spanEnd) | ✅ 3 targets |
| package manager MVP (`kof deps`, `kof new`) | ✅ disponível |

**Modelo de memória SC** (SG-020): campos mutáveis de classe Kof emitem
`ACC_VOLATILE` — stop-flags compartilhadas entre workers são observáveis.
**Long = BigInt no JS** (paridade 64-bit real). **Gaps de target** são
reportados em compile-time com códigos (`DB001`, `SECN00x`, `WEB00x`, `ORM001`,
`OTP001`, `TIME003`, `AND001`) — nunca de forma silenciosa. É parte da
filosofia: a plataforma diz o que consegue fazer.

## Licença

O material deste curso segue a filosofia e os exemplos do projeto
[Kof4j](https://github.com/KofLang/Kof4j) (GPL-3.0). Os exemplos de código
foram verificados contra o compilador real.
