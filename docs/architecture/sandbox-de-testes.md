# Sandbox de testes — cena pequena que cresce com o jogo

Status: proposta (decisão do responsável em 2026-11-02)  
Fonte das regras: [especificação](../spec/especificacao.md)  
Primeiras entregas: [Story 1.1a](../stories/1.1a-unity-foundation-and-sandbox.md) e [Story 1.1b](../stories/1.1b-combat-ai-loot-vendor-and-save.md)

## 1. Objetivo

Ter uma cena pequena com tudo o que é preciso para testar ações, skills, combate, IA, itens, economia, animações e, mais tarde, magia e crafting, sem andar pelo mundo atrás de cada caso. Decisões do responsável (2026-11-02):

- **O sandbox cresce com o jogo.** Cada sistema ganha uma estação, que liga quando o sistema é implementado. O escopo da primeira versão (especificação §1) não muda: magia, coleta e crafting entram na segunda versão, com suas estações.
- **O sandbox é a cena da Story 1.1.** A região autoral de 64 × 64 tiles do critério 4 é o sandbox. A Story 1.1 foi dividida depois em 1.1a (cena, estações de movimento e desenho, painel de depuração) e 1.1b (demais estações da Fase 1).

## 2. Princípios

1. **Mesma simulação, sem regra especial.** O sandbox é uma região de conteúdo próprio (`region.sandbox`) que roda o mesmo domínio, o mesmo passo de 25 ms e o mesmo command bus do jogo. Nenhuma estação tem atalho de regra: o que funciona aqui funciona no mundo.
2. **Estações em dados.** Cada estação é uma `TestStationDefinition`: `id`, área, sistema exigido (flag de funcionalidade), seções da especificação, lista de geração (itens, criaturas, NPCs) e estado inicial. A estação de um sistema que ainda não existe aparece como placa marcada "em breve", sem lógica.
3. **Reset por estação.** Cada estação volta ao estado inicial sem recarregar a cena, para repetir um teste em segundos.
4. **Estação é cenário de teste.** Cada estação traz um roteiro: uma sequência de comandos com semente fixa e os eventos esperados. O mesmo roteiro roda em EditMode (só domínio, sem cena) e em PlayMode (com apresentação). O teste manual e o automático cobrem a mesma coisa.
5. **Depuração é comando.** Gerar item, ajustar skill, avançar o relógio etc. são comandos `Debug*` que passam pelo command bus e ficam no journal de comandos. Eles só são aceitos com as ferramentas de desenvolvimento ligadas (build de desenvolvimento). Um save feito depois de um comando de depuração fica marcado (`usedDevTools`).
6. **Só conteúdo próprio.** Placeholders e arte própria; a origem "instalação" fica desligada no sandbox, que deve funcionar numa máquina sem o jogo de referência. Nada da EA no git, no build ou no save (especificação §3.0 e §17).
7. **Reprodutível.** Semente fixa por estação e journal de comandos com passo e sequência: um defeito encontrado no sandbox vira um arquivo de reprodução (estado inicial com o estado do RNG + comandos) que outra pessoa ou um teste reexecuta na mesma `simulationVersion` ([arquitetura §12](unity-offline-architecture.md)).

## 3. Layout (64 × 64 tiles, quatro chunks de 32 × 32)

```text
 (0,0)                     (32,0)                    (63,0)
   ┌──────────────────────────┬──────────────────────────┐
   │ NO — Movimento e render  │ NE — Combate e IA        │
   │ pista de passos,         │ boneco de treino,        │
   │ labirinto de colisão,    │ arsenal, cercados de     │
   │ galeria de animação,     │ criaturas, linha de      │
   │ galeria de desenho       │ visão                    │
   ├────────────────────── HUB (32,32) ──────────────────┤
   │ SO — Cidade              │ SE — Itens e futuro      │
   │ vendedor; depois banco,  │ containers, loot,        │
   │ curandeiro, guarda,      │ estresse; depois magia,  │
   │ santuário                │ coleta e crafting        │
   └──────────────────────────┴──────────────────────────┘
 (0,63)                                              (63,63)
```

- O hub fica no ponto (32, 32), onde os quatro chunks se encontram: todo teste começa ali e já exercita o caso de borda "carregamento na borda entre quatro chunks" da Story 1.1a.
- Cada quadrante é um chunk; a separação também mostra se o streaming e a ativação da IA por setores de 16 × 16 (especificação §2) se comportam ao cruzar fronteiras.
- O hub tem um quadro com as estações, o estado de cada uma (ativa ou "em breve") e o atalho de teleporte para cada uma (comando de depuração).

## 4. Estações por fase

### Fase 1 (Stories 1.1a e 1.1b)

A 1.1a ativa `st.mov.passos`, `st.mov.colisao`, `st.ren.desenho` e `st.ani.galeria`; a 1.1b ativa as demais.

