# Trilha 07 · Exercícios

> ★ básico · ★★ intermediário · ★★★ avançado. Rode Color/Theme com `kof run`;
> widgets compile para JS (`kof build --target js`).

## Cores e temas (aula 01)

- ★ [01-cores.kf](solucoes/01-cores.kf) — `Color(255,0,0)`, canais, `isOpaque`, `toCss`.
- ★ [02-palette.kf](solucoes/02-palette.kf) — imprima `Palette.green`, `Palette.transparent`, `Palette.purple`.
- ★★ [03-tema.kf](solucoes/03-tema.kf) — `Theme.dark()`/`light()`, cores semânticas, `isDark`.
- ★★ [04-bitpack.kf](solucoes/04-bitpack.kf) — decodifique `Color(0x11223344)`: red/green/blue/alpha.
- ★★★ [05-tema-custom.kf](solucoes/05-tema-custom.kf) — defina uma paleta própria com `static Color` e um `Style`.

## Widgets e layout (aulas 02-03)

- ★★ [06-contador.kf](solucoes/06-contador.kf) — janela com contador (botão +1, label). Compile para JS.
- ★★ [07-entrada.kf](solucoes/07-entrada.kf) — `Input` + `Label` (compile para JS).
- ★★★ [08-layout.kf](solucoes/08-layout.kf) — `Column`/`Row` com labels e botão (compile para JS).
- ★★★ [09-style.kf](solucoes/09-style.kf) — `View`+`Style` com background e padding (compile para JS).

> **Nota real:** a sintaxe exata de `Style(...)`, `Column.add(...)` e listas
> literais depende da versão do `kof.ui` (em evolução). **Teste sempre** o que
> compila; a estrutura conceitual (objetos, composição, estilo por objeto) é
> a parte estável. Se algo não compilar, ajuste e documente.

## KofJS (aula 04)

- ★★ [10-kofjs-basico.kf](solucoes/10-kofjs-basico.kf) — um programa com classes/lambdas que compila para JS (`kof build --target js`).
- ★★★ compile o contador (06) para JS e rode no browser/webview.

## Desafio integrador

[11-integrador.kf](solucoes/11-integrador.kf) — uma **lista de tarefas visual**:
- `record Tarefa(String titulo, Bool concluida)`.
- janela com `Input` + botão "adicionar" + labels por tarefa.
- botão "concluir" que marca e atualiza o label.
- Compile para JS sem erros.

> Se a API de widgets da sua versão não cobrir algo, implemente o que der e
> marque o restante como `WORKAROUND` — o objetivo é o fluxo completo.