# Trilha 09 · Exercícios (consolidado)

> As aulas já têm exercícios embutidos. Aqui está o consolidado com
> [soluções](solucoes/). Rode tudo em localhost.

## Fundamento (aula 01)

- ★ [01-threat-model.md](solucoes/01-threat-model.md) — preencha um threat model para a API do módulo 06 (5 rotas, 5 ameaças, pilar CIA violado).

## OWASP (aula 02)

- ★★ [02-owasp-kf.kf](solucoes/02-owasp-kf.kf) — um programa que demonstra o **fix** de A02 (crypto), A03 (SQL bind) e A09 (logging de login) — conceitualmente.

## Crypto (aula 03)

- ★★ [03-crypto.kf](solucoes/03-crypto.kf) — sha256, HMAC + constantTimeEquals, AES-GCM round trip + tamper.
- ★★★ [04-assinatura.kf](solucoes/04-assinatura.kf) — assinatura de mensagem (HMAC) + verificação.

## Identidade (aula 04)

- ★★ [05-jwt.kf](solucoes/05-jwt.kf) — create/verify; segredo errado e token adulterado rejeitados.

## Hardening (aula 05)

- ★★★ [06-rate-limit.kf](solucoes/06-rate-limit.kf) — `RateLimiter` (WORKAROUND manual) aplicado ao login.
- ★★★ [07-errors-genericos.kf](solucoes/07-errors-genericos.kf) — handler que loga o detalhe e devolve erro genérico.

## Secure coding (aula 06)

- ★★★ [08-validacao.kf](solucoes/08-validacao.kf) — validação de entrada completa num POST (nome vazio/longo, id negativo).
- ★★ escreva 3 payloads de SQLi e explique por que `?` bind os neutraliza.

## Pentest e auditoria (aulas 07-08)

- ★★★ siga o roteiro da aula 07 contra a API do "guardião".
- ★★★ preencha o checklist da aula 08 e gere um reporte.

## Labs (aula 09)

- ★★★ faça os Labs 1-6 (guardião de senhas, JWT roleta, API blindada, logs, audit trail, guardião de arquivos).

## Soluções

As [soluções](solucoes/) cobrem os itens com código. Os itens de escrita
(threat model, checklist, reporte) são entregas suas — não há gabarito.