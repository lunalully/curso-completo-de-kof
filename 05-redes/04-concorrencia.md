# Módulo 05 · Aula 4 — Concorrência com `spawn`

> Trabalho independente em paralelo, sem expor threads. Em Kof: `spawn`.

## Básico

```kof
void processar(Int id) {
    println("processando " + id)
}

main() {
    spawn processar(1)
    spawn processar(2)
    spawn {
        println("background")
    }
    println("fim")
}
```

## Semântica real (verificada)

- A tarefa roda **em paralelo** (JVM: virtual threads).
- O programa **espera as tarefas antes de sair** (join implícito).
- O retorno da função é descartado (fire-and-forget).
- Exceção na tarefa **não derruba** o programa.
- **Native ainda não suporta** (diagnostic `CONC001`) — use JVM.

## Quando usar

- Processamento independente em paralelo (filas, I/O, notificações).
- Tarefas de background.

## Quando NÃO usar

- Quando o resultado é necessário no fluxo principal (`await` é planned).
- Quando a ordem importa.

## BAD — expor plataforma

```kof
// NÃO EXISTE — não há Thread/Executor na linguagem
var t = new Thread(() -> work())
t.start()
```

## GOOD

```kof
spawn work()
```

## WHY

`spawn` expressa intenção. Thread/Runnable/Executor são mecanismos — a
decisão de como executar pertence ao runtime.

## Exemplo: download/processamento em paralelo

```kof
record Resultado(Int id, Bool ok)

void processarLote(Int inicio, Int fim) {
    for (var i = inicio; i < fim; i = i + 1) {
        println("item " + i + " processado")
    }
}

main() {
    spawn processarLote(0, 50)
    spawn processarLote(50, 100)
    println("lote disparado")
}
```

## Limitações honestas

- Resultado observável de tarefa: **planned** (`await`/join explícito).
- Filas produtor/consumidor: **planned** (`kof.concurrent.Queue`).
- Native: **CONC001** — use o target JVM por enquanto.
- Lambdas sem captura (mesma limitação das lambdas).

## Exercícios

1. Dispare 3 `spawn` e confirme o join implícito (o programa espera).
2. Processe uma lista de 100 itens em 4 lotes paralelos.
3. (Reflexão) Por que o Kof esconde Thread? Que problema isso resolve?
4. Compile para o target native um programa com `spawn` e observe o
   diagnostic `CONC001`.