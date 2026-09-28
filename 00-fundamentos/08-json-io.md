# Módulo 00 · Aula 8 — JSON e kof.io

> Serialização e filesystem fazem parte da plataforma — sem bibliotecas externas.

## JSON

```kof
// encode — objeto/record no JVM
var p = Point(3, 4)
var j = json.encode(p)             // {"x":3,"y":4}

// encode de listas/primitivos (funciona em todos os targets)
var l = json.encode(listOf(1, 2, 3))
println(l)                          // [1,2,3]

// decode tipado (records no JVM)
var d = json.decode<Point>("{\"x\": 10, \"y\": 20}")
println(d.x())                      // 10
```

**Novidades 0.4.0–0.5.0:**
- `json.encode(1.5)` e coleções com ponto flutuante compilam nos 5 alvos
  (o antigo `JSN001` fechou; no cross, elementos `Double`/`Long` têm caminho
  próprio — 0.5.0).
- `json.encode(mapOf(...))` emite chaves **ordenadas** (0.4.0), e strings
  seguem o escape do Jackson (0.5.0).
- `json.decode<Map<String, T>>` com `Map` aninhado e `List<Record>` fora da
  ordem de fonte funcionam (0.5.0); record dentro de `package` também.
- Record sem schema no JS continua gap honesto (`JSN002`).

## Filesystem — `kof.io`

`File`, `Path` e `Directory` representam caminhos; as operações são as mesmas
para os três. O backend resolve POSIX/`java.nio` para você.

```kof
// ler e escrever texto (UTF-8 sempre)
var ok = writeFile("data.txt", "kof io funciona")
var conteudo = readFile("data.txt")
println(conteudo)
```

### Path

| Operação | Resultado |
|----------|-----------|
| `Path("a").resolve("b")` | `a/b` |
| `Path("a/b").parent()` | `a` (ou null) |
| `Path("a/b.txt").fileName()` | `b.txt` |
| `Path("a/b.txt").extension()` | `txt` |
| `Path("a/./b/../c").normalize()` | `a/c` |

### File

| Operação | Comportamento |
|----------|---------------|
| `File("x").exists()` | Bool |
| `File("x").readText()` | String UTF-8; `null` se falhar |
| `File("x").writeText(s)` / `.appendText(s)` | Bool |
| `File("x").readBytes()` | `Int[]` (0-255); `null` se falhar |
| `File("x").writeBytes(b)` | Bool |
| `File("x").size()` | Long; **erro `file not found: <path>`** se não existir (JVM e Native, 0.5.0) |
| `File("x").delete()` | Bool (recursivo se diretório) |
| `File("x").copyTo(dest)` / `.moveTo(dest)` | Bool |
| `File("x").modifiedTime()` / `.isSymlink()` | Long / Bool (0.4.5+) |
| `File("x").readRange(inicio, fim)` | `Int[]` de bytes — leitura parcial (`fim` exclusivo) |

Estáticas: `File.exists(p)`, `File.readText(p)`, `File.writeText(p, s)`, ...

### Directory

| Operação | Comportamento |
|----------|---------------|
| `Directory("d").exists()` | Bool |
| `Directory("d").create()` | cria; falha se já existe |
| `Directory("d").createDirectories()` | cria recursivamente |
| `Directory("d").list()` | `List<String>` dos nomes (ordenado) |
| `Directory("d").delete()` | remove diretório vazio |

```kof
for (var entry in Directory("data").list()) {
    println(entry)
}
```

### Exemplo completo

```kof
var path = Path("data/users.txt")
path.parent().createDirectories()
path.writeText("Mel\nKof\n")
println(path.readText())
println(path.size())
```

## Erros de IO

- `readText`/`readBytes` de arquivo inexistente retornam `null` — nos 5 alvos
  (paridade travada na 0.5.0).
- `size()` de arquivo inexistente **lança** `file not found: <path>` — a
  mensagem é idêntica no JVM e no Native (0.5.0, §494). Verifique com
  `exists()` se a ausência for esperada.
- A partir da 0.5.0 o conjunto completo de `kof.io` roda também em
  riscv64/aarch64: predicados, `mkdir/mkdirs`, `delete` recursivo,
  `copyTo`/`moveTo`, `readRange`, `path`/`fileName`/`parent`/`extension` e
  `resolve`/`normalize`/`toAbsolute`. Método/namespace inexistente em tipo
  builtin agora é `SEM102` (0.5.0) — nunca no-op silencioso.

## `kof.time`

```kof
var agora = now()        // epoch milliseconds (Long)
println(agora > 1700000000000)
```

### Scheduler: `every` / `at`

Agendamento de tarefas recorrentes ou em horário específico:

```kof
// a cada 30 segundos (milissegundos)
var id = scheduler.every(30000) {
    println("tick")
}
scheduler.cancel(id)

// cron de 5 campos (UTC) — real no JVM/JS desde 0.4.4
var job = scheduler.at("0 3 * * *", () -> backup())

// expressão de duração idiomática (0.4.8): s, ms, m, h, d, M, a e composição com &
var lembrete = scheduler.at("30m", () -> println("pausa"))
var manutencao = scheduler.at("1d&30m", () -> println("janela"))
```

> `scheduler.at` com cron/duração é real no JVM e no JS; no **Native** é
> `CRON001` em compile-time (recusa honesta). `scheduler.every` funciona
> nos 3 targets (SCHED001 fechado). Para atraso único, o idiom é
> `spawn { time.sleep(ms); job() }`.

### `readLine()` (0.2.0+)

Leitura de linha do stdin:

```kof
main() {
    println("Digite seu nome:")
    var nome = readLine()
    println("Olá, " + nome)
}
```

## Exercícios

1. Escreva um programa que salva uma lista de nomes em `nomes.txt` e depois lê.
2. Crie um "contador de palavras": leia um arquivo e retorne quantas palavras tem.
3. Serialize um `record User` para JSON e depois decodifique de volta (JVM).
4. Liste o conteúdo de um diretório com `Directory(...).list()`.