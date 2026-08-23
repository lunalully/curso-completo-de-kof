# Módulo 05 · Aula 1 — Modelo TCP/IP

> A pilha de protocolos que conecta tudo. Entender as camadas ajuda a
> entender o que Kof faz (e o que não faz) por você.

## As camadas (visão simplificada)

```text
Aplicação    HTTP, DNS, SMTP  ← kof.web / kof serve mora aqui
Transporte   TCP, UDP          ← conexões confiáveis
Internet     IP, ICMP          ← endereçamento global
Acesso       Ethernet, Wi-Fi   ← hardware
```

## O que cada camada resolve

| Camada | Problema | Exemplo |
|--------|----------|---------|
| Aplicação | formato dos dados | HTTP |
| Transporte | entrega confiável, ordem, fluxo | TCP |
| Internet | encontrar a máquina | IP |
| Acesso | enviar bits pelo meio físico | Ethernet |

## Endereçamento

- **IP** identifica a máquina (ex.: `192.168.1.10`).
- **Porta** identifica o processo dentro da máquina (ex.: `8080`).
- **Socket** = IP + porta. Uma conexão TCP tem dois sockets (origem/destino).

## HTTP sobre TCP

Quando você chama uma rota Kof:

```
Seu browser → TCP/IP → servidor Kof (porta 8080)
```

O servidor Kof escuta na porta, aceita a conexão, lê a request HTTP, chama
sua rota, e responde.

## O que o Kof abstrai

Você **nunca** vê:

- `socket()`, `bind()`, `listen()`, `accept()`
- parsing de cabeçalhos HTTP
- threads para cada conexão

```kof
main() {
    var app = web.app()
    app.get("/") {
        return "kof serve online"
    }
    app.listen(8080)
}
```

O runtime gera um accept loop com **virtual threads** (JVM) — detalhe interno.

## O que ainda não existe

- Cliente HTTP (`kof.http` client — planned).
- TLS/HTTPS (planned, roadmap G12).
- Sockets crus expostos (planned).

Se precisar hoje, é `WORKAROUND` — não idiom.

## Exercícios

1. Explique com suas palavras a diferença entre IP e porta.
2. Por que o Kof abstrai sockets? Qual a vantagem de intenção?
3. Rode `kof serve` num programa com uma rota e use `curl` de outra janela.
4. (Pesquisa) O que muda para o programador quando TLS/HTTPS chegar?