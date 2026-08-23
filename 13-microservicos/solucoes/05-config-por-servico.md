# Config e operação dos serviços

## Por serviço (arquivo kof.config ou env)

```text
# pedidos/kof.config
server.port=8081
app.name=pedidos

# estoque/kof.config
server.port=8082
app.name=estoque
```

ou por env:

```bash
KOF_SERVER_PORT=8081 kof run pedidos.kf
KOF_SERVER_PORT=8082 kof run estoque.kf
```

## Rodar os dois juntos

```bash
# terminal 1
cd pedidos && kof run servico-pedidos.kf
# terminal 2
cd estoque && kof run servico-estoque.kf

curl localhost:8081/health
curl localhost:8082/health
```

## Logs com identidade

`kof.log` com o nome do serviço ajuda a correlacionar:

```kof
log.info("pedidos: " + method() + " " + path())
```

## Observabilidade (planejado)

- `kof.metrics`/`kof.observability`: ⏳ planned (health já está).
- TLS entre serviços: ⏳ planned (G12).
