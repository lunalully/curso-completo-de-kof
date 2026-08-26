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

O padrão é **HTTP**: o serviço A chama o serviço B via REST. Desde a
0.1.0-beta o Kof tem **cliente HTTP nativo**:

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
        return "" + baixa     // WORKAROUND: return de variável nua quebra a rota (nota #5a)
    }

    app.listen(config.int("server.port", 8081))
}
```

- `http.get(url[, header])`, `http.post(url, body)`, `put/delete/patch`.
- `http.status(url)` consulta o código HTTP sem baixar o corpo.
- Headers customizados: argumento extra `"Header: valor"`.
- Native/JS reportam `HTTP002` em compile-time — a comunicação entre
  serviços é padrão JVM por ora.

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