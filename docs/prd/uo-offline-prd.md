# PRD — RPG sandbox isométrico local inspirado em Ultima Online

Status: Draft inicial  
Data: 27 de outubro de 2026

## 1. Visão

Criar um RPG sandbox isométrico para um jogador, executado inteiramente de forma local, que preserve a liberdade sistêmica de *Ultima Online*: progressão por uso, mundo persistente, crafting relevante, objetos físicos, economia e uma sociedade simulada por agentes.

O produto não será um servidor privado empacotado nem um cliente que exige localhost. Será um jogo Unity de processo único, com conteúdo original por padrão e uma arquitetura capaz de receber novos perfis de regras e conteúdo.

## 2. Problema

UO foi desenhado para que outros jogadores preenchessem funções sociais e econômicas. Simplesmente remover a rede produz cidades vazias, vendors irrelevantes, crafting sem demanda e pouca narrativa emergente. A experiência local precisa substituir parte dessa dinâmica com agentes, facções, rotinas e uma economia persistente, sem fingir que NPCs são jogadores humanos.

## 3. Pilares

1. Liberdade sem classes: o personagem é definido pelas skills usadas.
2. Tudo é um sistema: combate, coleta, crafting, comércio e reputação se alimentam.
3. Mundo vivo offline: agentes viajam, trabalham, consomem, lutam e contam eventos.
4. Consequências persistentes: recursos, estoques, crimes, casas e relações sobrevivem ao reload.
5. Conteúdo autoral e seguro: a distribuição não depende de propriedade intelectual da EA.
6. Ferramentas antes de volume: designers criam regiões, spawns, receitas e agendas sem editar código.

## 4. Público inicial

- fãs de UO clássico que desejam uma experiência local e preservável;
- jogadores de RPG sandbox que valorizam crafting, builds livres e exploração;
- modders e designers que querem criar mundos e regras sem manter servidor.

## 5. Experiência-alvo

O jogador cria um personagem, explora uma região, coleta recursos, aprende skills pelo uso, fabrica ou saqueia equipamentos, negocia com uma economia local, forma relações com NPCs, enfrenta perigos, adquire uma casa e altera de modo persistente o estado do mundo.

## 6. Requisitos funcionais

### Fundação

- `FR-001`: iniciar, pausar, salvar, carregar e sair sem serviço externo.
- `FR-002`: utilizar seed e RNG controlado para testes reprodutíveis.
- `FR-003`: suportar profiles de regras com conteúdo habilitado por era.
- `FR-004`: manter IDs estáveis para entidades persistentes.

### Mundo

- `FR-100`: renderizar terreno isométrico, elevação, statics, portas e interiores.
- `FR-101`: carregar e descarregar chunks sem travar o frame principal.
- `FR-102`: suportar regiões, cidades, dungeons, portais e pontos de interesse.
- `FR-103`: pathfinding próximo em grid e rotas distantes em grafo hierárquico.

### Personagem e interação

- `FR-200`: criar personagem com aparência, stats e skills iniciais.
- `FR-201`: mover em oito direções e interagir por ação + alvo.
- `FR-202`: abrir, mover, empilhar, equipar e usar itens em containers aninhados.
- `FR-203`: aplicar peso, capacidade, durabilidade e condições de uso.
- `FR-204`: ganhar skills por uso com caps, locks e dificuldade.

### Combate e sobrevivência

- `FR-300`: combate melee e ranged com cadence, accuracy, dano, armor e resistência.
- `FR-301`: spells data-driven com mana, reagentes, cast time e alvo.
- `FR-302`: healing, poison, buffs, debuffs e efeitos temporizados.
- `FR-303`: morte, corpse, ghost/ressurreição ou perfil de penalidade equivalente.
- `FR-304`: reputação local para crime, agressão, roubo e murder.

### Produção e economia

- `FR-400`: mining, lumberjacking, fishing e harvesting de criaturas.
- `FR-401`: crafting por receitas, ferramentas, estações, skill e materiais.
- `FR-402`: qualidade, reparo, reciclagem e durabilidade de itens.
- `FR-403`: vendors com estoque, preços, compra e venda persistentes.
- `FR-404`: faucets e sinks mensuráveis para impedir inflação sem controle.

### Mundo vivo

- `FR-500`: NPCs com agenda, profissão, inventário, recursos e necessidades.
- `FR-501`: agentes aventureiros com behaviors, skills, equipamentos e risco de morte.
- `FR-502`: companions/pets com follow, stay, attack, loyalty e persistência.
- `FR-503`: memória de eventos e diálogo templateado contextual.
- `FR-504`: facções, relações e conflitos entre agentes.
- `FR-505`: simulação em níveis de detalhe conforme distância e relevância.

