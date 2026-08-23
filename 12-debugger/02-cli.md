# Trilha 12 · Aula 2 — kof debug (servidor DAP)

## O comando

```bash
kof debug <file.kf>
```

Inicia um **servidor DAP** (Debug Adapter Protocol) sobre **stdio**:

- breakpoints **por linha** do arquivo Kof,
- call stack com funções e linhas **Kof** (não bytecode),
- `continue` e `disconnect`.

Qualquer editor com suporte a DAP (VS Code, Neovim, Helix) pode conectar.

## Fluxo típico

1. Abra o programa no editor.
2. Configure o adapter para `kof debug <arquivo>`.
3. Marque um breakpoint numa linha suspeita.
4. Rode em modo debug.
5. Quando parar: veja a call stack e a linha atual.
6. `continue` para avançar até o próximo breakpoint.

## O que você NÃO vê

- Você não precisa ler bytecode JVM.
- A stack é mapeada de volta para as funções/linhas Kof.

## Nota

O `kof debug` é a ferramenta da distribuição (status alpha). Se o seu editor
não suportar DAP, use `kof check` + `kof test` + `log.debug` como alternativa
(que também são parte da plataforma).

## Exercício

Rode `kof debug` num programa com breakpoint e registre: linha parada, call
stack, e o que acontece ao `continue`.