# Módulo 07 · Aula 4 — KofJS e o fluxo web

> O mesmo frontend e a mesma IR geram **ES Modules** executados pela engine
> JS embarcada (GraalJS) — sem Node.js e sem runtime externo.

## Compilar para JS

```bash
kof build app.kf --target js --output out/
kof run app.kf --target js
```

## Cobertura do target JS (0.0.x)

| Recurso | JS |
|---------|----|
| classes, records, herança, interfaces | ✅ |
| lambdas (com capturas) | ✅ |
| if-expr, switch, loops, for-in | ✅ |
| `List`, JSON | ✅ |
| `kof.io` | ✅ |
| try/catch/finally | ✅ |
| `kof.ui` (widgets → DOM) | ✅ |
| `spawn` | ⏳ planned |

> **Nota:** no JS as lambdas podem capturar variáveis do escopo — uma
> diferença real de capacidade entre targets que a plataforma expõe
> honestamente.

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