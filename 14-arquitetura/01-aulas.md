# Trilha 14 · Aulas

## Aula 01 — Clean architecture na filosofia Kof

Princípios clássicos que **Kof preserva**:

| Princípio | Em Kof |
|-----------|--------|
| Dependência para dentro (domínio no centro) | packages + funções top-level |
| Independência de framework | a stdlib É a plataforma; sem container |
| Testabilidade | `kof test` + funções puras |
| Separação de responsabilidades | record (dados) / classe (comportamento) / função (lógica) |

O que Kof **remove**:
- Controller/Service/Repository **sem necessidade** → função top-level.
- Injeção de dependência → funções e estado de módulo cobrem composição.
- Annotations/AOP → middleware e composição de funções.

## Aula 02 — Modelagem de domínio

> Dados → record. Comportamento → classe. Lógica → função.

```kof
// dominio
record Reserva(String id, String sala, Int horario)

class Salas {
    List<String> nomes
    constructor() {
        nomes = listOf()
    }
    Bool existe(String nome) {
        return nomes.contains(nome)
    }
}

// aplicacao (lógica sem estado)
Bool conflita(List<Reserva> reservas, Reserva nova) {
    for (var r in reservas) {
        if (r.sala == nova.sala && r.horario == nova.horario) {
            return true
        }
    }
    return false
}
```

## Aula 03 — Módulos e fronteiras

```kof
package dominio
package aplicacao
package infra
```

- `package`/`import` existem em Kof.
- Programas pequenos: um arquivo basta. Ao crescer, separe por **direção de
  dependência**: `aplicacao` importa `dominio`; `infra` importa `aplicacao`.
- **Regra:** nunca dependa de um módulo mais "externo" (framework/HTTP/db) a
  partir do domínio. O domínio usa apenas tipos da linguagem.

## Aula 04 — Ports & Adapters sem container

O "hexagonal" sem DI:

```kof
// port (abstração)
interface Armazenamento {
    salvar(String chave, String valor)
    carregar(String chave): String
}

// adapter real (kof.io)
class ArmazenamentoEmArquivo implements Armazenamento {
    String pasta
    constructor(String pasta) {
        this.pasta = pasta
    }
    void salvar(String chave, String valor) {
        writeFile(pasta + "/" + chave + ".txt", valor)
    }
    String carregar(String chave) {
        return readFile(pasta + "/" + chave + ".txt")
    }
}

// composição sem container: construa o grafo no main()
main() {
    var storage = ArmazenamentoEmArquivo("data")
    storage.salvar("usuario-1", "Mel")
    println(storage.carregar("usuario-1"))
}
```

A "injeção" é só **passar o objeto** no construtor/função. Nada de reflection.

## Aula 05 — O que Kof NÃO precisa

| Mecanismo | Por que não precisa |
|-----------|---------------------|
| Container de beans / DI | funções + estado de módulo cobrem composição |
| AOP | middleware (`app.use`) + composição de funções |
| Annotations | o compilador sabe o que o programa usa |
| SpEL/expression language | funções de primeira classe |
| Config em XML/yml | `kof.config` (arquivo > env > profile) |

**Decisão arquitetural do projeto:** DI é NOT APPLICABLE no núcleo — reavaliar
apenas se surgir necessidade real (plugins/SPI).

## Checkpoint detalhado

1. Modelo `Reserva` + `Salas` + `conflita` (acima).
2. Separe em 3 arquivos/pacotes.
3. Explique por escrito: por que o domínio não depende de HTTP/db.
4. Refatore um Controller/Service/Repository trivial para função top-level.