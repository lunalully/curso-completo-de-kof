# Contrato HTTP entre pedidos e estoque

## pedidos → estoque

| Chamada | Request | Response |
|---------|---------|----------|
| GET /produtos/:id | — | 200 `{"id":1,"qtd":10}` |
| POST /produtos/:id/baixa | `{"qtd":1}` | 200 `{"ok":true}` |

## WORKAROUND — cliente HTTP (gap)

`kof.http` client é **planned** (0.0.8). Hoje, para o serviço `pedidos`
chamar `estoque`:

1. Use um processo externo (ex.: `curl`) via `process.run` — documentado como WORKAROUND.
2. Ou coloque um gateway/proxy na frente que roteia.
3. Quando `kof.http` existir, troque por `http.get(...)` sem mudar o contrato.

## Regra do curso

O **contrato** (método, path, body, erros) não muda com a implementação.
Desenhe o contrato primeiro; a mecanica de transporte é detalhe.