| ID | Estação | O que testa | Especificação |
|---|---|---|---|
| `st.mov.passos` | Pista de passos | Oito direções, ritmo de 400 ms a pé e 200 ms correndo, separação entre pedido, validação e passo aceito; contador de passos e de recusas | §2.2 |
| `st.mov.colisao` | Labirinto de colisão | Bloqueio, path bloqueado, desvio e alvo inalcançável | §10.5 |
| `st.ren.desenho` | Galeria de desenho | Ordem de desenho com objetos altos, pilhas e personagem atrás e na frente de statics | §3.9 |
| `st.ani.galeria` | Galeria de animação | Manequins nas oito direções para cada ação (parado, andar, correr, atacar, receber golpe, morrer), com placeholders; a velocidade da animação muda só na apresentação; arte própria | — |
| `st.cmb.boneco` | Boneco de treino | Alvo que não morre, com skill de defesa ajustável; mostra ritmo, chance de acerto, dano e absorção por parte atingida | §6.1–§6.4 |
| `st.cmb.arsenal` | Arsenal | Armas de velocidades diferentes (rápida, média, lenta, punhos) e armadura por parte do corpo | §6.1, §6.4 |
| `st.ia.cercado` | Cercado corpo a corpo | Percepção, perseguição, fuga abaixo de 20% de vida e volta acima de 50%, abandono do alvo, território | §13.1 |
| `st.cmb.visao` | Linha de visão | Ataque só a até 15 tiles e com linha de visão; alvo removido antes de a ação terminar | §6.1 |
| `st.lot.corpo` | Corpo e loot | Corpo da criatura, pacote de loot, sumiço do corpo aos 7 min (com avanço de relógio) e conteúdo despejado no chão | §13.4, §13.5, D11 |
| `st.eco.vendedor` | Vendedor | Compra e venda pela tabela, revenda a 1,9 ×, reposição por hora de jogo (com avanço de relógio), limites de 50.000 de ouro e 500 itens | §11.3 |
| `st.itm.containers` | Containers | Mochila cheia, peso acima do limite, duas ações na mesma pilha no mesmo passo | §2 (itens) |
| `st.sav.pedestal` | Pedestal de save | Salvar e carregar o slot do sandbox; corromper o checksum de propósito para testar o backup (só depuração) | §15 |
| `st.str.estresse` | Estresse | Gera N criaturas e N itens para o Profiler (o gate da Fase 2 pede 500 itens) | — |

### Fase 2

| ID | Estação | O que testa | Especificação |
|---|---|---|---|
| `st.chr.criacao` | Criação | Tabela de criação (padrão 80/100) e três skills iniciais | §4.4, D6 |
| `st.skl.treino` | Treino | Ganho de skill e de stat por uso, caps, travas e atrofia, com painel de chances | §4.2, §5.2–§5.5, D5 |
| `st.chr.regeneracao` | Regeneração | Vida a cada 11 s e estamina a cada 7 s; mana fica para a magia | §4.3 |
| `st.itm.paperdoll` | Vestiário | Equipar, paperdoll, peso, pilhas, containers aninhados | §2 (itens) |

### Fase 3 (fecha a primeira versão)

| ID | Estação | O que testa | Especificação |
|---|---|---|---|
| `st.mov.elevacao` | Torre | Elevação, rampas, portas, interiores e telhados que somem | §10.5, §3.9 |
| `st.mor.capela` | Capela | Morte, fantasma, corpo do jogador, ressurreição pelo curandeiro e pelo santuário | §8.3, §11.4 |
| `st.cur.bandagens` | Enfermaria | Bandagens em si e em outro, escorregões | §8.1 |
| `st.cid.banco` | Banco | Ouro e banco | §11.2 |
| `st.cid.guarda` | Guarda | Notoriedade básica contra NPCs, zona guardada | §9.1, §10.2 |
| `st.amb.ceu` | Relógio do céu | Dia e noite, curva de luz | §10.4, D9 |
| `st.cmb.escudo` | Escudo | Parry e escudo; arco (PODE) | §6.6, §6.7, D4 |
| `st.cmb.desgaste` | Forja de teste | Durabilidade de armas e desgaste de armadura | §6.4, §6.5 |
| `st.lot.magico` | Baú mágico | Loot com níveis mágicos | §13.4, D10 |
| `st.ia.animal` | Cercado de animais | Arquétipo animal, fuga abaixo de 10%, animais pacíficos | §13.1 |

### Fase 4 (segunda versão)

| ID | Estação | O que testa | Especificação |
|---|---|---|---|
| `st.mag.circulo` | Círculo de conjuração | Círculos, mana, reagentes, tempo e alcance, interrupção, meditação, resistência (a D3 é decidida antes) | §7, D3 |
| `st.col.recursos` | Jazidas | Minério, árvores, pesca, bancos de recurso | §12.1 |
| `st.cra.oficina` | Oficina | Fabricação por receita, ferramenta e estação | §12.2 |
| `st.vnn.laboratorio` | Laboratório | Veneno (quando entrar) | §8.2 |

As fases 5 em diante (agentes, mundo vivo) acrescentam estações próprias quando forem planejadas.

## 5. Ferramentas de depuração (build de desenvolvimento)

| Grupo | Comandos |
|---|---|
| Geração | gerar item, criatura ou NPC por `ContentId`; limpar área; reset da estação |
| Personagem | ajustar stat e skill; restaurar vida, estamina e mana; invulnerável; teleporte para estação |
| Relógio | pausar; avançar um passo (25 ms); avançar N minutos de jogo, executando os passos sem desenhar, de forma determinística, em lotes por frame, com progresso e cancelamento entre lotes (dez minutos são 24.000 passos) |
| Visualização | grade de tiles, chunks e setores de 16 × 16; linha de visão; caminho calculado; estado e percepção da IA; log das rolagens de combate |
| Reprodução | gravar e reexecutar semente + journal de comandos |

O avanço de relógio substitui, para teste, a velocidade 2× da arquitetura (§9): ele roda os mesmos passos, só que sem desenhar, e dá o mesmo resultado que esperar. Segue a política do scheduler da arquitetura (§9, R04): nenhum passo é pulado, e cancelar deixa o mundo num passo inteiro.

## 6. Fora deste documento

- A região jogável da primeira versão (cidade e arredores, D8) é outra região; o sandbox continua como cena de desenvolvimento ao lado dela.
- Se as ferramentas de depuração e o sandbox entram em algum build que não seja de desenvolvimento é decisão da Fase 8. Hoje nenhum build é distribuído.
