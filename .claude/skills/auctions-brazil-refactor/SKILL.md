# Auctions Brazil Refactor

## Objetivo
Executar mudanças no Auctions Brazil sem reescrever indiscriminadamente o produto.

## Regras
1. Supabase/Postgres é a source of truth operacional.
2. GitHub é source of truth de código e documentação.
3. JSON estático é legado, não authority.
4. Detalhes protegidos exigem backend/RLS/API.
5. Disponibilidade exige evidência reconciliada.
6. Preservar identidade canônica.
7. Não apagar histórico.
8. Não criar dual-write.
9. Cache nunca é authority.
10. Nunca inventar CNJ, matrícula, leiloeiro, preço, data ou status.

## FREE-TIER-FIRST
Preferir a combinação tecnicamente segura de Supabase Free + Cloudflare Free.

Supabase:
- Postgres
- Auth
- RLS
- Edge Functions apenas quando justificadas
- Storage seletivo
- secrets por provider/runtime

Cloudflare:
- Pages
- Workers
- R2
- KV
- Queues
- Turnstile
- D1 apenas para estado derivado/isolado

## FREE_TIER_BUDGET_GATE
PASS <= 60%

WARN > 60% e <= 80%

BLOCKED > 80% para expansão sem otimização ou justificativa explícita.

## Ordem das ondas
W0 -> W1 -> W2 -> W3

Depois:
INGEST / SEARCH / MEMBERSHIP / WEB-SEO / GROWTH / ADMIN

Depois:
C1 -> C5

## Workflow
1. Ler Prompt Master + SPEC + skill.
2. Reality check.
3. Confirmar ownership.
4. Diff mínimo.
5. Implementar somente a onda.
6. Testes positivos/negativos.
7. Testar idempotência/rollback quando aplicável.
8. Gerar WAVE_REPORT.
9. Reconciliar PRs.
10. Merge somente com gates verdes.

## AUCTION-W0
Somente inventário, baseline, riscos, interfaces congeladas e governança Free-Tier-First.

Zero migrations.
Zero endpoints.
