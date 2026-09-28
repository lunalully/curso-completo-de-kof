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
| `kof.ui` (widgets → DOM, `Style`, tokens, `AppState`) | ✅ |
| `kof.web` (servidor base) / `kof.http` | ✅ (base web no JS desde 0.4.0; `WEB003/004/005/006` por feature) |
| `kof.db`/`kof.orm` (ponte) | ✅ (`DB002` para `query<T>`; `ORM001` fechado) |
| `spawn` / `process.spawn` / `shell.pipeline` | ✅ (async real; host Kof) |
| `kof.buffer`, FFI (struct/array/buffer) | ✅ 0.5.0 |
| `time.collect()`, GPU fallback honesto | ✅ 0.5.0 (`TIME004`/`GPU001` aposentados) |

> Gaps de target são diagnósticos **em compile-time** — o programa não
> compila para JS com `spawn`; a plataforma diz, nunca silencia.
> 
> **Nota:** lambdas com capturas funcionam nos 5 alvos (JVM/Native/riscv/aarch64/JS) —
> paridade de captura fechada na 0.3.0 (3 níveis aninhados).

> **0.5.0:** `Long` é `BigInt` de verdade no JS (paridade de 64 bits);
> igualdade de coleções por conteúdo (`#518`); source maps com
> `sourcesContent`; `process.spawn`/pipeline reais sob o host Kof (node puro
> recusa com diagnóstico nomeado); `kof.ui` ganhou `Style` declarativo,
> tokens e `AppState` — ver `training/idioms/ui.md`.

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