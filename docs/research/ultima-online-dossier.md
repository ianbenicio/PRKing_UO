# Dossiê de pesquisa — Ultima Online para uma experiência Unity offline

Data da varredura inicial: 27 de outubro de 2026.

## 1. Objetivo e critério de pesquisa

Este documento identifica o DNA sistêmico de *Ultima Online* (UO), as diferenças entre eras, os dados úteis para implementação e as adaptações necessárias para transformar um mundo originalmente massivo em um RPG local. As fontes foram ordenadas assim:

1. documentação oficial de UO e Unity;
2. código e documentação de implementações open source;
3. enciclopédias especializadas e fóruns históricos;
4. relatos comunitários, usados como evidência de expectativa ou problema, nunca como especificação isolada.

Não se pretende copiar código, texto, arte, áudio, mapas ou marcas da EA. Fórmulas e comportamentos devem ser reespecificados a partir de documentação pública e testes próprios. Consulte também o [source ledger](source-ledger.md).

## 2. O que torna UO reconhecível

Os elementos que mais se repetem nas fontes oficiais, nos postmortems e nas discussões da comunidade são:

- progressão sem classe e sem nível global, baseada no uso de skills;
- três stats fundamentais — Strength, Dexterity e Intelligence — derivados em vida, stamina e mana;
- objetos físicos persistentes, containers aninhados, peso, durabilidade, qualidade e propriedades;
- mundo isométrico contínuo com elevação, interiores e grande densidade de objetos interativos;
- interação por verbo e alvo: usar, abrir, arrastar, soltar, equipar, falar, atacar, domar, colher e fabricar;
- crafting e coleta que alimentam uma economia de faucets e sinks;
- combate com posicionamento, linha de visão, tempo de ataque, magia, reagentes, cura, veneno e morte recuperável;
- casas, decoração, armazenamento e lojas;
- NPCs e criaturas que fazem o mundo parecer habitado;
- liberdade de construir um papel: guerreiro, mago, ladrão, bardo, domador, artesão, mercador ou híbrido.

