# Projeto 4 — Blog Completo

> **Objetivo:** um blog com posts e comentários: API REST + frontend
> `kof.ui` (KofJS) + banco + auth. O projeto integrador do curso.

## Requisitos

- Módulos: 03, 04, 06, 07 (frontend), 08, 09.
- Backend: API REST (posts, comentários, auth).
- Frontend: janela `kof.ui` que consome a API.

## Modelo de dados

```kof
record Post(Int id, String titulo, String conteudo, String autor)
record Comentario(Int id, Int postId, String texto, String autor)
```

## Backend (API REST)

| Método | Rota | Ação |
|--------|------|------|
| GET | `/posts` | lista posts (público) |
| GET | `/posts/:id` | post + comentários |
| POST | `/posts` | cria post (autenticado) |
| POST | `/posts/:id/comentarios` | comenta (autenticado) |
| DELETE | `/posts/:id` | remove (autor ou admin) |

**Regra de autorização:** só o autor (ou admin) remove — valide o dono via
`auth.claims()` (ler `sub` do JSON; `auth.user()` ainda quebra em handler,
nota #5) comparado a `post.autor` (evita IDOR, módulo 09).

## Frontend (kof.ui)

```kof
// janela do blog: lista de posts, abrir post, adicionar comentário
main() {
    var w = Window("Blog Kof")
    // consome a API com... (ver nota abaixo)
    w.show()
}
```

> **Nota real:** o cliente HTTP nativo existe no **JVM e JS**
> (`http.get/post`). O frontend `kof.ui` renderiza via target JS, onde
> `kof.http` funciona nos 3 targets (HTTP002 fechado, 0.2.8-beta). O padrão honesto do projeto:
> o backend JVM serve a API; a UI consome estado servido ou usa `WORKAROUND`
> documentado (fetch exposto pela plataforma). O importante é modelar a
> integração e respeitar a separação backend/frontend.

## Passo a passo

1. Modelo + tabelas (posts, comentarios) com `kof.db`.
2. Rotas de leitura públicas.
3. Auth (JWT) + autorização por autor.
4. Rotas de escrita com validação (titulo vazio, texto curto).
5. Frontend `kof.ui` com listagem e detalhe.
6. Logging + rate limit nos comentários.

## Critérios de aceite

- [ ] Listar posts e abrir detalhe com comentários
- [ ] Criar post/comentário autenticado
- [ ] Só autor/admin remove
- [ ] Validação de entrada em todas as escritas
- [ ] Sem SQLi (bind), sem XSS (texto, não HTML cru)
- [ ] Logs de segurança presentes

## Extensões

- Busca de posts por título.
- "Posts do autor" (query filter).
- Contador de visualizações.
- Paginação (limit/offset).

## Entregável

O repositório do blog com: backend, frontend, `kof.config`, testes
(`kof test`) e um reporte curto de auditoria de segurança.