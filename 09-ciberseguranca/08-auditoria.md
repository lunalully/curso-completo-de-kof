# Módulo 09 · Aula 8 — Auditoria

> Auditar é revisar sistematicamente o código contra um checklist. Esta aula
> ensina o processo e fornece o checklist que você aplica em qualquer projeto
> Kof.

## O processo de auditoria

1. **Escopo** — o que auditar (módulos, rotas, dados sensíveis).
2. **Coleta** — leia o código com os olhos do atacante.
3. **Varrer** — aplique o checklist (abaixo).
4. **Provar** — para cada achado, tente explorar (aula 07).
5. **Reportar** — risco (baixo/médio/alto), evidência, correção.
6. **Remediar** — corrija e re-teste.

## Checklist de auditoria de código Kof

### Autenticação e Autorização
- [ ] Senhas com `passwords.hash` (nunca sha256/texto plano)?
- [ ] Verificação com `passwords.verify` (constant-time)?
- [ ] JWT com `exp`, `iss`, `aud` verificados?
- [ ] `alg` confiado ao token? (deve ser fixo HS256 — a plataforma garante)
- [ ] Roles/permissions por rota (`hasRole`/`hasPermission`)?
- [ ] Middleware de auth cobre TODAS as rotas sensíveis?
- [ ] Mensagens de login genéricas?

### Criptografia
- [ ] AES-GCM (não ECB/DES/MD5)?
- [ ] IV/nonce único por mensagem?
- [ ] Chaves fora do código (`secrets.get` — ou `Secret`/`KeyHandle`, que
      redigem por construção e comparam constant-time)?
- [ ] Comparações secretas com `constantTimeEquals`?
- [ ] Aleatório seguro (`crypto.randomHex/randomInt`)?
- [ ] ChaCha20 só onde há implementação (JVM/JS; Native recusa `SECN002`)?

### Segredos no repositório
- [ ] Nenhum literal de chave/token no código (procure `sk-`, `AKIA`,
      `-----BEGIN`, `password=`)?
- [ ] `reveal()` de um `Secret` só aparece onde é inevitável — e está
      justificado na revisão?
- [ ] `log`/`json.encode` de segredos redigem (`Secret`)?
- [ ] O repo tem política de reporte de vulnerabilidade e varredura de
      segredos no CI (o próprio Kof4j tem `.github/` para isso — use de
      referência)?

### Injeção e validação
- [ ] SQL 100% com `?` bind?
- [ ] Entrada validada (tipo, tamanho, intervalo)?
- [ ] Saída é texto (sem HTML cru)?
- [ ] Paths normalizados (path traversal)?
- [ ] Payloads malformados não quebram (try/catch)?

### Hardening
- [ ] Headers de segurança documentados/aplicados?
- [ ] CSRF em estados mutáveis?
- [ ] CORS com whitelist (nunca `*` com credenciais)?
- [ ] Rate limit em login/endpoints caros?
- [ ] Erros genéricos + log detalhado?
- [ ] Segredos redigidos em logs (`secrets.redact`)?

### Dados
- [ ] Dados sensíveis cifrados em repouso?
- [ ] Exposição mínima (não vaze listas inteiras sem necessidade)?
- [ ] Backups e exclusão segura (conforme necessidade)?

## Modelo de reporte

```text
ACHADO #1 — SQL Injection em GET /users?q=
Risco: ALTO
Evidência: db.query(db, "select * from users where name = '" + q + "'")
Impacto: leitura/escrita arbitrária no banco
Correção: usar bind — db.query<User>(db, "... name = ?", q)
Status: CORRIGIDO / PENDENTE
```

## Auditando a própria plataforma

O Kof audita a si mesmo: `docs/ecosystem-coverage.md`, `docs/security.md`
(matriz Spring Security), `kof.security` com casos adversariais nos testes.
Use esses docs como referência do que a plataforma garante.

## Exercícios

1. Aplique o checklist completo na API do módulo 06 e gere um reporte.
2. Classifique cada achado por risco e prioridade.
3. Corrija os itens ALTO e re-audite (aula 07 para provar).
4. (Desafio) Escreva um "checklist de auditoria" para um frontend `kof.ui`:
   o que muda? (XSS via texto, permissões de janela, dados no client)