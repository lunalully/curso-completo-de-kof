# Módulo 07 · Aula 1 — Color, Palette e Theme

> A fundação visual: cores de 32 bits com nomes, não códigos mágicos.

## Color

```kof
var c = Color(255, 0, 0)          // canais 0-255, alpha 255
var a = Color.rgba(10, 20, 30, 128)  // com alpha
var cru = Color(0xFF0000FF)       // valor empacotado 0xRRGGBBAA

c.red()      // 255
c.green()    // 0
c.blue()     // 0
c.alpha()    // 255
c.isOpaque() // true (alpha == 255)
c.withAlpha(128)

c.toCss()    // "rgb(255, 0, 0)" ou "rgba(10, 20, 30, 128)"
```

Layout: `(r << 24) | (g << 16) | (b << 8) | a`.

## Palette — cores nomeadas

```kof
Palette.red       // rgb(255, 0, 0)
Palette.green
Palette.blue
Palette.yellow
Palette.cyan
Palette.magenta
Palette.black
Palette.white
Palette.gray      // ou Palette.grey
Palette.orange
Palette.purple
Palette.pink
Palette.brown
Palette.transparent
```

## Theme — claro/escuro com cores semânticas

```kof
var dark = Theme.dark()
dark.isDark()               // true
dark.background().toCss()   // rgb(18, 18, 18)
dark.surface()
dark.primary()
dark.secondary()
dark.text()                 // rgb(255, 255, 255)
dark.error()

var light = Theme.light()
light.background().toCss()  // rgb(255, 255, 255)
```

Cores semânticas: `background`, `surface`, `primary`, `secondary`, `text`,
`error`.

## Semântica entre targets

- Color/Theme são **valores Int de 32 bits** — os canais são manipulados com
  bitwise; `toCss()` usa helpers idênticos em JVM, Native e JS.
- Verificável em qualquer target com `kof run`.

## Exercícios

1. Imprima `Palette.purple.toCss()` e `Color(0xFF6750A4).red()`.
2. Crie um tema customizado: `background` escuro e `primary` ciano.
3. Interprete o layout de bits de `Color(0x11223344)`: red/green/blue/alpha.
4. Use `withAlpha(0)` para criar transparente e compare com
   `Palette.transparent`.