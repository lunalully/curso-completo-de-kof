# Trilha 12 · Aula 1 — Mentalidade de depuração

## Os 3 tipos de erro

| Tipo | Exemplo | Detectado |
|------|---------|-----------|
| Compilação | tipo errado, sintaxe | `kof check` / `kof build` |
| Runtime | array out of bounds, null | `kof run` + mensagem |
| Lógico | média com `- 1` indevido | **testes** / depurador |

## O processo (sem pânico)

1. **Reproduza** — rode e veja o erro (ou a saída errada).
2. **Leia a mensagem** — arquivo, linha, mensagem. O runtime aponta o local.
3. **Hipótese** — o que você *acha* que está errado?
4. **Verifique** — `log.debug`, `println`, breakpoint.
5. **Corrija** e **regride** com `kof test`.

## Regra de ouro

> O erro de runtime dá a linha. O erro de lógica dá um número errado.
> Para lógica, o depurador + testes são seus amigos — não adivinhe.