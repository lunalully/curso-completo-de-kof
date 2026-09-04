# Notas — Workarounds do compilador 0.2.x (verificados até 0.2.8-beta)

> Bugs reais encontrados ao validar as soluções do curso contra a linha
> **0.2.x** (verificados executando o compilador). Todos são
> `WORKAROUND` — quando corrigidos no compilador, remova os desvios.

## 1. Campo `Long` em comparação → COMP002 (crash) — ABERTO

**Sintoma:** comparar um campo `Long` de uma classe com um literal/variável
`Int` em `if`/`while` crasha o compilador (`frame crash ... Index -1 out of
bounds`).

**Workaround:** compare com outro campo `Int`, ou copie para variável local
`Long` e use-a nos dois lados:
```kof
class R {
    Int janelaMs   // Int, não Long
}
// agora: t.quando < r.janelaMs compila
```

## 2. ~~`assert(Double == Double)` → falha de runtime~~ — CORRIGIDO ✅

Corrigido no compilador (verificado na 0.1.0): `assert(soma == 10.5)`
com `Double` funciona normalmente. Comparar por faixa segue sendo boa
prática para ponto flutuante em geral.

## 3. Método `Double` chamado como statement → VerifyError — ABERTO

**Sintoma:** `algoQueRetornaDouble()` como statement (descartando o retorno)
compila e falha em runtime (`VerifyError: Bad type on operand stack`).

**Workaround:** atribua a uma variável:
```kof
var descartado = algoQueRetornaDouble()
```

## 4. `!auth.hasRole(...)` direto no `if` de rota → VerifyError — ABERTO

**Sintoma:** `if (!auth.hasRole("admin"))` em lambda de rota web gera
bytecode inválido (VerifyError, operand stack underflow).

**Workaround:** atribua antes:
```kof
var admin = auth.hasRole("admin")
if (!admin) { ... }
```
Verificado: o padrão com atribuição funciona; a negação direta não.

## 5. `auth.user()` em handler web → derruba o servidor — ABERTO

**Sintoma:** chamar `auth.user()` dentro de um handler quebra a classe
gerada (`VerifyError`) e o servidor morre no primeiro request.

**Workaround:** trabalhe com `auth.claims()`/**`auth.hasRole(...)`**/
`auth.authenticated()` (com atribuição prévia) — `claims()` verificado
funcionando; documente a limitação de `auth.user()`.

### 5a. `return <variável>` nu em rota web → rota não registra — ABERTO

**Sintoma:** terminar o corpo de uma rota/middleware com `return x;`
(uma variável local nua) faz a rota retornar 404 — ela não registra.

```kof
app.get("/t") {
    var x = "abc"
    return x          // BUG: rota vira 404
}
```

**Workaround:** retorne uma expressão (concatenação/literal):
```kof
return "" + x       // ou monte a resposta com concatenação
```
Verificado: `jwt.create(...)` **dentro** de handler funciona quando o
retorno é concatenação (`"{\"token\": \"" + token + "\"}"`).

## 6. Resultado de `.split(...)` → bytecode inválido no JVM — ABERTO

**Sintoma:** usar o resultado de `"a,b".split(",")` (`get(0)`, `size()`)
gera classe inválida (`ClassFormatError: Illegal class name ""`). Chamar
`.split` sem usar o resultado compila.

Além disso, `split` continua **regex**: `"a|b".split("|")` quebra — use
`split("\\|")`.

**Workaround prático:** prefira `indexOf`/`substring` para parsing simples
no curso até o bug fechar.

## 7. ~~Retorno `Double` de divisão `Int` → VerifyError~~ — CORRIGIDO ✅

Corrigido no compilador 0.2.4-beta (widening de return): `return soma /
dados.size` (Int/Int) com retorno `Double` funciona normalmente.

## 8. Lambda com captura + parâmetro tipado → VerifyError — ABERTO

**Sintoma:** `(x: Int) -> x * fator` capturando variável local compila e
falha em runtime (`VerifyError: Bad local variable type`).

**Funciona (verificado):**
- captura de campo/campo estático em lambda sem parâmetros:
  ```kof
  var inc = () -> { c.n = c.n + 1 return c.n }
  ```
- lambda com parâmetro tipado **sem** captura: `(x: Int) -> x * 2`.

**Workaround:** passe o valor capturado como parâmetro, ou capture apenas
em lambdas sem parâmetros.

## 9. `Map`/`Set`: use `.size()` método, não propriedade — ABERTO

**Sintoma:** `m.size` (sem parênteses) sobre `Map`/`Set` gera bytecode
inválido. A API oficial é de métodos:

```kof
var m = mapOf()
m.put("a", 1)
m.get("a")        // 1
m.contains("a")   // true
m.remove("a")
m.keys().size()   // keys()/values() retornam List
m.clear()
m.isEmpty()

var s = setOf(1, 2)
s.add(3)
s.contains(2)
s.remove(1)
s.size()          // sempre com ()
```

## Nota estrutural: diretório = módulo

`kof run <arquivo>` compila **todos os `.kf` do diretório** do arquivo como
um único módulo — dois `main()` no mesmo diretório viram
`PKG002: module has N main() functions`. Um programa = um diretório.

## Regra do curso

Sempre que usar um workaround: **marque `WORKAROUND`** e cite a versão.
Quando o bug for corrigido, o exemplo oficial é atualizado e o rótulo removido.
