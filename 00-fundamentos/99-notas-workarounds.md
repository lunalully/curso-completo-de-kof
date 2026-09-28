# Notas — Workarounds do compilador 0.2.x–0.5.0 (verificados até 0.5.0-beta)

> Bugs reais encontrados ao validar as soluções do curso contra a linha
> **0.2.x–0.5.0** (verificados executando o compilador). Todos são
> `WORKAROUND` — quando corrigidos no compilador, remova os desvios. A
> varredura de setembro/2026 contra a **0.5.0-beta** fechou a maioria dos
> itens; os que restam estão marcados abaixo.

## 1. ~~Campo `Long` em comparação → COMP002 (crash)~~ — CORRIGIDO ✅ (0.5.0)

`if (t < r.janelaMs)` com campo `Long` e literal `Int` compila e roda
normalmente (re-testado na 0.5.0).

## 2. ~~`assert(Double == Double)` → falha de runtime~~ — CORRIGIDO ✅

Corrigido no compilador (verificado na 0.1.0): `assert(soma == 10.5)`
com `Double` funciona normalmente. Comparar por faixa segue sendo boa
prática para ponto flutuante em geral.

## 3. ~~Método `Double` chamado como statement → VerifyError~~ — CORRIGIDO ✅ (0.5.0)

`calc()` (retorno `Double`) como statement roda sem erro (re-testado na
0.5.0).

## 4. `!auth.hasRole(...)` direto no `if` de rota — NÃO RE-TESTADO na 0.5.0

**Sintoma histórico:** `if (!auth.hasRole("admin"))` em lambda de rota web
gerava bytecode inválido (VerifyError, operand stack underflow).

**Padrão seguro (continua recomendado):** atribua antes:
```kof
var admin = auth.hasRole("admin")
if (!admin) { ... }
```
Este item depende de um app com `auth.*` configurado; não foi reproduzido
nesta varredura. Se você o encontrar na 0.5.0, registre aqui com o trecho.

## 5. `auth.user()` em handler web — NÃO RE-TESTADO na 0.5.0

**Sintoma histórico:** chamar `auth.user()` dentro de um handler quebrava a
classe gerada (VerifyError) e o servidor morria no primeiro request.

**Padrão seguro:** use `auth.claims()`/`auth.hasRole(...)`/
`auth.authenticated()` (com atribuição prévia). Também não re-testado nesta
varredura (requer auth configurado).

### 5a. ~~`return <variável>` nu em rota web → rota não registra~~ — CORRIGIDO ✅ (0.5.0)

`return x` (variável local nua) em rota agora responde **200** com o valor
(re-testado na 0.5.0). Não é mais preciso concatenar (`return "" + x`) —
mas concatenar continua válido quando você quer montar a resposta.

## 6. ~~Resultado de `.split(...)` → bytecode inválido no JVM~~ — CORRIGIDO ✅ (0.5.0)

`"a,b,c".split(",")` compila e roda; o retorno é **array** (`String[]`),
então use `.length` e `[i]` — `size()`/`get()` dão `SEM028` com a dica
certa. `split` continua **regex**: `"a|b".split("\\|")` (escape
necessário).

## 7. ~~Retorno `Double` de divisão `Int` → VerifyError~~ — CORRIGIDO ✅

Corrigido no compilador 0.2.4-beta (widening de return): `return soma /
dados.size` (Int/Int) com retorno `Double` funciona normalmente.

## 8. ~~Lambda com captura + parâmetro tipado → VerifyError~~ — CORRIGIDO ✅ (0.3.0)

Corrigido: lambda com captura + parâmetro tipado funciona nos 5 alvos, incluindo lambdas aninhados 3 níveis. Ver `00-fundamentos/04-funcoes-e-lambdas.md`.

## 9. ~~`Map`/`Set`: use `.size()` método, não propriedade~~ — CORRIGIDO ✅ (0.5.0)

`m.size` (propriedade) funciona para `List`, `Map` e `Set` na 0.5.0; a API
de métodos (`size()`, `get`, `put`, `contains`, `remove`, `keys`, `values`,
`isEmpty`, `clear`) segue estável e é a forma documentada pela linguagem.

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
s.size()
```

## 10. `math.sqrt(divisão mista)` → SEM025 (0.5.0) — WORKAROUND

**Sintoma:** `math.sqrt(soma / dados.size)` (Double / Int) falha com
`Cannot resolve method 'sqrt' on namespace 'math'` — a resolução do overload
não aplica widening no argumento de uma divisão mista.

**Workaround:** materialize o argumento num `Double`:

```kof
var variancia: Double = soma / dados.size
return math.sqrt(variancia)
```

`math.sqrt(variancia(dados))` (argumento já `Double`) funciona direto.

## Nota estrutural: diretório = módulo

`kof run <arquivo>` compila **todos os `.kf` do diretório** do arquivo como
um único módulo — dois `main()` no mesmo diretório viram
`PKG002: module has N main() functions`. Um programa = um diretório.
(Confirmado de novo na 0.5.0.)

## Nota: recursos não fechados agora avisam (0.5.0)

A 0.5.0 traz diagnósticos de memória/recurso: `web.app()` e `db.connect()`
nunca fechados emitem **aviso `MEM014`** no compile (`app.close()` resolve);
`MEM001`/`MEM002`/`MEM013`/`MEM022` cobrem close duplo, alias após claim,
retorno de binding claimado e mutação durante iteração (aviso). Não são
erros — são a plataforma dizendo o que você esqueceu.

## Regra do curso

Sempre que usar um workaround: **marque `WORKAROUND`** e cite a versão.
Quando o bug for corrigido, o exemplo oficial é atualizado e o rótulo removido.
