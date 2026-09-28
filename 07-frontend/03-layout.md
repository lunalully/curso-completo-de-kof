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

## Novidades reais 0.4.5–0.5.0 (kof.ui)

**`Style` declarativo (UI007)** — uma string CSS-like, parseada e validada no
compilador (propriedade desconhecida → `SEM076/077/078`, nunca silencioso):

```kof
var card = Style("background: #ffffff; padding: 8; border-radius: 4")
var v = View(card)
var l = Label("titulo")
l.setStyle(card)               // qualquer widget, não só View
```

**Tokens do design system (Fase 10)** — números mágicos viram intenção:

```kof
l.setFontSize(Typography.lg)   // 20
var pad = Spacing.md           // 16  (grade de 8px: 4/8/16/24/32)
var rad = Radius.md            // 4   (none/sm/md/lg/full)
var b = Border.thin            // 2   (hairline/thin/medium/thick)
var e = Elevation.sm           // 1   (none/sm/md/lg/xl)
```

Membro inexistente (`Spacing.huge`) é `SEM079`.

**Estado sem prop-drilling (Fase 8)** — `AppState` é o store-raiz:

```kof
AppState(0).set(1)
AppState(0).subscribe((v: Int) -> { l.text = "mode " + v })
```

**Ciclo de vida libera sozinho (`D-UI-AUTOUNSUB`, 18/09)** — subscription ou
store criado **durante** o ciclo de vida do componente morre no unmount; sem
`unsubscribe` manual, sem leak (10k ciclos provados). Criado fora de qualquer
componente, é escopo de app e continua manual.

> O observable vive no KofJS; no JVM/Native/Script essas faces são no-op
> documentado. O `Style` de string é real no KofJS.

## Exercícios

1. Monte uma `Column` com 3 `Label` e um `Button`.
2. Monte uma `Row` com 2 botões.
3. Aplique um `Style` com background e padding num `View`.
4. (Desafio) Tema claro/escuro alternável: troque `Theme.dark()` por
   `Theme.light()` conforme o estado.