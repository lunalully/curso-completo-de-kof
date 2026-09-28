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
- **Native suporta** via pthread (CONC001 fechado, 0.3.0-beta) — use `kof run --target native`.

## Quando usar

- Processamento independente em paralelo (filas, I/O, notificações).
- Tarefas de background.

## Quando NÃO usar

- Quando a ordem importa.

## Resultado de tarefa: `spawn` expressão + `await` (0.1.0+)

Quando o resultado **é** necessário no fluxo principal, capture o handle:

```kof
Int trabalho() {
    return 42
}

main() {
    var r = spawn trabalho()
    println(await r)        // 42
}
```

- `spawn f()` retorna um handle tipado; `await r` espera e devolve o valor
  (JVM: virtual threads).
- Statement puro (`spawn tarefa()`) continua fire-and-forget com join
  implícito no fim do programa.

## Verificação não-bloqueante: `poll` / `done` (0.2.0+)

```kof
val r = spawn trabalho()
if (done(r)) {
    println("pronto: " + poll(r))
}
```

- `poll(r)` devolve o valor se pronto; **default do tipo** (0/false) para
  primitivos não-prontos, `null` para referências. Use `done()` para
  distinguir "não pronto" de um valor default.
- `done(r)` → `Bool`.

## Exceções atravessam `await`

A exceção lançada dentro da tarefa chega **com a mensagem original** no
ponto do `await`:

```kof
Int quebra() { throw "boom" }

main() {
    val r = spawn quebra()
    try {
        await r
    } catch (String e) {
        println(e)   // "boom"
    }
}
```

## Cancelamento cooperativo (0.2.0+)

```kof
Int trabalho() {
    var i = 0
    while (i < 10000 && !cancelled()) {
        time.sleep(1)
        i++
    }
    return i
}

main() {
    val r = spawn trabalho()
    time.sleep(30)
    assert(cancel(r))       // marca a tarefa
    await r                 // a tarefa sai do loop cedo
}
```

- `cancel(r)` marca o handle; **a tarefa decide quando sair** consultando
  `cancelled()` dentro do próprio corpo.
- `cancelled()` fora de uma tarefa devolve `false`.

## `selectAny` — primeiro que chegar (0.2.0+)

```kof
val a = spawn lenta()      // 300ms
val b = spawn rapida()     // imediata
println(selectAny(a, b))   // valor da rapida
```

Bloqueia até **qualquer** handle completar e devolve o valor dele.

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

## Processos externos: `process.run` e `process.spawn` (0.2.4+)

`process.run` executa um processo externo e bloqueia até ele terminar:

```kof
var r = process.run("git", "status")
println(r.stdout)            // saída padrão
println(r.stderr)            // saída de erro
println(r.exitCode)          // exit != 0 é DADO, nunca exceção
```

`process.spawn` lança um processo com **stdin/stdout vivos**:

```kof
var h = process.spawn("sh", "-c", "read x; echo got=$x")
h.write("hello\n")           // envia para stdin (no cross, no-op honesto)
var line = h.readLine()      // lê stdout
println(h.alive())
h.kill()                     // destrói o processo
```

- `process.run`/`process.spawn`: ✅ JVM, Native x86/riscv64/aarch64 e JS
  (sob o host Kof; `node` puro recusa com diagnóstico nomeado).
- `process.exit(code)`: ✅ todos os alvos com runtime de processo.
- **`kof.shell`** é a forma idiomática para argv dinâmico/pipeline:
  `shell.run("git", listOf("status"))`, `shell.ok(r)`,
  `shell.pipeline(listOf(listOf("echo", "a b"), listOf("wc", "-w")))` —
  argv é **lista**, nunca string concatenada (anti-injeção).
- **`kof.ssh`**: `ssh.run(host, cmd)`/`ssh.cmd`/`ssh.ok` no JVM e no cross.

## Limitações honestas (0.5.0)

- Filas produtor/consumidor: `kof.mq` (`queue/push/pop`, `mq.publish/
  subscribe`) — 3 targets (MQ001 fechado).
- Native: **CONC001 fechado** (pthread) — spawn/await real; canais
  `channel<T>()` portados para riscv64/aarch64 na 0.5.0.
- JS: **CONC003 fechado** — async/await/Promise. `cancelled()` no JS
  ainda devolve `false` (sem thread-local da task) e o `sleep` é
  cooperativo (§132): espere timers com um `sleep` longo, não polling curto.
- Exceção dentro da task atravessa `await` com a mensagem original
  (frame por thread no Native desde a 0.4.1).
- `selectAny`/`awaitTimeout` para esperar "o primeiro que chegar" ou limitar
  a espera.

## Exercícios

1. Dispare 3 `spawn` e confirme o join implícito (o programa espera).
2. Processe uma lista de 100 itens em 4 lotes paralelos.
3. Use `spawn` expressão + `await` para somar resultados de duas tarefas.
4. Compile para o target native um programa com `spawn` e confirme que funciona
   via pthread (CONC001 fechado, 0.3.0-beta).