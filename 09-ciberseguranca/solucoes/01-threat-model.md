# Threat Model — API de usuários (módulo 06)

| Rota | Atacante | Ameaça | CIA violado | Mitigação |
|------|----------|--------|-------------|-----------|
| POST /login | anônimo | brute force de senha | Confidencialidade | rate limit + passwords.verify (constant-time) |
| GET /users | usuário logado | IDOR: listar todos | Confidencialidade | autorização por role/permissão |
| GET /users/:id | usuário logado | IDOR: ler outro | Confidencialidade | verificar dono (`sub` das claims == dono) |
| POST /users | usuário logado | criar sem permissão | Integridade | hasRole admin |
| POST /login | anônimo | injeção SQL via email | Confidencialidade/Integridade | bind `?`, nunca concatenar |
| qualquer | anônimo | vazamento de erro/stack | Confidencialidade | erro genérico + log detalhado |
