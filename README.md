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
kof run <arquivo>.kf [--target jvm|native|js|native.risc|native.arm|android] [--release]
kof check <arquivo>.kf|dir [--target <t>] [--json]  # type-check sem emitir código
kof build <dir|file.kf> --target jvm|native|js|native.risc|native.arm|android [--release] [--fat] [--print-sizes] [--output <dir>]
kof serve app.kf [--port N] [--host H]   # sobe um servidor HTTP (web.app + TLS)
kof test <arquivo.kf|dir> [--target jvm|native|js] [--timeout S] [--tag T]  # PASS/FAIL por teste
kof debug app.kf [--dap] [--attach <pid>] [--target native] [--break <linha>]
kof bench [paths...] [--target jvm|native|js] [--baseline f] [--fail-on-regression]
kof profile <file.kf> [--target jvm|native|js] [--methods]   # CPU, RSS, GC, métodos
kof inspect <file.kf> [--json]  # estatísticas de IR
kof fmt <file.kf|dir> [-w]  # formatador (parser real, idempotente)
kof md check|format <file.md>  # Kofmd (MD002)
kof script <file.ks|kf> [--target jvm|native|js]  # KofScript: Kof puro via KofInterpreter (sem let/const)
kof repl                    # REPL KofScript interativo (Kof puro)
kof c <file.c> [--target x86_64|riscv64|aarch64]  # KofC: subset C -> ELF nativo
kof new <dir> [--type mono|backend|frontend|full-stack]  # esqueleto de projeto
kof deps <init|add|remove|list|resolve>  # gerenciador de pacotes (kofdeps)
kof workflow <list|run> <file.kf> [--job <nome>] [--dry-run]
kof deploy <dir> --target jvm [--publish]  # release empacotada (fat jar + checksums)
kof migrate|translate|decompile|compare    # legado Java/.class
kof editor <list|detect|setup|install>     # integração com editores (EDI001)
kof config gen <file.kf|dir> [--output <f>]  # template kof.config das chaves usadas
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

Todas as **soluções compilam** no compilador real **0.5.0-beta** (varredura
`kof check` arquivo a arquivo nesta trilha: 193/193 `.kf`, 0 falhas).
Workarounds residuais estão
em [`00-fundamentos/99-notas-workarounds.md`](00-fundamentos/99-notas-workarounds.md)
— a varredura de setembro/2026 fechou quase todos.

## Novidades 0.3.8 → 0.5.0 (o que o curso ganhou nesta atualização)

| Área | O que entrou |
|------|--------------|
| Linguagem | sobrecarga top-level (0.4.0), casts saturantes, `pkg.Type` qualificado, funções aninhadas com hoisting, guards de pattern, switch-expressão e tipo comum de ramos, generics **invariantes** (0.5.0) |
| Null safety | `null` literal proibido (`SEM048`), narrowing obrigatório, **`Troolean`** para três estados (`SEM095` — `Bool?` não existe) |
| Tipos/records | enum é classe real (identidade, `ordinal`/`compareTo`, `SEM062`), regra de campo único (`SEM076`), forma de classe validada (`SEM041-071`), `return` com valor em void é `SEM093` |
| Stdlib | `math.sqrt/pow/parse*/roundTo/lerp`, `strings.uncapitalize/reverse/toCharArray`, `time.addDays/diffDays/todayIso/…`, `uuid.v7/isUuid`, `validation.format*/isNis`, `random.randomBytesHex/randomString`, `rng` (semeado), `kof.shell`/`kof.ssh`, `Secret`/`KeyHandle` |
| Web/Segurança | `app.security()` (CSRF on por padrão, ordem fixa), cookies seguros, TLS próprio, OAuth2 resource server, `encryptChacha20` (JVM/JS; Native `SECN002`), `app.serveDir` |
| Dados | driver JDBC automático, schemes `mysql://`/`postgres://`/`sqlite:` normalizados, MySQL/MariaDB wire no Native, ORM nativo completo, `mongodb://` no JVM/Android |
| Ferramentas | `kof fmt`, `kof md`, `kof new`, `kof deps` (registry + proveniência), `kof workflow`, `kof deploy`, `kof debug` nativo (DWARF), `kof profile --methods`, `kof test --tag/--timeout` + setup/teardown, `kof config gen`, `kof editor` |
| Frontend | `Style` declarativo (UI007), tokens `Spacing/Radius/Border/Elevation/Typography`, `AppState`, auto-unsub no unmount |
| Confiabilidade | `kof.workflow` (retry/backoff/deadLetter/checkpoint/schedule/`runSupervised`), supervisor OTP, MEM001–022 (recursos não fechados avisam), `SEM102`/gates silenciosos viraram diagnóstico |

## Estado real da linguagem (versão do curso)

Baseado na **0.5.0-beta** (implementada, testada e verificada no compilador — ~3.700 testes no compilador / 4.300 no reactor):

