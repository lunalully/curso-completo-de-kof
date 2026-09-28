# Trilha 13 · Aulas — conceitos e comunicação

## Aula 01 — O que são microserviços

Microserviços = **vários processos** (um por domínio), cada um com seu próprio
ciclo de vida, banco e deploy, comunicando por **HTTP**.

```text
cliente → gateway → pedidos ──HTTP──→ estoque
                      │
                      └──HTTP──→ pagamentos
```

**Quando NÃO usar:**
- Time pequeno → um monolito bem modular basta.
- Domínio pequeno → os custos (rede, operação, consistência) superam os ganhos.
- Dados fortemente acoplados → um banco compartilhado é monolito disfarçado.

## Aula 02 — Um serviço Kof

Um serviço é **um programa Kof** com `web.app()`:

```kof
main() {
    var app = web.app()

    app.get("/health") {
        return "{\"status\": \"ok\"}"
    }

    app.get("/pedidos") {
        return json.encode(listaDePedidos)
    }

    app.listen(config.int("server.port", 8081))
}
```

Cada serviço tem:
- sua própria porta,
- seu próprio `kof.config` (nome, porta, banco),
- seus próprios logs (`kof.log`),
- sua própria rota de `/health` (para orquestração/observabilidade).

## Aula 03 — Comunicação entre serviços

O padrão é **HTTP**: o serviço A chama o serviço B via REST. O Kof tem
**cliente HTTP nativo** (JVM + JS):

```kof
main() {
    var app = web.app()

    app.post("/pedidos") {
        // chama o serviço de estoque
        var st = http.status("http://127.0.0.1:8082/produtos/1")
        if (st != 200) {
            return "{\"error\": \"produto indisponivel\"}"
        }
        var baixa = http.post("http://127.0.0.1:8082/produtos/1/baixa", "{\"qtd\":1}")
        return baixa          // 0.5.0: return de variável funciona (nota #5a fechada)
    }

    app.listen(config.int("server.port", 8081))
}
```

- `http.get(url[, header])`, `http.post(url, body)`, `put/delete/patch`.
- `http.status(url)` consulta o código HTTP sem baixar o corpo.
- Headers customizados: argumento extra `"Header: valor"`.
- Resiliência real desde a 0.4.3: `http.timeout(ms)`, `http.retry(n)`,
  `http.circuit(n)` — o circuito abre após N falhas e fecha com
  `http.circuit(0)`.
- HTTP client funciona nos 5 alvos — a comunicação entre serviços é padrão
  JVM por ora.

## Aula 04 — API gateway

Um serviço que **roteia** para os demais (entry point único):

```kof
main() {
    var app = web.app()
    app.get("/health") {
        return "{\"status\": \"gateway ok\"}"
    }
    // roteamento/composição: o gateway chama os serviços com http.get/post
    app.listen(8080)
}
```

O gateway concentra: autenticação (módulo 04), rate limit (módulo 09),
logging e roteamento.

## Aula 05 — Config e operação

- `kof.config` por serviço: `KOF_SERVER_PORT`, `KOF_APP_NAME`, ou arquivo
  `kof.config` por serviço.
- Rode os serviços em portas diferentes no mesmo host, ou em contêineres
  (trilha 15).
- Logs com o nome do serviço: `log.info("pedidos: " + ...)`.

## Aula 06 — Pipelines e supervisão (0.4.0–0.5.0)

O anti-fragmento do `Bash + YAML + jq`: um pipeline é **código Kof tipado** —
o runner é tooling. `kof.workflow` dá `job`, `dag`, `after`, `run` e um
`Report` tipado:

```kof
import kof.workflow

main() {
    val build = job("build", () -> process.run("echo", "build").exitCode == 0)
    val test = job("test", () -> true).after(build)
    val d = dag(listOf(build, test))
    d.retry(build, 3, exponential(100, 2))   // backoff tipado (tentativa -> ms)
    d.retryFixed(test, 2)
    d.deadLetter(build, (nome: String, motivo: String) -> { log.error(nome + ": " + motivo); return true })
    val rep = d.run()
    println(rep.summary())                   // succeeded/failed/skipped/retries/dead
    if (!rep.allOk()) { throw "pipeline red: " + rep.summary() }   // exit != 0
}
```

- Job falho cascateia `skipped` nos dependentes; ciclo é rejeitado no `run()`.
- `checkpoint(d, dbUrl, nome)` grava o progresso em `kof.db`/`kof.orm` — job
  concluído não re-executa (no Native é stub honesto `ORM001` em runtime).
- `schedule(dag, "0 3 * * *")` delega ao `scheduler.at` (Native: `CRON001`).
- Runner dedicado: `kof workflow list|run a.kf [--job <nome>] [--dry-run]`.
- **Supervisão OTP** (`kof.supervisor`): `runSupervised(dag, "nome", max)`
  roda o DAG como workers one_for_one (cada job é um filho `transient`;
  estouro do limite = drop + skip transitivo). Para árvores de workers,
  `supervisor("net").child("conn", fabrica, "permanent").restartLimit(5)`
  com `start()`/`stop(ms)`/`stats()` e `escalate(cb)` no limite — a fábrica
  produz um worker **novo** a cada reinício. Políticas: `permanent`,
  `transient`, `temporary`.

## Contrato (escreva antes de codar)

Para cada par de serviços, defina:
- método + path,
- request body (JSON),
- response body + status,
- casos de erro.

Exemplo `pedidos → estoque`:

```text
GET /estoque/produtos/:id          → 200 {"id":1,"qtd":10}
POST /estoque/produtos/:id/baixa   → 200 {"ok":true} | 400 {"error":"sem estoque"}
```