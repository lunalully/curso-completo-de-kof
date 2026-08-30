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

**Gaps documentados (compile-time, nunca silenciosos):**
- `JSN002` — JSON de objetos/records no target Native.
- `JSN001` — `json.encode(1.5)` (Float/Double) ainda não compila; converta
  para String ou use Int.

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
| `File("x").size()` | Long; `-1` se não existir |
| `File("x").delete()` | Bool |

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

- Leituras que falham retornam `null` (JVM); no Native, `readText` de arquivo
  inexistente encerra com erro — verifique `exists()` antes.
- `size()` retorna `-1` para arquivo inexistente.

## `kof.time`

```kof
var agora = now()        // epoch milliseconds (Long)
println(agora > 1700000000000)
```

### Scheduler: `every` / `at` (0.2.0+)

Agendamento de tarefas recorrentes ou em horário específico (JVM/JS):

```kof
// a cada 30 segundos
every(30s) {
    println("tick")
}

// às 03:00 todo dia (cron)
at("0 3 * * *") {
    backup()
}
```

> Native reporta `SCHED001` (gap documentado).

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