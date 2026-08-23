# Módulo 07 · Aula 2 — Widgets: Window, Label, Button, Input

> A UI é uma árvore de objetos tipados. Widgets → DOM no webview/browser.

## Um contador (exemplo oficial)

```kof
class App {
    static Int count = 0
}

main() {
    var w = Window("Contador")
    var label = Label("contagem: 0")

    w.bind(label)
    w.bind(Button("+1", () -> {
        App.count = App.count + 1
        label.text = "contagem: " + App.count
    }))

    w.show()
}
```

```bash
kof run contador.kf --target=js   # abre a janela; fechar encerra o programa
```

**O que acontece:**
- `Window("Contador")` — a janela, com título.
- `Label(texto)` — texto renderizado.
- `Button(texto, lambda)` — botão com ação por lambda.
- `w.bind(widget)` — adiciona à árvore.
- `w.show()` — exibe; fechar encerra o programa.

## Input

```kof
class App {
    static String nome = ""
}

main() {
    var w = Window("Entrada")
    var entrada = Input()
    var saida = Label("digite seu nome")

    w.bind(entrada)
    w.bind(saida)
    w.bind(Button("OK", () -> {
        saida.text = "ola, " + App.nome
    }))

    // ligar o input ao estado
    // (consulte a documentação de bind da sua versão)

    w.show()
}
```

## Propriedades típicas de widgets

- `Label`: `text`, `fontSize`, `bold`, `color`.
- `Window`: `title`, `size`, `theme`, `show()`, `close()`.

## Por que objetos

- **Tipado**: um widget com campo errado não compila.
- **Composável**: a tela é um valor — pode vir de função, lista, condição.
- **Multi-target**: a mesma árvore vira o que cada backend souber desenhar.

## Exercícios

1. Reproduza o contador e clique várias vezes.
2. Adicione um botão "-1" (decremento).
3. Crie uma janela com um `Input` que imprime o texto num `Label`.
4. (Desafio) Use `Palette` + `Theme` para estilizar o label do contador.