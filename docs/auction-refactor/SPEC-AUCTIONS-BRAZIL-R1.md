# SPEC — AUCTIONS BRAZIL R1

## AUCTION-W0 — Objetivo
Registrar o estado real do legado antes de qualquer refatoração estrutural, congelar interfaces existentes e estabelecer gates de custo, segurança e ownership.

## Baseline confirmado
- Repositório: camilagbhmaia/realstateauctions
- Branch base operacional: github-pages
- Branch W0: chore/auction-w0-baseline-20260909
- Site predominantemente estático HTML/CSS/JS
- Catálogo legado em quatro arquivos JSON
- Total nominal: 152 registros
- JSON carregado diretamente no browser
- Nenhum backend canônico do catálogo identificado
- Nenhum projeto Supabase Auctions Brazil identificado na conexão atual

## Arquivos legados do catálogo
- data/property_1_to_50.json
- data/property_51_to_100.json
- data/property_101_to_150.json
- data/property_151_to_152.json

## Contratos legados congelados
- property-grid.html
- property-single.html?id=<legacy-id>
- /data/property_*.json
- assets/js/loadProperties*.js
- assets/js/populate*.js
- assets/js/searchPropertyAdjustments.js
- assets/js/paginateProperties.js
- forms/contact.php

Congelamento significa preservar comportamento até substituição compatível e testada.

## Source of truth
Supabase/Postgres será a única authority operacional.

Cloudflare será infraestrutura complementar de edge/cache/storage/fila, nunca segunda authority.

## FREE_TIER_BUDGET_GATE

PASS:
<= 60%

WARN:
> 60% e <= 80%

BLOCKED:
> 80% para novas expansões sem otimização ou justificativa explícita.

## Segurança
- service_role proibido no frontend
- RLS/capabilities para dados protegidos
- projeção pública separada dos dados subscriber-only
- autenticação não equivale a autorização
- acesso sensível revalidado server-side

## Gate de W0
W0 passa quando:
1. baseline estiver documentado
2. contratos estiverem congelados
3. mapa de riscos estiver registrado
4. Free-Tier-First estiver formalizado
5. migrations = 0
6. endpoints = 0
7. documentação estiver em PR
8. PR estiver verde

## AUCTION-W1
Só inicia após:
- merge verde da W0
- definição explícita do projeto Supabase authority do Auctions Brazil