| Capacidade | Estado |
|------------|--------|
| Classes, records, interfaces, herança, dispatch virtual | ✅ 3+ targets |
| Funções (todas as formas, sem `fun`), **sobrecarga top-level**, tipos de função, lambdas com capturas | ✅ 4 alvos |
| if-expr, switch, switch-expressão (`case ... ->`), loops, for-in, default parameters | ✅ 3+ targets |
| `List<T>`, `listOf`, arrays `new Int[n]`, `new Long[n]` | ✅ 4 alvos |
| `Map<K,V>` (`mapOf`, put/get/remove/keys/values/size), `Set<T>` (`setOf`) | ✅ 4 alvos |
| `enum` como classe real (+ `values`/`valueOf`/`name`/`ordinal`/`compareTo`, switch exaustivo) | ✅ 4 alvos |
| Strings (`+`, `==`, API completa, `toCharArray`) | ✅ 5 alvos |
| Exceções `throw "msg"` / try/catch/finally (tipos de catch validados) | ✅ 4 alvos |
| `assert`, `test "nome", "tag" { }` + `kof test` (setup/teardown, `--timeout`) | ✅ 3 targets |
| JSON `json.encode` / `json.decode<T>` (objetos/records/arrays/FP) | ✅ 5 alvos |
| `kof.io` completo (File/Path/Directory, `readRange`, `copyTo`/`moveTo`) | ✅ 5 alvos (0.5.0) |
| `kof.time` (`now`, `sleep`, calendário ISO) + `scheduler.every/at` | ✅ 3 targets (cron real JVM/JS; Native `CRON001`) |
| `kof.web` (`web.app()`, rotas, middleware, `status()`, `headerSet()`, `serveDir`) | ✅ JVM/Native; base JS |
| `app.security()` (rate-limit, CORS, headers, sessão, CSRF) | ✅ JVM (`WEB006` Native/JS) |
| `kof.web` WebSocket/SSE | ✅ JVM (`WEB004`/`WEB003` fora) |
| `kof.http` client (`get/post/...` + `timeout/retry/circuit`) | ✅ 5 alvos |
| `kof.cache` (`cache.get/set/ttl/delete/clear`) | ✅ 4 alvos |
| `kof.db` (JDBC/SQLite/MySQL wire, `transaction {}`, `query<T>`) | ✅ JVM/Native/JS (recusas `DB001`/`DB002` nomeadas) |
| `kof.orm` (`entity`, CRUD, `where`, page, migrations, Query DSL) | ✅ JVM/Native; JS (fechado) |
| `kof.security` (passwords, crypto, jwt, secrets, `Secret`/`KeyHandle`, auth, rateLimit) | ✅ JVM/JS/Native x86 (gates `SECN00x`) |
| `kof.config`, `kof.log` | ✅ 4 alvos |
| `spawn`/`await`/`poll`/`done`/`cancel`/`selectAny`/`channel<T>` | ✅ 4 alvos |
| `process.run/spawn`, `kof.shell` (argv em lista), `kof.ssh` | ✅ JVM/Native/JS |
| `kof.workflow` (job/dag/retry/deadLetter/checkpoint/schedule/`runSupervised`) | ✅ JVM/JS/Native |
| `kof.supervisor` (OTP one_for_one) | ✅ JVM/Script/Native x86 (gates `OTP001/002`) |
| `kof fmt` / `kof md` / `kof test` / `kof profile --methods` / `kof debug` nativo | ✅ tooling |
| `kof.ui` (Color, Palette, Theme, widgets, `Style`, tokens, `AppState`) | ✅ JS/KofJS (faces no-op documentadas) |
| Pattern matching `case String s`, `case Point(x,y)` + guards | ✅ 3+ targets |
| Null safety `String?`/`Int?` + narrowing; `Troolean` | ✅ 4 alvos |
| `List.map/filter/reduce` (higher-order) | ✅ 4 alvos |
| `kof.mq` (pub/sub em memória) | ✅ 3 targets |
| `kof.validation` (30+ predicados) | ✅ 5 alvos |
| `kof.observability` (health, counter/gauge/histogram, spans) | ✅ 4 alvos |
| `kof.strings` / `kof.encoding` / `kof.random` / `kof.rng` | ✅ 5 alvos |
| `kof.net` (URI parse + fetch) | ✅ 5 alvos |
| `kof.uuid` (v4 + v7 + isUuid) | ✅ 5 alvos |
| arrays multidimensionais `new T[a][b]` | ✅ 5 alvos |
| package manager (`kof deps`, registry + proveniência) | ✅ disponível (0.5.0) |
| `kof deploy` (multi-target, `--publish`) | ✅ disponível |

**Gaps de target** são reportados em compile-time com códigos
(`DB001`, `DB002`, `SECN00x`, `WEB002-006`, `ORM001`, `CRON001`, `OTP00x`,
`AND001`, `PROC001`, `STR003`...) — nunca de forma silenciosa. É parte da filosofia:
a plataforma diz o que consegue fazer.

## Licença

O material deste curso segue a filosofia e os exemplos do projeto
[Kof4j](https://github.com/KofLang/Kof4j) (GPL-3.0). Os exemplos de código
foram verificados contra o compilador real.
