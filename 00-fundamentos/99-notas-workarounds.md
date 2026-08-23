# Notas — Workarounds do compilador 0.0.8-alpha

> Bugs reais encontrados ao validar as soluções do curso. Todos são
> `WORKAROUND` — quando corrigidos no compilador, remova os desvios.

## 1. Campo `Long` em classes → COMP002 (crash)

**Sintoma:** carregar um campo `Long` de uma classe (`this.meuCampoLong`) em
comparação/retorno crasha o compilador (`Internal compiler error: Index -1
out of bounds`).

**Workaround:** use campo `Int` e compare com `Long` (funciona):
```kof
class R {
    Int janelaMs   // Int, não Long
}
// agora - t.quando < janelaMs   // Long expr < Int field: OK
```

## 2. `assert(Double == Double)` → falha de runtime

**Sintoma:** `assert(soma == 20.5, "msg")` com `Double` falha em runtime
(a mensagem "JavaFX runtime components not found" é um falso positivo do
launcher).

**Workaround:** compare por faixa OU por string OU com Int:
```kof
var t = total()
assert(t >= 20.4 && t <= 20.6, "total")   // faixa
```

## 3. Método `Double` chamado como statement → falha de runtime

**Sintoma:** `algoQueRetornaDouble()` como statement (descartando o retorno)
falha em runtime.

**Workaround:** atribua a uma variável:
```kof
var descartado = algoQueRetornaDouble()
```

## 4. `!auth.hasRole(...)` direto no `if` → VerifyError

**Sintoma:** `if (!auth.hasRole("admin"))` em lambda de rota web gera
bytecode inválido (VerifyError).

**Workaround:** atribua antes:
```kof
var admin = auth.hasRole("admin")
if (!admin) { ... }
```

## 5. `auth.user()` / `auth.claims()` → falha de runtime

**Sintoma:** `auth.user()` falha em runtime no 0.0.8. `auth.authenticated()`
e `auth.hasRole(...)` funcionam.

**Workaround:** use `auth.authenticated()` para confirmar identidade;
documente a limitação para `auth.user()`.

## 6. `split("|")` usa regex

`"a|b".split("|")` quebra (regex). Use `split("\\|")`.

## Regra do curso

Sempre que usar um workaround: **marque `WORKAROUND`** e cite a versão.
Quando o bug for corrigido, o exemplo oficial é atualizado e o rótulo removido.
## 7. Retorno `Double` de divisão Int → falha de runtime

**Sintoma:** `return soma / dados.size` (Int/Int) com retorno `Double`
falha em runtime.

**Workaround:** force divisão Double com `* 1.0`:
```kof
return (soma * 1.0) / dados.size
```