A página oficial de [Skills, Stats and Attributes](https://uo.com/wiki/ultima-online-wiki/player/stats/skills-stats-and-attributes/) confirma o modelo de stats e atributos. O [postmortem da GDC](https://gdcvault.com/play/1025402/Classic-Game-Postmortem-Ultima-Online) destaca ecologia simulada, economia, PvP e histórias emergentes. A análise de Raph Koster sobre a [ecologia original](https://www.raphkoster.com/games/snippets/did-players-destroy-the-uo-ecology/) esclarece que seu abandono envolveu custo econômico e desempenho, não apenas jogadores exterminando a fauna.

### Implicação para o projeto offline

Remover a rede não pode remover a sensação de sociedade. A população que antes era humana precisa ser parcialmente substituída por agentes locais, rotinas, demanda econômica, boatos e conflitos simulados. O projeto [UO Offline](https://github.com/Klein187/uo-offline) fornece evidência prática: bots que viajam, compram, fabricam, agrupam-se e frequentam bancos transformam um shard vazio em um mundo crível.

## 3. Eras e regra-base recomendada

UO acumulou sistemas incompatíveis entre si. O perfil de regras deve ser uma configuração explícita, nunca uma mistura acidental.

| Era | Ano | Mudanças de referência | Consequência de projeto |
|---|---:|---|---|
| Original | 1997 | mundo único, sistemas iniciais, PvP aberto | fonte do sandbox, mas com muitas regras historicamente instáveis |
| The Second Age (T2A) | 1998 | Lost Lands, dungeons, stat cap 225 | melhor baseline clássico e já usado por experiências offline |
| Renaissance (UOR) | 2000 | divisão Felucca/Trammel, power hour | reduz griefing e consolida o conjunto clássico |
| Third Dawn | 2001 | Ilshenar e cliente 3D | expansão de mundo e criaturas |
| Lord Blackthorn's Revenge | 2002 | pet bonding e quests | domesticação menos punitiva |
| Age of Shadows | 2003 | propriedades em itens, Malas, Chivalry e Necromancy | muda profundamente loot, builds e balanceamento |
| Samurai Empire | 2004 | Tokuno, Bushido e Ninjitsu | amplia combate e conteúdo |
| Mondain's Legacy | 2005 | elfos, Spellweaving, dungeons | conteúdo avançado e artifacts |
| Stygian Abyss | 2009 | gargoyles, Ter Mur, Mysticism, Imbuing e Throwing | terceira raça e novos loops |
| High Seas | 2010 | navios, pirataria e pesca revisada | grande subsistema marítimo |
| Time of Legends | 2015 | Eodon, masteries, account gold | endgame moderno |
| Endless Journey | 2018 | camada free-to-play e restrições | irrelevante para um produto local |

A matriz acima é corroborada pela documentação de [eras do ModernUO](https://modernuo.com/docs/development/era-and-expansions/) e pelo [resumo oficial para jogadores retornando](https://uo.com/wiki/ultima-online-wiki/a-summary-for-returning-players/).

### Recomendação

- `Classic`: T2A como núcleo, com correções de usabilidade inspiradas em UOR.
- `Modern`: reservado para uma fase futura, começando em AoS.
- `Custom`: regras originais do projeto, sem obrigação de compatibilidade.

O primeiro vertical slice implementará `Classic`. As regras ficarão em `RuleProfile`, permitindo comparar fórmulas e conteúdo sem ramificações espalhadas pelo código.

## 4. Inventário de sistemas

| Domínio | Comportamentos essenciais | Adaptação offline |
|---|---|---|
| Personagem | aparência, stats, skills, reputação, morte | um avatar principal; companions opcionais |
| Skills | ganho por uso, locks, caps, dificuldade | curva ajustável para uma campanha solo |
| Movimento | oito direções, elevação, portas, teleportes, montaria | grid isométrico autoritativo e pathfinding hierárquico |
| Interação | duplo clique, drag/drop, contexto, alvo | ações semânticas independentes da UI |
| Inventário | containers, peso, stacks, equipamento, banco | containers persistentes com IDs estáveis |
| Combate | melee, ranged, armor, resist, special moves | pausa opcional e velocidade configurável são QoL possíveis |
| Magia | spellbook, círculos, mana, reagentes, targeting | 64 magias clássicas podem ser conteúdo incremental |
| Morte | corpo, ghost, ressurreição, recuperação de itens | autosave seguro, modos de penalidade configuráveis |
| Coleta | mining, lumberjacking, fishing, skinning | recursos com regeneração e reservas regionais |
| Crafting | ferramentas, estações, receitas, qualidade, reparo | cadeia econômica completa, não um menu isolado |
| Criaturas | spawns, loot, agressão, fauna | população simulada em LOD |
| Pets | tame, controle, loyalty, treino, stable | companions persistentes; bonding reduz frustração solo |
| NPCs | vendors, trainers, healers, guards, bankers | agenda, necessidades, memória curta e estoque real |
| Economia | gold, preços, estoque, vendors, sinks | agentes compram e consomem; controlador anti-inflação |
| Reputação | karma, fame, notoriety, murder | crimes e resposta social local substituem PvP massivo |
| Guildas/facções | pertencimento, guerra, reputação | facções de NPCs e companheiros; PvP humano fica fora |
| Housing | deed, placement, storage, lock-down, decoração | casa como meta de longo prazo e sink econômico |
| Mundo | facets, cidades, dungeons, regiões, clima e tempo | uma região densa primeiro; streaming por chunks |
| Viagem | moongates, runes, recall, gate, barcos | fast travel desbloqueável e rotas visíveis de NPCs |
| Quests | clássicas, narrativas, escorts, treasure maps | objetivos sistêmicos e histórias curtas, sem daily grind |
| Virtudes | oito virtudes, shrines e benefícios | reputação moral com consequências locais |
| UI | paperdoll, gumps, hotbars, macros, journal | UI escalável, acessível e totalmente remapeável |
| Persistência | mundo contínuo e casas | snapshots locais versionados e journal de eventos |
| Ferramentas | map, spawn, item, recipe e AI editors | ferramentas de Editor são requisito de produto |

Referências especializadas por sistema incluem a [categoria Combat oficial](https://uo.com/wiki/ultima-online-wiki/combat/), [spellcasting](https://uo.com/wiki/ultima-online-wiki/skills/an-introduction-to-spellcasting/), [pet ownership](https://uo.com/wiki/ultima-online-wiki/skills/animal-taming/pets-ownership/), [housing](https://uo.com/wiki/ultima-online-wiki/gameplay/houses-placing-a-house/house-ownership-4-managing-your-home/), [quests](https://uo.com/wiki/ultima-online-wiki/gameplay/quests/) e [virtues](https://uo.com/wiki/ultima-online-wiki/gameplay/the-virtues/).

## 5. Catálogo moderno de skills

A [enciclopédia UOGuide](https://www.uoguide.com/Skills) registra 58 skills ativas e uma removida. O projeto deve importar skills como definições de dados, não criar uma classe C# por skill.

### Combate

Archery, Chivalry, Fencing, Focus, Mace Fighting, Parrying, Swordsmanship, Tactics, Wrestling, Bushido e Throwing.

### Cura

Healing e Veterinary.

### Magia e suporte

Alchemy, Evaluating Intelligence, Inscription, Magery, Meditation, Necromancy, Resisting Spells, Spellweaving, Spirit Speak e Mysticism.

### Bardos

Discordance, Musicianship, Peacemaking e Provocation. Enticement é a skill removida que foi substituída por Discordance.

### Ladinos

Begging, Detecting Hidden, Hiding, Lockpicking, Poisoning, Remove Trap, Snooping, Stealing, Stealth e Ninjitsu.

### Criaturas e percepção

Anatomy, Animal Lore, Animal Taming, Camping, Forensic Evaluation, Herding, Taste Identification e Tracking.

### Crafting

Arms Lore, Blacksmithy, Bowcraft & Fletching, Carpentry, Cooking, Item Identification, Cartography, Tailoring, Tinkering e Imbuing.

### Coleta

Fishing, Mining e Lumberjacking.

O perfil T2A habilitará apenas o subconjunto historicamente apropriado. Skills posteriores continuam no catálogo com `introducedInEra` e permanecem desativadas.

## 6. Números de referência que afetam o modelo

| Dado | Valor documentado | Uso no projeto |
|---|---:|---|
| Skills ativas no UO moderno | 58 | catálogo máximo, não escopo inicial |
| Skill cap moderno | 720.0 | somente profile moderno |
| Skill cap clássico T2A | 700.0 | default proposto para `Classic` |
| Cap individual clássico | 100.0 | Grandmaster; profile moderno pode chegar a 120 com scrolls |
| Stat cap inicial | 225 | default clássico e base moderna de novos personagens |
| Stats | STR, DEX, INT | derivam HP, stamina e mana |
| Magery | 64 spells em 8 circles | conteúdo incremental e data-driven |
| Limite documentado de backpack/vendor | 125 itens | não precisa ser copiado, mas informa budgets e UX de containers |

Os caps modernos e o modelo raise/lower/lock vêm da página oficial de [skills e stats](https://uo.com/wiki/ultima-online-wiki/player/stats/skills-stats-and-attributes/). A página oficial de [Magery](https://uo.com/wiki/ultima-online-wiki/skills/magery/) registra 64 spells em oito círculos. Limites de containers variaram por contexto e era; o [vendor player-owned](https://uo.com/wiki/ultima-online-wiki/gameplay/npc-commercial-transactions/npcs-player-owned/) documenta 125 itens no backpack do vendor. Esses valores são referências históricas, não requisitos automáticos: cada profile deve declará-los.

## 7. Mundo e formatos de dados

O cliente clássico divide o mundo em terreno, statics, tile metadata, arte, animações, gumps, hues, som e localização. O [UOFiddler](https://uofiddler.polserver.com/) documenta e permite visualizar praticamente todos esses grupos, e o [Ultima SDK Python](https://github.com/UltimaWorks/ultima-sdk-python) lê `.mul`, `.idx` e `.uop`.

Dimensões de referência:

| Facet | Dimensão em tiles |
|---|---:|
| Britannia/Felucca/Trammel atual | 7168 × 4096 |
| Britannia legacy | 6144 × 4096 |
| Ilshenar | 2304 × 1600 |
| Malas | 2560 × 2048 |
| Tokuno | 1448 × 1448 |
| Ter Mur | 1280 × 4096 |

Essas dimensões são confirmadas pelo [UO Asset Studio](https://www.uoassets.com/studio/guide/UOAssetStudio-User-Guide-0.33.13) e podem ser comparadas aos [facet maps oficiais](https://uo.com/wiki/ultima-online-wiki/world/facet-maps/).

### Conclusão técnica

Um facet clássico possui dezenas de milhões de tiles. Não se pode criar um `GameObject` por tile ou entidade. O runtime Unity precisa de:

- armazenamento binário ou arrays nativos por chunk;
- geração/batching de meshes e sprites por chunk;
- streaming em anéis ao redor da câmera;
- entidades visuais apenas para o working set próximo;
- simulação abstrata para regiões distantes.

## 8. Evidências dos fóruns e projetos contemporâneos

O levantamento comunitário encontrou padrões especialmente úteis:

- há demanda real por UO local, sem depender de shard ou conexão;
- a ausência de jogadores torna bancos, estradas, lojas e dungeons vazios;
- bots precisam de suprimentos reais, compras, crafting e morte para não parecerem decoração;
- navegação em mapas antigos é um dos maiores custos: alturas erradas, portas, teleportes e pontos inalcançáveis exigem tooling e telemetria;
- jogadores pedem UI escalável; zoom do mundo não resolve paperdoll, lojas e texto pequenos;
- persistência precisa ser explícita e confiável, inclusive ao fechar o jogo;
- muitos usuários querem selecionar intensidade de PK ou desativá-lo;
- LLM local não é requisito para NPCs convincentes; utility AI, templates de fala e memória de eventos são mais previsíveis e baratos.

O [README do UO Offline](https://github.com/Klein187/uo-offline) detalha behaviors, grafo de waypoints, destinos, arrival points, A*/Dijkstra, consumo real de recursos e recuperação de agentes presos. O tópico comunitário de [UO Offline no Reddit](https://www.reddit.com/r/ultimaonline/comments/1vxm5oz/uo_offline/) concentra pedidos sobre UI, save, guildas, housing, party e controle de PK. Um protótipo de [remake Unity](https://www.reddit.com/r/ultimaonline/comments/1vqkdhb/anyone_interested_in_superearlyprealpha_testing_a/) confirma a viabilidade de streaming em blocos, mas permanece cliente/servidor e depende de dados do cliente oficial.

## 9. Projetos técnicos de referência

| Projeto | Valor para pesquisa | Licença/limite |
|---|---|---|
| [ClassicUO](https://github.com/ClassicUO/ClassicUO) | rendering, formatos, interação e cliente clássico | BSD-2-Clause; não distribui assets da EA |
| [ModernUO](https://github.com/modernuo/ModernUO) | eras, entidades, conteúdo, regras e persistência | GPL-3.0; não copiar para um projeto incompatível |
| [RunUO](https://www.runuo.net/) | origem do ecossistema C# de emulação | verificar licença por componente |
| [UOFiddler](https://github.com/polserver/UOFiddler) | inspeção e conversão de dados 2D | Beerware; assets continuam protegidos |
| [UO Offline](https://github.com/Klein187/uo-offline) | desenho de população simulada local | derivado de ModernUO/ClassicUO; confirmar licença antes de reutilizar |
| [sosaria-forge](https://github.com/xor-droid/sosaria-forge) | formatos de mapa e geração procedural | consultar licença do repositório |

O documento de [content patterns do ModernUO](https://github.com/modernuo/ModernUO/blob/main/dev-docs/content-patterns.md) é útil como taxonomia de itens, criaturas, spells, skills, loot e lifecycle, mas seu código não deve entrar no projeto sem uma decisão consciente de licenciamento.

## 10. Limite jurídico e de propriedade intelectual

A [EA User Agreement](https://www.ea.com/legal/user-agreement) afirma que o conteúdo e os direitos associados permanecem com a EA/licenciadores e restringe cópia, modificação, distribuição e extração salvo autorização ou lei aplicável. A [política de conteúdo da EA](https://help.ea.com/en/articles/security-and-rules/ea-content-policy/) não oferece uma autorização geral para mods de UO. Portanto:

- não versionar nem distribuir `.mul`, `.idx`, `.uop`, mapas, sprites, animações, gumps, fontes, música, sons, textos ou logos de UO;
- não usar `Ultima`, `Ultima Online`, Britannia ou personagens como marca do produto distribuído;
- não conectar um cliente customizado aos servidores oficiais;
- manter código clean-room e conteúdo autoral como caminho padrão;
- se houver um modo de compatibilidade pessoal, o usuário deve apontar para a própria instalação legítima e o importador deve produzir cache local não redistribuível;
- realizar revisão jurídica antes de qualquer distribuição pública ou monetização.

O próprio [ClassicUO](https://github.com/ClassicUO/ClassicUO) segue o padrão de distribuir código, não assets, e exige uma instalação legítima dos dados.

## 11. Conclusão

O escopo correto não é “copiar um MMO sem servidor”. É construir um RPG sandbox local que preserve o ciclo sistêmico de UO:

`explorar → coletar → fabricar/saquear → negociar/equipar → assumir riscos → afetar o mundo → persistir consequências`.

O perfil clássico oferece uma fundação limitada e coerente. A arquitetura deve ser data-driven, autoritativa localmente, testável fora do Unity e capaz de simular uma população em diferentes níveis de detalhe. O primeiro marco não deve tentar reproduzir Britannia inteira; deve provar esse ciclo em uma pequena região densa.

