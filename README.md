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
kof run <arquivo>.kf        # compila e executa (JVM)
kof check <arquivo>.kf      # type-check sem emitir código
kof build <dir> --target jvm   # múltiplos targets: jvm | native | js
kof serve app.kf            # sobe um servidor HTTP
kof test <arquivo.kf>       # roda testes (PASS/FAIL por exit code)
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

Todas as **soluções compilam e rodam** no compilador real 0.0.8-alpha
(verificadas nesta trilha). Workarounds para bugs reais do compilador estão
em [`00-fundamentos/99-notas-workarounds.md`](00-fundamentos/99-notas-workarounds.md).

## Estado real da linguagem (versão do curso)

Baseado na **0.0.8-alpha** (implementada, testada e verificada no compilador):

| Capacidade | Estado |
|------------|--------|
| Classes, records, interfaces, herança, dispatch virtual | ✅ |
| Funções (todas as formas, sem `fun`), lambdas (sem captura) | ✅ |
| if-expr, switch, loops, for-in | ✅ |
| `List<T>`, `listOf`, arrays `new Int[n]` | ✅ |
| Strings (`+`, `==`, API completa) | ✅ |
| Exceções `throw "msg"` / try/catch/finally | ✅ |
| JSON `json.encode` / `json.decode<T>` (objetos só JVM) | ✅ |
| `kof.io` (File/Path/Directory), `kof.time` (`now()`) | ✅ |
| `kof.web` (`web.app()`, rotas, middleware) | ✅ JVM |
| `kof.db` (JDBC idiomático, `transaction {}`) | ✅ JVM |
| `kof.security` (passwords, crypto, jwt, secrets, auth) | ✅ JVM (parcial Native/JS) |
| `kof.config`, `kof.log` | ✅ JVM |
| `kof.ui` (Color, Palette, Theme, Window, widgets) | ✅ JS/webview |
| `spawn` (concorrência) | ✅ JVM |
| `Map`, `Set`, `Option<T>`, higher-order (`map`/`filter`), captura em lambda, pattern matching | ⏳ planned — **não use** |

**Gaps de target** (Native/JS) são reportados em compile-time com códigos
(`DB001`, `CONF001`, `LOG001`, `SECN001`, `CONC001`, `JSN002`) — nunca de
forma silenciosa. É parte da filosofia: a plataforma diz o que consegue fazer.

## Licença

O material deste curso segue a filosofia e os exemplos do projeto
[Kof4j](https://github.com/KofLang/Kof4j) (GPL-3.0). Os exemplos de código
foram verificados contra o compilador real.# curso-completo-de-kof
