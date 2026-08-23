# Trilha 07 — Frontend com kof.ui

> **Trilha isolada.** Pré-requisito: Trilha 00. Ao terminar, você constrói
> interfaces visuais com `kof.ui` (Color, Palette, Theme, widgets) e compila
> para o target JS.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-cores-temas.md](01-cores-temas.md) | Color, Palette, Theme |
| [02-widgets.md](02-widgets.md) | Window, Label, Button, Input |
| [03-layout.md](03-layout.md) | Column, Row, View, Style |
| [04-kofjs.md](04-kofjs.md) | target JS e fluxo web |

## Como estudar

1. Leia as aulas. Rode os exemplos de Color/Theme (`kof run`).
2. Compile os widgets para JS (`kof build --target js`).
3. Faça os [exercícios](exercicios.md).
4. Confira as [soluções](solucoes/).
5. Passe no checkpoint.

## Checkpoint

Um programa que:
1. Imprime `Color(255,0,0).toCss()`, um `Palette` e um `Theme.dark().background()`.
2. Declara um `Style` com cores semânticas.
3. Compila para JS sem erros (`kof build --target js`).

`kof run` + `kof build --target js` sem erros → trilha concluída.