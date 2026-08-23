# Projetos Práticos

> Projetos de ponta a ponta para consolidar o curso. Cada projeto tem:
> objetivo, requisitos (módulos usados), passo a passo, critérios de aceite e
> extensões.

## Lista de projetos (ordem sugerida)

| # | Projeto | Módulos usados | Complexidade |
|---|---------|----------------|--------------|
| [01-calculadora.md](01-calculadora.md) | Calculadora CLI com pilha | 00, 02 | ★☆☆ |
| [02-gerenciador-tarefas.md](02-gerenciador-tarefas.md) | Gerenciador de tarefas (CLI + arquivo) | 00, 01, 03(io) | ★★☆ |
| [03-api-rest-auth.md](03-api-rest-auth.md) | API REST autenticada | 03, 04, 06 | ★★★ |
| [04-blog-completo.md](04-blog-completo.md) | Blog completo (posts + comentários) | 03, 04, 06 | ★★★★ |
| [05-monitor-seguranca.md](05-monitor-seguranca.md) | Monitor de segurança (auditoria) | 04, 09 | ★★★★ |

## Critérios gerais de aceite

1. Todo código compila (`kof check` sem erros).
2. Todo programa roda (`kof run` sem exceptions).
3. Código **idiomático** (revisar com o módulo 08).
4. Segurança de entrada (bind SQL, validação) — módulo 09.
5. Documentação curta do que foi aprendido.

## Como avaliar seu progresso

- Concluiu 1-2 → nível básico.
- Concluiu 3 → nível intermediário.
- Concluiu 4-5 → nível avançado ("padrão ouro").

## Regras de ouro dos projetos

- **Intenção primeiro:** use a abstração da plataforma, não reimplemente.
- **Anti-patterns proibidos:** sentinelas como erro, camadas de cerimônia,
  estado duplicado, Java-like code.
- **WORKAROUND marcado:** se uma feature não existe, marque `WORKAROUND`.