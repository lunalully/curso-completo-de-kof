# Contrato HTTP entre pedidos e estoque

## pedidos → estoque

| Chamada | Request | Response |
|---------|---------|----------|
| GET /produtos/:id | — | 200 `{"id":1,"qtd":10}` |
| POST /produtos/:id/baixa | `{"qtd":1}` | 200 `{"ok":true}` |

## Cliente HTTP nativo

O serviço `pedidos` chama `estoque` com `kof.http` (JVM + JS):

```kof
// 1. consulta o produto
var st = http.status("http://127.0.0.1:8082/produtos/" + id)
if (st != 200) {
    return "{\"error\": \"produto nao encontrado\"}"
}

// 2. dá a baixa
var resposta = http.post("http://127.0.0.1:8082/produtos/" + id + "/baixa", "{\"qtd\":1}")
```

- Trate **status** antes de parsear o corpo (`http.status(url)`).
- HTTP client funciona nos 3 targets (HTTP002 fechado, 0.2.8-beta).
- Termine o handler com expressão (`"" + resposta`), nunca `return resposta;`
  nua — a rota não registra (nota #5a).

## Regra do curso

O **contrato** (método, path, body, erros) não muda com a implementação.
Desenhe o contrato primeiro; a mecanica de transporte é detalhe.
