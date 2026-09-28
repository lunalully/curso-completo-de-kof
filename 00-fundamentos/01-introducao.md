# Módulo 00 — Fundamentos de Kof

> **Objetivo:** conhecer a sintaxe e a semântica da linguagem para ler e
> escrever programas Kof idiomáticos.

## O que é Kof?

Kof é uma linguagem **compilada**, **fortemente** e **estaticamente tipada**,
orientada a objetos, que compila para **JVM**, **nativo x86-64**, **nativo RISC-V**,
**nativo ARM64**, **JS** (ES Modules), **KofScript** (execução direta via `KofInterpreter`) e **KofC** (subset C → ELF). Um mesmo código-fonte
gera programas para múltiplos mundos:

```
Linguagem Kof (definida pela especificação)
      │
Kof Compiler (uma implementação)
      │
   Kof IR (máquina de pilha linear, 30 ops)
      │
 ┌────┼───────────────┬──────────────┐
JVM  Native (x86_64/riscv64/aarch64)  JS
                KofScript (IR)  KofC (C→ELF)
```

**A linguagem não muda. O target muda.** `KofScript` não é JavaScript (`let`/`const`/`async`/`fn` não existem — dão `PARSE085`).

Sem transpilar para Java. O compilador tem lexer, parser, AST, sistema de
tipos, análise semântica, IR e geração de código próprios.

### A filosofia em uma frase

> **Menos código. Mais intenção.** O código expressa *o que* quer; a
> plataforma (linguagem + compilador + runtime + stdlib) decide *como*, por
> target e por convenção.

> **Kof é orientada à intenção — o extremo da OO, não um paradigma formal.** A cadeia é `intenção → Kof → compilador → backend`. Você escreve `spawn tarefa()` (não `Thread`), `app.get("/users/:id")` (não servlet container), `json.decode<User>(body)` (não parser manual), `Palette.red` (não `0xFF0000FF`). Se um target não consegue realizar a intenção, ele diagnostica em compile-time com código de gap (`CONC001`, `DB001`, `SECN000`...) — nunca silenciosamente. Ver `docs/philosophy.md` e `training/idioms/`.

## Seu primeiro programa

```kof
main() {
    println("Hello, Kof!")
}
```

Rode:

```bash
kof run hello.kf
```

Observações importantes:

- `main()` é o ponto de entrada — a única função sem tipo de retorno.
- **Não existe a palavra-chave `fun`.** Funções são declaradas pelo nome.
- `println` é parte do núcleo da linguagem (`kof.core`).

## Onde as coisas moram

- **`docs/`** — documentação técnica (estado, arquitetura, segurança, performance, debugging, distribuição). A separação **linguagem ≠ compilador ≠ target** é o eixo (ver `docs/language-reference/` e `docs/compiler-architecture.md`).
- **`learn/`** — trilha para humanos (numerada: 00 → 39_stdlib, + native/).
- **`training/`** — corpus para LLMs (fatos, idioms, anti-patterns, exemplos, migration, tooling, releases).
- **`kof --help` / `kof info`** — ambiente instalado.

## Comandos da CLI

| Comando | Faz o quê |
|---------|-----------|
| `kof run a.kf [--target jvm\|native\|js\|native.risc\|native.arm\|android] [--profile host\|freestanding] [--release] [args...]` | compila e executa |
| `kof check a.kf\|dir [--target t] [--json]` | type-check sem emitir código |
| `kof build dir --target jvm\|native\|js\|native.risc\|native.arm\|android [--release] [--fat] [--print-sizes] [--output <dir>]` | compila para target |
| `kof serve a.kf [--port N] [--host H] [--backend t] [--frontend t]` | sobe servidor HTTP |
| `kof test a.kf\|dir [--target jvm\|native\|js] [--timeout <s>] [--tag <t>]` | roda `test "nome", "tag" { }` (PASS/FAIL por teste) |
| `kof debug a.kf [--dap] [--attach <pid>] [--target native] [--break <linha>]` | DAP/breakpoints (0.4.9+: nativo) |
| `kof bench [paths...] [--target jvm\|native\|js] [--iterations N] [--warmup N] [--baseline f] [--json] [--quick]` | benchmarks com baseline e regressão |
| `kof profile a.kf [--target jvm\|native\|js] [--methods]` | métricas de execução (CPU, RSS, GC, métodos) |
| `kof inspect a.kf [--json]` | estatísticas de IR (antes/depois otimização) |
| `kof fmt a.kf\|dir [-w]` | formatador oficial (parser real, idempotente) |
| `kof md check\|format a.md` | valida/canoniza Kofmd (MD002) |
| `kof script a.ks [--target jvm\|native\|js] [--watch] [--inspect]` | KofScript: Kof puro via KofInterpreter (sem let/const/fn) |
| `kof repl` | REPL KofScript interativo (Kof puro) |
| `kof c a.c [--target x86_64\|riscv64\|aarch64] [--run]` | KofC: subset C -> ELF nativo |
| `kof new <dir> [--type mono\|backend\|frontend\|full-stack]` | esqueleto de projeto |
| `kof deps <init\|add\|remove\|list\|resolve>` | gerenciador de pacotes (`kofdeps`) |
| `kof workflow <list\|run> a.kf [--job <nome>] [--dry-run]` | roda pipelines (`pipeline(): KofWfDag`) |
| `kof deploy dir --target jvm [--publish]` | empacota release (fat jar + checksums) |
| `kof migrate \| translate \| decompile \| compare` | migração/inspeção de legado Java/.class |
| `kof editor <list\|detect\|setup\|install>` | integração com editores (EDI001) |
| `kof config gen a.kf [--output <f>]` | gera template `kof.config` das chaves usadas |
| `kof lsp` | language server (LSP 3.x, stdio) |
| `kof info [--json]` | relatório do ambiente |
| `kof install <dir>` | instala distribuição local |
| `kof version` | versão do compilador |

> No curso, a maioria dos exemplos roda com `kof run`. Quando um recurso é
> específico de target, o texto avisa (ex.: `kof.web` no Android é `AND002`).

## Regra de ouro do curso

1. Todo exemplo é verificável — **rode e veja a saída**.
2. Código que compila ≠ código idiomático. Prefira sempre a forma Kof.
3. Se uma feature não existe (`Option<T>`), **não invente** — o compilador rejeita e o curso marca `WORKAROUND`.