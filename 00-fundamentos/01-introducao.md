# Módulo 00 — Fundamentos de Kof

> **Objetivo:** conhecer a sintaxe e a semântica da linguagem para ler e
> escrever programas Kof idiomáticos.

## O que é Kof?

Kof é uma linguagem **compilada**, **fortemente** e **estaticamente tipada**,
orientada a objetos, que compila para **JVM**, **nativo x86-64** e **JS**
(ES Modules). Um mesmo código-fonte gera programas para os três mundos:

```
Source (.kf) → Kof Compiler → Kof IR → JVM | Native | JS
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
| `kof run a.kf` | compila e executa (JVM) |
| `kof check a.kf` | type-check sem emitir código |
| `kof build dir --target jvm\|native\|js` | compila para um target |
| `kof serve a.kf [--port N]` | sobe servidor HTTP |
| `kof test a.kf` | roda testes (exit code = PASS/FAIL) |
| `kof lsp` | language server (LSP 3.x, stdio) |
| `kof info` | relatório do ambiente |

> No curso, a maioria dos exemplos roda com `kof run`. Quando um recurso é
> específico de target, o texto avisa (ex.: `kof.db` é JVM-only hoje).

## Regra de ouro do curso

1. Todo exemplo é verificável — **rode e veja a saída**.
2. Código que compila ≠ código idiomático. Prefira sempre a forma Kof.
3. Se uma feature não existe (`Map`, `Set`, `map()`, captura em lambda),
   **não invente** — o compilador rejeita e o curso marca `WORKAROUND`.