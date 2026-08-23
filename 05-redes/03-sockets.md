# Módulo 05 · Aula 3 — Sockets (o que a plataforma abstrai)

> Sockets são o mecanismo. Kof prefere intenção — mas entender o mecanismo
> ajuda a entender HTTP, concorrência e segurança.

## A pilha que você não escreve

```kof
// Isto NÃO existe em Kof hoje (planned) — mostra o mecanismo por trás:
// socket() → bind() → listen() → accept() → read/write → close()
```

O `KofHttpServer` do runtime faz isso por você quando você chama
`app.listen(porta)`. O accept loop roda em **virtual threads** (JVM).

## Por que abstrair

1. **Correção** — HTTP tem muitos detalhes (framing, headers, keep-alive).
2. **Concorrência** — cada conexão numa virtual thread, sem o programador
   gerenciar threads.
3. **Segurança** — parsing defensivo no runtime, não no seu handler.
4. **Multi-target** — no futuro, o mesmo código roda com o servidor HTTP de
   outro target.

## A analogia

| Socketeiro | Programador Kof |
|------------|-----------------|
| abre socket | `web.app()` |
| faz bind/listen | `app.listen(8080)` |
| aceita conexão | (automático) |
| lê request cru | `body()`, `header()`, `param()` |
| escreve response | `return "..."` |
| fecha | `app.close()` |

## Rede segura: o que você precisa lembrar

- Não confie em dados de request sem validar (ver módulo 04/09).
- `header("x-auth")` no middleware é o padrão para autenticação.
- O contexto de request é por-request (ThreadLocal em runtime) — handlers
  concorrentes não compartilham estado.

## Exercícios

1. Explique a sequência de chamadas de socket que `app.listen(8080)` dispara.
2. Por que cada conexão em virtual thread importa para servidores?
3. (Pesquisa) O que muda quando o Kof expor cliente HTTP (`kof.http`)?
4. Rode o programa do módulo 05 aula 2 e observe que nenhum socket aparece
   no seu código.