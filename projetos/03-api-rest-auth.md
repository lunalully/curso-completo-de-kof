# Projeto 3 — API REST Autenticada

> **Objetivo:** API REST completa de usuários com autenticação JWT,
> autorização por role, banco (H2), config e logging. O "guardião de API"
> seguro — tudo do módulo 09 aplicado.

## Requisitos

- Módulos: 03 (`kof.db`), 04 (`kof.security`), 06 (`kof.web`), 08
  (`kof.config`, `kof.log`), 09 (secure coding).
- Driver JDBC no classpath (H2).

## Endpoints

| Método | Rota | Acesso | Ação |
|--------|------|--------|------|
| POST | `/login` | público | emite JWT |
| GET | `/users` | autenticado | lista usuários |
| GET | `/users/:id` | autenticado | busca usuário |
| POST | `/users` | admin | cria usuário |
| PUT | `/users/:id` | admin | atualiza |
| DELETE | `/users/:id` | admin | remove |

## Estrutura do projeto

```text
api-usuarios/
├── app.kf           # o programa (módulo 06 aula 4 como base)
├── kof.config       # database.url, server.port, jwt.secret
└── testes/
    └── api-test.kf  # asserts sobre as funções puras
```

## Passo a passo

1. **Banco** — `record User(Int id, String name, String email)` + tabela.
2. **Login** — `passwords.verify` + `jwt.create` (claims com role).
3. **Middleware** — `auth.authenticated()` global (exceto `/login`).
4. **Autorização** — `hasRole("admin")` em POST/PUT/DELETE.
5. **CRUD** — SQL com `?` bind; validação de entrada; erro genérico + log.
6. **Config** — `kof.config` para url/porta; `secrets.get` para JWT secret.
7. **Rate limit** — `RateLimiter` da trilha (aula 05 do módulo 09).
8. **Logging** — `kof.log` para login ok/falhou, erros, acessos.

## Critérios de aceite

- [ ] Login correto → token; senha errada → erro genérico
- [ ] GET /users sem token → `unauthorized`
- [ ] POST /users com role user → `forbidden`
- [ ] POST /users com role admin → cria
- [ ] SQLi via `q=` não injeta (bind)
- [ ] Brute force no login bloqueado após N tentativas
- [ ] Logs reconstroem a timeline de um ataque (módulo 09, lab 4)

## Extensão de segurança (nota)

Status codes customizados ainda não existem na Fase 1 do `web.app()`.
Use o corpo para comunicar o erro e **documente** como `WORKAROUND`:

```kof
return "{\"error\": \"unauthorized\"}"
```

## Entregável

Um `README` da API com: endpoints, exemplos `curl`, modelo de ameaças
(módulo 09 aula 1) e o reporte de auditoria (módulo 09 aula 8).