### Conteúdo e progressão

- `FR-600`: quests e objetivos compostos por condições e efeitos data-driven.
- `FR-601`: treasure maps, dungeons e encontros com loot tables.
- `FR-602`: housing, colocação, storage e decoração.
- `FR-603`: viagem por gates, runes/recall ou equivalente autoral.
- `FR-604`: virtudes/reputações morais como módulo de conteúdo, não dependência do core.

### Interface e acessibilidade

- `FR-700`: UI escalável independentemente do zoom do mundo.
- `FR-701`: drag/drop, tooltips, target cursor, journal e hotbar.
- `FR-702`: remapeamento de teclado, mouse e gamepad.
- `FR-703`: opções de contraste, tamanho de fonte, velocidade, pausa e intensidade de penalidade.
- `FR-704`: logs de save, erros e recuperação compreensíveis ao usuário.

### Ferramentas

- `FR-800`: editor de regiões, chunks, spawns e portais.
- `FR-801`: validadores de item, recipe, loot, skill e referências quebradas.
- `FR-802`: visualizador de navmesh/grid, waypoints e agentes presos.
- `FR-803`: simulador headless de economia e população.

## 7. Requisitos não funcionais

- `NFR-001`: alvo de 60 FPS no hardware recomendado e 30 FPS no mínimo.
- `NFR-002`: nenhum frame normal acima de 33 ms por streaming ou save.
- `NFR-003`: autosave atômico; corrupção não pode destruir o último save íntegro.
- `NFR-004`: domínio testável em EditMode sem carregar Scene.
- `NFR-005`: conteúdo inválido deve falhar no Editor/CI, não silenciosamente no runtime.
- `NFR-006`: o build padrão realiza zero chamadas de rede.
- `NFR-007`: nenhuma dependência obrigatória de conta, launcher ou CDN.
- `NFR-008`: nenhum asset ou texto protegido de UO entra no repositório.

## 8. Escopo do primeiro produto jogável

Uma região autoral pequena contendo:

- uma cidade, uma área selvagem e uma dungeon;
- um personagem e cinco skills representativas;
- melee, uma arma ranged e quatro spells;
- vinte definições de itens e seis receitas;
- mining e lumberjacking;
- dois vendors, um healer, um guard e oito agentes simulados;
- dez tipos de criaturas, pets básicos e loot;
- uma casa simples;
- uma quest curta e um evento emergente;
- save/load, UI escalável e ferramentas mínimas de conteúdo.

Esse recorte prova o ciclo completo antes de multiplicar mapas, skills e itens.

## 9. Fora do escopo inicial

- multiplayer, servidor dedicado, login, chat online ou serviços de conta;
- compatibilidade de protocolo com servidores UO;
- PvP entre humanos;
- todas as facets, todas as eras ou 58 skills na primeira versão;
- LLM obrigatório para NPCs;
- live service, loja, microtransações ou telemetry externa;
- redistribuição de assets de UO.

## 10. Métricas de sucesso

- um novo jogador completa o ciclo coleta → crafting → combate → comércio → save sem console;
- dez ciclos de save/load preservam inventário, agentes, economia e mundo;
- agentes mantêm atividade observável em cidade, estradas e dungeon sem intervenção;
- economia simulada de 30 dias não entra em inflação/escassez irreversível;
- nenhuma referência quebrada ou dado inválido passa pelos validadores;
- a UI permanece utilizável em 1280×720, 1920×1080 e 4K com escalas configuráveis.

## 11. Riscos principais

| Risco | Mitigação |
|---|---|
| escopo equivalente a décadas de MMO | vertical slice e expansão por profiles/packs |
| mundo vazio sem jogadores | agentes locais e economia desde o primeiro slice |
| performance em mapas densos | chunks, pooling, LOD de simulação e profiling precoce |
| navegação frágil | arrival points, validadores, telemetria de stuck e rotas hierárquicas |
| save quebrado após mudanças | versionamento, migração, snapshot atômico e backups |
| contaminação de IP/licença | conteúdo original, ledger de fontes e revisão jurídica |
| fórmulas conflitantes por era | `RuleProfile` e testes de comportamento por perfil |
| tooling tardio | ferramentas são critério de aceite, não backlog opcional |

## 12. Decisões ainda pendentes

- direção de arte autoral;
- regras exatas do profile `Classic`;
- possibilidade ou não de importador pessoal de dados legítimos;
- intensidade padrão de morte, crime e PK simulado;
- plataformas além de Windows no primeiro release.

