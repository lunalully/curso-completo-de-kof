# Notas — Workarounds e contratos verificados (linha 0.2.x–0.4.x)

> Bugs reais encontrados ao validar as soluções do curso, **re-verificados
> executando o compilador 0.4.1-beta** (15/09). A maioria dos workarounds
> antigos foi fechada pelo compilador — ficam aqui como registro histórico e
> para quem estuda com versões velhas. No fim, os **contratos novos** da
> 0.3.8–0.4.x que todo código do curso segue.

## Workarounds antigos — FECHADOS pelo compilador ✅

| # | Workaround (sintoma original) | Status |
|---|-------------------------------|--------|
| 1 | Campo `Long` em comparação → crash `COMP002` | ✅ CORRIGIDO (0.4.x) — `t < campoLong` compila e roda; widening Int→Long abençoado no IR |
| 2 | `assert(Double == Double)` | ✅ CORRIGIDO (0.1.0) — comparar por faixa segue boa prática FP |
| 3 | Método `Double` como statement → `VerifyError` | ✅ CORRIGIDO (0.4.x) — descarte de retorno largo funciona (POP2 corrige) |
| 4 | `!auth.hasRole(...)` direto no `if` de rota → `VerifyError` | ✅ CORRIGIDO (0.4.x) — verificado ao vivo: rota com `if (!auth.hasRole("admin"))` responde `{"ok": true}` com token válido |
| 5 | `auth.user()` em handler derrubava o servidor | ✅ CORRIGIDO (0.4.x) — verificado ao vivo: `auth.user()` devolve o `sub` dentro do handler |
| 5a | `return <variável>` nu em rota → 404 | ✅ CORRIGIDO (0.3.x, bug #28/§53 — varredura recursiva de `ReturnStmt`) |
| 6 | Resultado de `.split(...)` → bytecode inválido (`ClassFormatError: Illegal class name ""`) | ✅ CORRIGIDO (0.3.x, bug #30/§56) — **mas `split` continua regex**: `"a|b|c".split("|")` divide carácter-a-carácter (5) — use `split("\\|")` |
| 7 | Retorno `Double` de divisão `Int` → `VerifyError` | ✅ CORRIGIDO (0.2.4) |
| 8 | Lambda com captura + parâmetro tipado → `VerifyError` | ✅ CORRIGIDO (0.3.0) |
| 9 | `m.size`/`s.size` (propriedade) em `Map`/`Set` → `NoSuchFieldError` | ✅ CORRIGIDO (0.3.x, bug #14) — verificado na 0.4.1: `m.size`, `l.size` e `s.size()` todos funcionam; a API de métodos continua a forma oficial |

## Nota estrutural: diretório = módulo (continua verdade)

`kof run <arquivo>` compila **todos os `.kf` do diretório** do arquivo como
um único módulo — dois `main()` no mesmo diretório viram
`PKG002: module has N main() functions`. Um programa = um diretório.

## Contratos novos da 0.3.8–0.4.x (não são bugs — são a linguagem)

Estes diagnósticos existem de propósito (paridade absoluta, regra R6 — nunca
silêncio). O código idiomático do curso já os segue:

- **SEM053** — `<`/`>`/`<=`/`>=` em `String` é **rejeitado** (a ordem
  divergia por target). Use `a.compareTo(b) < 0`. (Ordem lexicográfica:
  `compareTo` nos 5 alvos, paridade JDK.)
- **SEM028** — array **não tem** `.get()`/`.size()`/`.add()`; array é
  fixo: use `arr[i]` e `arr.length`. `split` devolve `String[]` (array, não
  `List`) — `parts[0]`, `parts.length`.
- **SEM054** — subscript `x[i]` em `String`/`Map`/`Set` é rejeitado: use
  `.charAt(i)`, `.get(k)`, ou itere. **Array e `List` aceitam `x[i]`**
  (além de `.get(i)`).
- **SEM048/SEM049** — null safety apertada: literal `null` em primitivo não
  nullable é erro; `Map.get` devolve `V?` sempre — deref sem narrowing é
  erro no compile-time (SEM049 reporta a posição real).
- **SEM046** — `private`/`protected` validados em compile-time.
- **SEM047** — duas funções top-level com a MESMA assinatura é erro;
  assinaturas diferentes coexistem (sobrecarga top-level, oracle JVM).
- **SEM061** — dois métodos com a mesma assinatura na classe: rejeitados na
  declaração.
- **SEM058** — `for (var c in "string")` é rejeitado (iterar String não é
  coleção).
- **`header()`/`query()` no `kof.web` agora são `String?`** — narrowing
  obrigatório antes do uso.
- **Literal `Long` no limite**: `9223372036854775807` é o máximo; `-…08`
  só como expressão (`-big - 1`).

## Regra do curso

Sempre que usar um workaround: **marque `WORKAROUND`** e cite a versão.
Quando o bug for corrigido, o exemplo oficial é atualizado e o rótulo
removido. As 180 soluções do curso passam `kof check` e as do módulo 00
passam `kof run` na 0.4.1-beta sem nenhum workaround ativo.
