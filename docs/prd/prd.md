# PRD — Outrora, RPG sandbox isométrico local

Status: Draft inicial  
Data: 27 de outubro de 2026  
Revisão: 2 de novembro de 2026 — alinhamento com a [especificação](../spec/especificacao.md) (Anexo A dela)

## 1. Visão

Criar um RPG sandbox isométrico para um jogador, executado inteiramente de forma local, que preserve a liberdade sistêmica do MMO clássico de referência na era T2A: progressão por uso, mundo persistente, crafting relevante, objetos físicos, economia e uma sociedade simulada por agentes.

O produto não será um servidor privado empacotado nem um cliente que exige localhost. Será um jogo Unity de processo único, chamado **Outrora**, com conteúdo próprio por padrão, leitura opcional da instalação do próprio jogador (só em tempo de execução e só para uso pessoal) e uma arquitetura capaz de receber novos perfis de regras e conteúdo. As regras vêm só da especificação de sala limpa.

## 2. Problema

O jogo de referência foi desenhado para que outros jogadores preenchessem funções sociais e econômicas. Simplesmente remover a rede produz cidades vazias, vendors irrelevantes, crafting sem demanda e pouca narrativa emergente. A experiência local precisa substituir parte dessa dinâmica com agentes, facções, rotinas e uma economia persistente, sem fingir que NPCs são jogadores humanos.

## 3. Pilares

1. Liberdade sem classes: o personagem é definido pelas skills usadas.
2. Tudo é um sistema: combate, coleta, crafting, comércio e reputação se alimentam.
3. Mundo vivo offline: agentes viajam, trabalham, consomem, lutam e contam eventos.
4. Consequências persistentes: recursos, estoques, crimes, casas e relações sobrevivem ao reload.
5. Conteúdo autoral e seguro: o jogo roda só com conteúdo próprio; nada da EA entra no git, no build ou no save.
6. Ferramentas antes de volume: designers criam regiões, spawns, receitas e agendas sem editar código.

## 4. Público inicial

- fãs do MMO clássico de referência que desejam uma experiência local e preservável;
- jogadores de RPG sandbox que valorizam crafting, builds livres e exploração;
- modders e designers que querem criar mundos e regras sem manter servidor.

## 5. Experiência-alvo

O jogador cria um personagem, explora uma região, coleta recursos, aprende skills pelo uso, fabrica ou saqueia equipamentos, negocia com uma economia local, forma relações com NPCs, enfrenta perigos, adquire uma casa e altera de modo persistente o estado do mundo. A primeira versão jogável cobre só o loop básico (§8).

## 6. Requisitos funcionais

### Fundação

- `FR-001`: iniciar, pausar, salvar, carregar e sair sem serviço externo.
- `FR-002`: utilizar seed e RNG controlado para testes reprodutíveis.
- `FR-003`: suportar profiles de regras com conteúdo habilitado por era; números em tabelas de dados, com os perfis "T2A estrito" e "pré-AOS final" (especificação, "Modelo numérico").
- `FR-004`: manter IDs estáveis para entidades persistentes.
- `FR-005`: ler todo conteúdo por um provedor de recursos com duas origens — próprio (padrão) e instalação do jogador (opcional, só em tempo de execução, desligado por padrão) — e usar só IDs próprios em regras, dados e save (especificação §3.0 e §15).

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
- `NFR-008`: nenhum asset, dado ou texto da EA entra no repositório, no build, no save ou em capturas versionadas.
- `NFR-009`: nenhum build é distribuído por ora; um build compartilhado no futuro exige nova avaliação do responsável e usa só conteúdo próprio (especificação §17).
- `NFR-010`: nenhuma monetização, nem doação ligada ao jogo; nenhum porte para outras plataformas sem nova avaliação (especificação §17).

## 8. Escopo do primeiro produto jogável

Loop básico (especificação §1), jogado num recorte do mapa: uma cidade e arredores com uma área de criaturas (D8: recorte do mapa real lido da instalação, para uso pessoal; o conteúdo próprio é a base e cobre o que faltar com placeholders):

- mundo isométrico navegável, com elevação e colisão (especificação §10.5);
- criação de personagem (D6), stats e skills com ganho por uso, caps e travas (D5); skills mínimas da especificação §5.1;
- combate corpo a corpo contra criaturas com IA simples (corpo a corpo e animal), armadura e parry (D4); arco PODE;
- loot (D10), corpos e itens no chão (D11);
- morte, fantasma e ressurreição, com curandeiro na cidade e santuário no campo;
- uma cidade com vendedor, banqueiro, curandeiro e guarda; notoriedade básica contra NPCs;
- dia e noite (D9);
- save/load, UI escalável e ferramentas mínimas de conteúdo.

Esse recorte prova o loop antes de multiplicar mapas, skills e itens.

## 9. Fora do escopo inicial

