# Módulo 09 · Aula 1 — Fundamentos de Segurança

## O triângulo CIA

| Pilar | O que é | Exemplo em Kof |
|-------|---------|----------------|
| **C**onfidencialidade | só quem pode lê, lê | AES-GCM, segredos |
| **I**ntegridade | dados não são alterados | HMAC, assinatura JWT |
| **A**disponibilidade | o serviço continua no ar | rate limit, resiliência |

Toda decisão de segurança responde a pelo menos um desses pilares.

## Princípios

1. **Least privilege** — dê o mínimo de permissão necessário
   (`hasRole`, `hasPermission`).
2. **Defense in depth** — várias camadas (auth no middleware + validação no
   handler + hardening no servidor).
3. **Fail safe** — quando algo falha, falhe para o lado seguro (rejeite, não
   aceite).
4. **Never trust user input** — valide tudo que chega da rede.
5. **Secure by default** — a plataforma escolhe o seguro por você
   (PBKDF2 600k, HS256 fixo).

## Threat model (modelo de ameaças)

Pergunte, por cada recurso:

- **Quem** é o atacante? (anônimo, usuário logado, admin)
- **O que** ele quer? (dados, privilégio, derrubar serviço)
- **Como** ele chega lá? (injeção, força bruta, token roubado, segredo vazado)
- **Qual o impacto** se conseguir? (vazamento, indisponibilidade, reputação)

```kof
// Para cada rota, defina a política:
//   autenticação necessária?  → auth.authenticated()
//   autorização?              → auth.hasRole("admin")
//   validação de entrada?     → check antes de usar
//   taxa de uso?              → rate limit (workaround manual)
```

## A superfície de ataque de uma app Kof

| Camada | Ameaça |
|--------|--------|
| Rede/HTTP | brute force no login, scraping |
| Roteamento/middleware | bypass de auth, abuso de path |
| Handlers | SQLi, XSS, validação fraca |
| Dados/banco | exfiltração, manipulação |
| Segredos | vazamento em logs/env |
| Cliente/JS | XSS, token em localStorage |

## Princípio de Kof que ajuda você

> Gaps são **compile-time** (`SECN00x`, `DB001`...) — a plataforma nunca
> falha silenciosamente. Use isso a seu favor: compile para saber o que um
> target não faz, e trate explicitamente.

## Exercícios

1. Faça um threat model da API de usuários do módulo 06 (5 rotas, 5 ameaças).
2. Para cada ameaça, diga qual pilar CIA ela viola.
3. Liste as camadas de defesa que você já tem no módulo 06 e as que faltam.
4. (Reflexão) Por que "falhar para o lado seguro" é melhor que "tentar
   funcionar de qualquer jeito"?