# Módulo 00 — Fundamentos de Kof

> **Objetivo:** conhecer a sintaxe e a semântica da linguagem para ler e
> escrever programas Kof idiomáticos.

## O que é Kof?

Kof é uma linguagem **compilada**, **fortemente** e **estaticamente tipada**,
orientada a objetos, que compila para **JVM**, **nativo x86-64**, **nativo RISC-V**,
**nativo ARM64**, **JS** (ES Modules), **KofScript** e **KofC**. Um mesmo código-fonte
gera programas para múltiplos mundos:

```
Source (.kf) → Kof Compiler → Kof IR → JVM | Native | Native.risc | Native.arm | JS | KofScript | KofC
```

Sem transpilar para Java. O compilador tem lexer, parser, AST, sistema de
tipos, análise semântica, IR e geração de código próprios.

### A filosofia em uma frase

> **Menos código. Mais intenção.** O código expressa *o que* quer; a
> plataforma (linguagem + compilador + runtime + stdlib) decide *como*, por
> target e por convenção.

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

- **`docs/`** — documentação técnica (estado, arquitetura, segurança).
- **`learn/`** — trilha para humanos (numerada: 00 → 37).
- **`training/`** — corpus para LLMs (fatos, idioms, anti-patterns, exemplos).
- **`kof --help` / `kof info`** — ambiente instalado.

## Comandos da CLI

| Comando | Faz o quê |
|---------|-----------|
| `kof run a.kf [--target jvm\|native\|js\|native.risc\|native.arm] [--release] [args...]` | compila e executa |
| `kof check a.kf` | type-check sem emitir código |
| `kof build dir --target jvm\|native\|js\|native.risc\|native.arm\|kofc [--release] [--output <dir>]` | compila para target |
| `kof serve a.kf [--port N]` | sobe servidor HTTP (web.app + TLS) |
| `kof test a.kf [--target jvm\|native\|js]` | roda blocos `test "nome" { }` (PASS/FAIL por teste) |
| `kof debug a.kf` | sessão DAP (breakpoints, call stack) |
| `kof bench [paths...] [--target jvm\|native\|js] [--iterations N]` | benchmarks com validação |
| `kof profile a.kf [--target jvm\|native\|js]` | métricas de execução (CPU, RSS, GC) |
| `kof inspect a.kf [--json]` | estatísticas de IR (antes/depois otimização) |
| `kof script a.ks [--target jvm\|native\|js]` | KofScript (let top-level, repl) |
| `kof repl` | REPL KofScript interativo |
| `kof c a.c` | KofC: subset C -> ELF nativo |
| `kof lsp` | language server (LSP 3.x, stdio) |
| `kof info [--json]` | relatório do ambiente |
| `kof install <dir>` | instala distribuição local |
| `kof version` | versão do compilador |

> No curso, a maioria dos exemplos roda com `kof run`. Quando um recurso é
> específico de target, o texto avisa (ex.: `kof.db` é JVM-only hoje).

## Regra de ouro do curso

1. Todo exemplo é verificável — **rode e veja a saída**.
2. Código que compila ≠ código idiomático. Prefira sempre a forma Kof.
3. Se uma feature não existe (`Option<T>`), **não invente** — o compilador rejeita e o curso marca `WORKAROUND`.