- magia (segunda versão; a D3 é decidida antes);
- coleta e crafting (segunda versão);
- housing e barcos, pets e domesticação, guildas, quests, viagem e agentes simulados do mundo vivo;
- multiplayer, servidor dedicado, login, chat online ou serviços de conta;
- compatibilidade de protocolo com servidores do jogo de referência;
- PvP entre humanos;
- outras facets, outras eras e skills além das 49 do T2A;
- LLM obrigatório para NPCs;
- qualquer monetização (live service, loja, microtransações, doação ligada ao jogo) ou telemetry externa;
- distribuição de builds e porte para outras plataformas;
- redistribuição de assets da EA.

## 10. Métricas de sucesso

- um novo jogador completa o ciclo explorar → combater → saquear → morrer e ressuscitar → vender e guardar no banco → salvar sem console;
- dez ciclos de save/load preservam inventário, skills, corpos, estoque e mundo;
- a UI permanece utilizável em 1280×720, 1920×1080 e 4K com escalas configuráveis;
- nenhuma referência quebrada ou dado inválido passa pelos validadores;
- a partir da Fase 4: economia simulada de 30 dias não entra em inflação/escassez irreversível;
- a partir da Fase 5: agentes mantêm atividade observável em cidade, estradas e dungeon sem intervenção.

## 11. Riscos principais

| Risco | Mitigação |
|---|---|
| escopo equivalente a décadas de MMO | vertical slice e expansão por profiles/packs |
| mundo vazio sem jogadores | agentes locais e economia a partir das Fases 4 e 5; NPCs de cidade desde a primeira versão |
| performance em mapas densos | chunks, pooling, LOD de simulação e profiling precoce |
| navegação frágil | arrival points, validadores, telemetria de stuck e rotas hierárquicas |
| save quebrado após mudanças | versionamento, migração, snapshot atômico e backups |
| contaminação de IP/licença | sala limpa (só a especificação), conteúdo próprio, nome Outrora, nada da EA no git ou no build, ledger de fontes e revisão jurídica |
| fórmulas conflitantes por era | `RuleProfile`, ponto de referência de março de 2000 (D1) e testes de comportamento por perfil |
| tooling tardio | ferramentas são critério de aceite, não backlog opcional |

## 12. Decisões ainda pendentes

- direção de arte autoral;
- licença do próprio Outrora — **decidida por ora (2026-11-02):** todos os direitos reservados enquanto o projeto for local; a licença será decidida antes de publicar (Anexo A, A26);
- tique autoritativo da simulação — **resolvida (2026-11-02):** passo de 25 ms para timers de ação e IA, sistemas pesados em fatias de 10 Hz; o intervalo de golpe segue a fórmula contínua (Anexo A, A18);
- nome da pasta do repositório e dos arquivos que citam o jogo de referência — **resolvida em parte (2026-11-02):** arquivos renomeados para `docs/prd/prd.md` e `docs/research/dossie-pesquisa-inicial.md`; a pasta e o repositório são renomeados antes de o repositório ficar público. Menção descritiva ao jogo é permitida, nunca como nome ou marca (Anexo A, A7);
- destino da Story 1.2 — **resolvida (2026-11-02):** a story do gadget de temperatura saiu deste repositório (Anexo A, A2);
- regras exatas do profile `Classic` — **resolvida (2026-11-02):** valem as seções 4 a 13 da especificação, no perfil T2A, com as decisões abaixo;
- possibilidade ou não de importador pessoal de dados legítimos — **resolvida:** não há importador; há leitor opcional em tempo de execução, só para uso pessoal (decisões de 2026-10-28 e 2026-11-01, especificação §3.0 e §17);
- intensidade padrão de morte, crime e PK simulado — **resolvida para a primeira versão:** o padrão são as regras da especificação (§8.3 e §9); intensidade configurável fica para depois;
- plataformas além de Windows no primeiro release — **resolvida:** só Windows; nenhum porte sem nova avaliação (especificação §17).

### Decisões D1–D11 (aceitas pelo responsável em 2026-11-01)

Resumo; o texto que vale é o da especificação §16.

| # | Tema | Decisão |
|---|---|---|
| D1 | Ponto de referência dentro do T2A | estado de março de 2000; números do emulador onde não houver número oficial |
| D2 | Vida máxima | 50 + força/2 |
| D3 | Resistência a magia | adiada: decidir antes da magia, na segunda versão |
| D4 | Parry | modelo da especificação §6.6 |
| D5 | Travas de skill e bônus da primeira hora do dia | travas sim; bônus da primeira hora PODE, como dado |
| D6 | Criação de personagem | tabela configurável, padrão 80 de stats e 100 de skills |
| D7 | Fama e karma | faixas de ±15000 |
| D8 | Mapa da primeira versão | recorte do mapa real lido da instalação, uma cidade e arredores; só para uso pessoal; build compartilhado no futuro usa mundo próprio |
| D9 | Curva de luz | crepúsculos de 2 horas, com a alternativa em dados |
| D10 | Bônus mágico de dano | +1 a +9 |
| D11 | Corpos e itens no chão | corpos 7 + 7 min; itens 1 h; configuráveis |
