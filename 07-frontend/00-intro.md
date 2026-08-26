# Módulo 07 — Frontend

> **Objetivo:** interfaces visuais com Kof — `kof.ui` (Color, Palette, Theme,
> widgets) e o target JS. A mesma linguagem, o mesmo compilador, a mesma IR.

## A filosofia

> O visual é um **grafo de objetos**. Nada de templates, XML ou strings
> mágicas: a interface é código Kof tipado, compilado e verificado como
> qualquer outro.

```text
Kof source → Kof Compiler → Kof IR → KofJS → DOM real (webview/browser)
```

## Estado

| Recurso | Estado |
|---------|--------|
| `Color`, `Palette`, `Theme` | ✅ JVM/Native/JS |
| `Window`, `Label`, `Button`, `Input` | ✅ JS (webview/browser) |
| `Column`, `Row`, `View`, `Style` | ✅ JS |
| Ações por lambda | ✅ JS |
| Renderização | sempre via KofJS → DOM (webview/browser); JVM/Native ponte para o mesmo runtime |
| `spawn` no JS | ❌ `CONC003` em compile-time |

A renderização é **KofJS**: widgets → DOM real no webview nativo
(`bin/kof-webview`, WebKitGTK) ou no browser. Sem JavaFX, sem AWT.

## Sumário de aulas

| Aula | Tema |
|------|------|
| [01-cores-temas.md](01-cores-temas.md) | Color, Palette e Theme |
| [02-widgets.md](02-widgets.md) | Window, Label, Button, Input |
| [03-layout.md](03-layout.md) | Column, Row, View, Style |
| [04-kofjs.md](04-kofjs.md) | O target JS e o fluxo web |