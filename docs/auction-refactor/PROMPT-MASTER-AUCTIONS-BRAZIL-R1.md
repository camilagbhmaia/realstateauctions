# PROMPT MASTER — AUCTIONS BRAZIL R1

## Missão
Refatorar o Auctions Brazil de forma incremental, auditável e reversível, preservando tudo que já funciona e substituindo apenas superfícies incompatíveis com a arquitetura alvo.

## Autoridades
- GitHub: fonte de verdade de código e documentação.
- Supabase/Postgres: fonte de verdade operacional futura para catálogo, memberships, entitlements, leads, alertas e reuniões.
- Cloudflare: edge, distribuição, cache, filas e object storage quando isso reduzir custo sem criar segunda autoridade.
- JSON legado do frontend: somente fonte histórica de migração; nunca autoridade futura de disponibilidade.

## Regras invariáveis
1. Nunca publicar oportunidade como disponível com base apenas em edital ou data futura.
2. Reconciliar ativo, evento, processo, publicações e evidências posteriores.
3. Nunca inventar CNJ, matrícula, leiloeiro, preço, data ou status.
4. Preservar identidade canônica do ativo entre republicações, praças, leiloeiros e processos relacionados.
5. Não apagar histórico; preferir versionamento, status e intervalos de validade.
6. Detalhes protegidos devem ser revalidados server-side/RLS/API.
7. Não usar service_role no frontend.
8. Não criar dual-write ou duplicação de authority.
9. Cache é derivado e descartável; nunca source of truth.
10. Toda mutação estrutural pertence a uma única onda e precisa de rollback.

## FREE-TIER-FIRST ARCHITECTURE
O produto deve permanecer nos planos gratuitos pelo maior tempo tecnicamente seguro possível.

### Supabase
- Postgres para dados canônicos e relacionamentos.
- Auth + RLS para identidade/autorização.
- Edge Functions somente quando justificadas.
- Storage somente quando vantajoso.
- Secrets agrupados por provider/runtime.
- Nunca um secret por tribunal ou fonte quando o provider puder ser compartilhado.

### Cloudflare
- Pages para frontend público.
- Workers para gateway e integrações leves.
- R2 para PDFs, editais, imagens, snapshots e evidências pesadas.
- KV para cache derivado.
- Queues para jobs assíncronos e batching.
- Turnstile para proteção antifraude/bot.
- D1 apenas para workloads isolados/cache/checkpoints, nunca como source of truth.
- Nenhum produto Cloudflare pode criar segunda authority.

### Estratégia de processamento
- Preferir batching.
- Evitar uma invocation por imóvel, movimento ou documento quando um lote puder ser processado com segurança.
- Persistir idempotency keys/checksums quando houver ingestão ou webhook.

## FREE_TIER_BUDGET_GATE

### PASS
Consumo projetado até 60% da franquia gratuita aplicável.

### WARN
Acima de 60% e até 80%; exige plano explícito de otimização e capacidade.

### BLOCKED
Acima de 80%; não permitir nova expansão que aumente consumo sem redesign, redução ou justificativa/aprovação explícita.

### Métricas — Supabase
- database size/delta
- storage size/delta
- egress estimado
- MAU
- Edge Function count
- Edge invocations
- secrets usados
- Realtime impact

### Métricas — Cloudflare
- R2 storage
- R2 Class A/B operations
- Workers requests
- Worker CPU
- KV reads/writes/storage
- Queue operations
- Pages builds
- Turnstile
- D1 reads/writes/storage
- image transformations
- demais quotas aplicáveis

Os limites dos planos gratuitos devem ser revalidados na documentação oficial antes de qualquer onda que dependa deles.

## Sequência
W0 -> W1 -> W2 -> W3.

Após W3 podem avançar, com ownership isolado:
- INGEST
- SEARCH
- MEMBERSHIP
- WEB/SEO
- GROWTH
- ADMIN

Convergência final:
C1 -> C2 -> C3 -> C4 -> C5.

## AUCTION-W0
Somente reality check, inventário, baseline, mapa de riscos, congelamento de interfaces e governança Free-Tier-First.

Não criar nesta onda:
- migrations
- endpoints
- schema canônico
- RLS
- auth
- collectors
- billing
- alterações funcionais no frontend legado
