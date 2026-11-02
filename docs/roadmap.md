# Roadmap de desenvolvimento

O roadmap é orientado por riscos. Cada fase termina em um produto executável; conteúdo em escala só começa depois de provar ferramentas, persistência e performance.

As regras de cada fase vêm da [especificação](spec/especificacao.md). A **primeira versão jogável** (loop básico, especificação §1) fecha no fim da Fase 3. Magia, coleta e crafting formam a **segunda versão** (Fase 4). Nenhum build é distribuído por ora (especificação §17).

## Fase 0 — pesquisa e fundação documental

Status: concluída nesta primeira varredura; especificação de sala limpa recebida e documentos alinhados em 2026-11-02.

- dossiê, PRD e arquitetura;
- política de fontes e propriedade intelectual;
- recorte inicial e riscos;
- primeira story pronta para refinamento.

Gate: decisão explícita sobre direção de arte antes de produção de conteúdo em volume. O uso da instalação já está decidido: leitor opcional em tempo de execução, sem importador nem cache, só para uso pessoal (especificação §3.0 e §17).

## Fase 1 — vertical slice técnico local

- bootstrap Unity 6.6 (6000.6.7f1);
- migração para a Unity 6.7 LTS quando sair (prevista para o quarto trimestre de 2026; a 6.6 não é LTS);
- assemblies e domínio C# puro;
- provedor de recursos com a origem "próprio" e IDs próprios (especificação §3.0);
- cena sandbox de 64 × 64 tiles em chunks, com as estações da Fase 1 e o painel de depuração ([sandbox de testes](architecture/sandbox-de-testes.md));
- movimento, câmera, interação e um container;
- combate corpo a corpo, uma criatura com IA simples, loot e um vendedor;
- save/load atômico;
- testes e build Windows local (não distribuído).

Gate: ciclo mover → combater → saquear → vender → salvar → carregar funciona sem console.

## Fase 2 — personagem e objetos

- criação de personagem (D6), stats, skills, caps e travas (D5);
- ganho de skill e de stat por uso (especificação §4.2 e §5.4–§5.5);
- regeneração de vida e estamina (especificação §4.3);
- containers aninhados, peso, equipment, durability e stacks;
- tooltips, paperdoll, skills e hotbar;
- content catalog e validators;
- sandbox: estações de criação, treino, regeneração e vestiário.

Gate: 500 itens instanciados e dez ciclos de save/load sem perda ou duplicação.

## Fase 3 — mundo, combate e morte (fecha a primeira versão jogável)

- streaming assíncrono;
- elevação, portas, interiores e roofs;
- leitor opcional da instalação, só em tempo de execução e só para uso pessoal: recorte do mapa real, uma cidade e arredores (D8);
- melee, armor, parry (D4) e durabilidade; arco PODE;
- morte, fantasma e ressurreição (curandeiro e santuário); bandagens (especificação §8.1);
- loot (D10), corpos e itens no chão (D11);
- cidade com vendedor, banqueiro, curandeiro e guarda; notoriedade básica contra NPCs;
- dia e noite (D9);
- sandbox: estações de elevação, capela, enfermaria, banco, guarda, céu, escudo, desgaste, baú mágico e animais.

Gate: loop básico completo numa cidade e arredores com uma área de criaturas (especificação §1 e §10.1), dentro do budget de frame e memória.

## Fase 4 — magia, coleta, crafting e economia (segunda versão)

- magia: spellbook, círculos, reagentes, mana e targeting (especificação §7; decidir D3 antes);
- mining, lumberjacking, fishing e harvesting;
- ferramentas, estações, recipes, qualidade, repair e recycle;
- vendors, estoque, preços, gold, faucets e sinks;
- simulador econômico headless;
- sandbox: estações de magia (círculo de conjuração), jazidas, oficina e, quando entrar, veneno.

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

Nenhum build é distribuído por ora. Qualquer release exige nova avaliação do responsável, e um build compartilhado precisa usar só conteúdo próprio (especificação §17).

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
| E06 | Combat, Magic & Death | 3 (combate e morte); 4 (magia) |
| E07 | Gathering & Crafting | 4 |
| E08 | Economy & Vendors | 1–3 (vendedor e banco); 4 |
| E09 | Agents, Navigation & Schedules | 5 |
| E10 | Reputation, Crime & Factions | 3 (guardas e notoriedade básica); 5 |
| E11 | Quests, Dungeons & Encounters | 6 |
| E12 | Pets & Companions | 6 |
| E13 | Housing & Decoration | 6 |
| E14 | Tooling, Modding & Content Packs | transversal |
| E15 | Accessibility, Performance & Release | transversal |

## Definition of Done global

- acceptance criteria da story satisfeitos;
- regras com fonte na especificação (a story cita a seção);
- testes novos e existentes passam;
- lint/analyzers e build passam;
- nenhum warning novo sem justificativa;
- dados e ferramentas documentados;
- story atualizada com checkboxes e File List;
- profiling proporcional ao risco;
- nenhuma dependência ou asset sem licença registrada;
- nenhum dado da EA no repositório, no build, no save ou em capturas versionadas.
