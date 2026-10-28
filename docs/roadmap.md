# Roadmap de desenvolvimento

O roadmap é orientado por riscos. Cada fase termina em um produto executável; conteúdo em escala só começa depois de provar ferramentas, persistência e performance.

## Fase 0 — pesquisa e fundação documental

Status: concluída nesta primeira varredura.

- dossiê, PRD e arquitetura;
- política de fontes e propriedade intelectual;
- recorte inicial e riscos;
- primeira story pronta para refinamento.

Gate: decisão explícita sobre direção de arte e uso ou não de importador pessoal antes de produção de conteúdo em volume.

## Fase 1 — vertical slice técnico local

- bootstrap Unity 6.6 (6000.6.7f1);
- assemblies e domínio C# puro;
- mapa isométrico pequeno em chunks;
- movimento, câmera, interação e um container;
- um recurso, uma receita, um NPC e um inimigo;
- save/load atômico;
- testes e build Windows.

Gate: ciclo coletar → fabricar → combater → salvar → carregar funciona sem console.

## Fase 2 — personagem e objetos

- criação de personagem, stats, skills, caps e locks;
- containers aninhados, peso, equipment, durability e stacks;
- tooltips, paperdoll, skills e hotbar;
- content catalog e validators.

Gate: 500 itens instanciados e dez ciclos de save/load sem perda ou duplicação.

## Fase 3 — mundo, combate e magia

- streaming assíncrono;
- elevação, portas, interiores, roofs e portals;
- melee, ranged, armor, resist, status effects e death;
- targeting e primeiro spellbook;
- loot e corpse recovery.

Gate: dungeon curta completa dentro do budget de frame e memória.

## Fase 4 — coleta, crafting e economia

- mining, lumberjacking, fishing e harvesting;
- ferramentas, estações, recipes, qualidade, repair e recycle;
- vendors, estoque, preços, gold, faucets e sinks;
- simulador econômico headless.

Gate: 30 dias simulados sem inflação ou escassez irreversível.

## Fase 5 — agentes e mundo vivo

- Utility AI, agendas, professions e needs;
- navegação hierárquica, arrival points e stuck recovery;
- aventureiros, crafters, gatherers, guards, healers e merchants;
- gossip/event memory, factions e reputação;
- LOD de simulação.

Gate: uma cidade, estrada e dungeon permanecem ativas por duas horas sem agentes presos ou economia parada.

## Fase 6 — progressão de campanha

- quests data-driven;
- treasure maps, dungeons e encounters;
- housing e decoração;
- pets e companions;
- viagem rápida e reputações/virtudes autorais.

Gate: campanha de 5–10 horas com início, progressão e objetivo de médio prazo.

## Fase 7 — expansão de conteúdo

- packs de regiões e regras;
- mais skills, spells, creatures, recipes e loot;
- modding documentado;
- ferramentas de balanceamento e import/export.

Gate: conteúdo novo criado sem mudanças em Domain para casos suportados.

## Fase 8 — qualidade e release

- acessibilidade e localização;
- performance e compatibilidade de hardware;
- recovery de saves e crash diagnostics;
- auditoria de licenças/IP;
- tutorial, documentação e packaging offline.

Gate: release candidate reproduzível, sem chamadas de rede e com todos os quality gates verdes.

## Epics

| Epic | Nome | Fase principal |
|---|---|---:|
| E01 | Project Foundation | 1 |
| E02 | Content Catalog & Validation | 1–2 |
| E03 | Isometric World & Streaming | 1–3 |
| E04 | Interaction & Inventory | 1–2 |
| E05 | Character, Stats & Skills | 2 |
| E06 | Combat, Magic & Death | 3 |
| E07 | Gathering & Crafting | 4 |
| E08 | Economy & Vendors | 4 |
| E09 | Agents, Navigation & Schedules | 5 |
| E10 | Reputation, Crime & Factions | 5 |
| E11 | Quests, Dungeons & Encounters | 6 |
| E12 | Pets & Companions | 6 |
| E13 | Housing & Decoration | 6 |
| E14 | Tooling, Modding & Content Packs | transversal |
| E15 | Accessibility, Performance & Release | transversal |

## Definition of Done global

- acceptance criteria da story satisfeitos;
- testes novos e existentes passam;
- lint/analyzers e build passam;
- nenhum warning novo sem justificativa;
- dados e ferramentas documentados;
- story atualizada com checkboxes e File List;
- profiling proporcional ao risco;
- nenhuma dependência ou asset sem licença registrada.

