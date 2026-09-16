# Módulo 07 · Aula 4 — KofJS e o fluxo web

> O mesmo frontend e a mesma IR geram **ES Modules** executados pela engine
> JS embarcada (GraalJS) — sem Node.js e sem runtime externo.

## Compilar para JS

```bash
kof build app.kf --target js --output out/
kof run app.kf --target js
```

## Cobertura do target JS

| Recurso | JS |
|---------|----|
| classes, records, herança, interfaces | ✅ |
| lambdas (com capturas) | ✅ |
| if-expr, switch, loops, for-in | ✅ |
| `List`, `Map`, `Set`, `enum`, JSON | ✅ |
| `kof.io` | ✅ |
| try/catch/finally | ✅ |
| `kof.security` (passwords/crypto/jwt) | ✅ |
| `kof.ui` (widgets → DOM) | ✅ |
| `kof.web` / `kof.http` | ✅ HTTP client (`HTTP002`); servidor web roda no host (GraalJS `HttpServer`); residual ws/sse/TLS = `WEB001` |
| `spawn` / `await` / `selectAny` | ✅ `CONC003` fechado (async/await/Promise real) |

> Gaps de target são diagnósticos **em compile-time** (ex.: `OTP002` em
> `kof.supervisor` no JS, `DB001` em `kof.db`) — a plataforma diz, nunca
> silencia.
>
> **Nota:** lambdas com capturas funcionam nos 5 alvos (JVM/Native/riscv/aarch64/JS) —
> paridade de captura fechada na 0.3.0 (3 níveis aninhados).

> **0.4.x:** além de `kof.strings`/`encoding`/`random`/`net` (5-6 alvos), o
> JS ganhou: servidor web GraalJS hostless (`web.app()` + fila de eventos
> `KofJsWebQueue`), `spawn http.get(url)` + `await` = fetch assíncrono real
> no Node/browser (§133), `Long = BigInt` (paridade 64-bit), `json.decode<Map>`,
> `crypto` AES-256-GCM em JS puro (`SECN002` fechado) e o mesmo `kof.ui` com
> widgets (Table/Select/Textarea/Canvas) renderizando em browser real —
> Chrome headless E2E.

## kof.ui no JS

```bash
kof run contador.kf --target=js
```

Abre a janela no **webview nativo** (`bin/kof-webview`, WebKitGTK) ou no
browser. Fechar a janela encerra o programa.

## O editor Kof é a prova viva

O **Kof Editor** (`github.com/KofLang/Kof-Editor`) é escrito 100% em Kof:
lexer, highlight, árvore de arquivos, abas, terminal, git. Servido por
`kof serve` e exibido na janela nativa. Ele usa o **mesmo lexer do
compilador** que o escreveu.

```bash
kof-editor                # abre a janela
kof-editor caminho.kf     # abre com um arquivo
kof-editor --stop         # para servidor e janela
```

## Exercícios

1. Compile o contador para JS e rode no browser/webview.
2. Compare um programa com lambda capturando escopo nos targets jvm e js.
3. Abra o Kof Editor e escreva um programa Kof dentro dele.
4. (Desafio) Crie uma mini-UI (lista de tarefas) com `kof.ui` no JS.