# Módulo 07 · Aula 3 — Layout: Column, Row, View, Style

> Layout é composição de objetos: containers empilham filhos; `View`+`Style`
> dão o visual.

## O grafo de objetos

```kof
View homeView() {
    return View(
        Style(background: Colors.background, padding: 16),
        [
            Text("Bem-vinda", Style(color: Colors.primary, bold: true)),
            Button("Entrar", () => login())
        ]
    )
}
```

> **Nota real:** a sintaxe exata (`Style(color: ...)`, lista literal `[...]`,
> `=>`) depende da sua versão do compilador — o `kof.ui` está em evolução.
> **Teste sempre** o que compila. A **estrutura conceitual** (objetos, estilo
> por objeto, composição) é a parte estável.

## Column / Row

- `Column` empilha filhos **verticalmente**.
- `Row` empilha filhos **horizontalmente**.

```kof
// exemplo conceitual
main() {
    var w = Window("Layout")

    var col = Column()
    col.add(Label("item 1"))
    col.add(Label("item 2"))
    col.add(Button("acao", () -> println("ok")))

    w.bind(col)
    w.show()
}
```

## View + Style

`View` é um container; `Style` carrega o visual:

```kof
class Style {
    Color background
    Color foreground
    Int padding
    Int radius
}
```

Estilo é feito de `Color`, `Int` e `String` — **tipos da linguagem, nunca
textos soltos**.

## Paleta da aplicação

Cores nomeadas como constantes (nunca hex solto no meio do código):

```kof
class Colors {
    static Color primary    = Color(0xFF6750A4)
    static Color background = Color(0xFF121212)
    static Color text       = Color(0xFFE0E0E0)
    static Color error      = Color(0xFFCF6679)
}
```

## Exercícios

1. Monte uma `Column` com 3 `Label` e um `Button`.
2. Monte uma `Row` com 2 botões.
3. Aplique um `Style` com background e padding num `View`.
4. (Desafio) Tema claro/escuro alternável: troque `Theme.dark()` por
   `Theme.light()` conforme o estado.