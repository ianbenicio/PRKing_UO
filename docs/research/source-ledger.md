# Source ledger e política de evidências

## Política

Cada dado importado para o projeto deverá registrar:

- `sourceUrl`: origem pública;
- `sourceType`: official, open-source, specialist-wiki, forum ou inference;
- `era`: período ao qual a informação se aplica;
- `verifiedAt`: data da verificação;
- `licenseNote`: permissão ou restrição de reutilização;
- `confidence`: high, medium ou low.

Uma fonte descrevendo o UO moderno não pode ser usada silenciosamente para uma regra T2A. Um relato de fórum pode gerar hipótese e teste, mas não se torna fórmula apenas por ser popular.

**Sala limpa (desde 2026-11-02).** Sobre o jogo de referência, regras, fórmulas, formatos de arquivo e escopo vêm só da [especificação](../spec/especificacao.md). As fontes abaixo ficam como registro da pesquisa inicial: nenhuma regra nova sai delas diretamente, e o código dos repositórios listados não é aberto ao escrever o projeto. Pergunta que a especificação não responde segue a pergunta de volta descrita em [docs/spec/README.md](../spec/README.md). A documentação da Unity continua fonte normal para a engine.

## Fontes de primeira linha

| Fonte | Tipo | Uso autorizado no projeto | Atenção |
|---|---|---|---|
| [UO Wiki oficial](https://uo.com/wiki/ultima-online-wiki/) | official | taxonomia, terminologia, comportamento documentado | conteúdo e redação têm copyright; parafrasear |
| [Previous Publishes](https://uo.com/wiki/ultima-online-wiki/technical/previous-publishes/) | official | reconstruir quando regras mudaram | validar data e contexto da era |
| [Manual/guia original](https://www.mocagh.org/origin/uo-manual.pdf) | primary historical copy | experiência original, comandos, skills e mundo | não redistribuir o PDF no projeto |
| [Unity 6 support](https://unity.com/releases/unity-6/support) | official | versão do Editor e suporte | revisar antes de upgrades |
| [Unity Isometric Tilemaps](https://docs.unity3d.com/Manual/Tilemap-Isometric.html) | official | configuração de grid e sorting | o renderer final pode precisar de chunks customizados |
| [Unity ScriptableObject](https://docs.unity3d.com/6000.1/Documentation/Manual/class-ScriptableObject.html) | official | dados autorais de conteúdo | não usar como formato de save em build |
| [Unity Input System](https://docs.unity3d.com/Manual/com.unity.inputsystem.html) | official | input remapeável | fixar versão compatível com o Editor |
| [Unity UI Toolkit](https://docs.unity3d.com/6000.0/Documentation/Manual/ui-systems/introduction-ui-toolkit.html) | official | HUD, gumps, menus e tooling | validar drag/drop e performance em protótipo |
| [Unity Test Framework](https://docs.unity3d.com/Manual/com.unity.test-framework.html) | official | EditMode e PlayMode tests | manter domínio testável sem Scene |

## Fontes especializadas

| Fonte | Uso principal | Confiança |
|---|---|---:|
| [UOGuide](https://www.uoguide.com/Main_Page) | skills, crafting, itens, criaturas e mecânicas | medium/high quando cita o oficial |
| [UOGuide Reborn](https://uoguidereborn.com/) | índice moderno de sistemas | medium |
| [Stratics](https://community.stratics.com/) | histórico, dúvidas, efeitos percebidos e arquivos | medium/low por post |
| [UO Asset Studio guide](https://www.uoassets.com/studio/guide/UOAssetStudio-User-Guide-0.33.13) | dimensões e tooling de mapa | high para formatos suportados |
| [UOFiddler](https://uofiddler.polserver.com/) | catálogo de arquivos de cliente | high para capacidade da ferramenta |
| [Raph Koster](https://www.raphkoster.com/games/) | intenção de design, economia, ecologia e postmortem | high para história de desenvolvimento |
| [GDC UO postmortem](https://gdcvault.com/play/1025402/Classic-Game-Postmortem-Ultima-Online) | decisões, sucessos e falhas do produto | high |

## Implementações open source

Pela sala limpa, nenhum código desses repositórios é aberto ao escrever o projeto nem copiado; formatos e regras vêm da especificação (§3 e §4–§13). A coluna do meio registra o que a pesquisa inicial usou, não um uso permitido daqui em diante.

| Repositório | Uso na pesquisa inicial | Restrição |
|---|---|---|
| [ClassicUO](https://github.com/ClassicUO/ClassicUO) | arquitetura e formatos | arquivo LICENSE BSD-2-Clause; o README diz "BSD 4"; não concede assets da EA |
| [ModernUO](https://github.com/modernuo/ModernUO) | comparar comportamento e eras | GPL-3.0 |
| [UOFiddler](https://github.com/polserver/UOFiddler) | catálogo de arquivos do cliente | "Beerware" só no README; dados lidos continuam protegidos |
| [Ultima SDK Python](https://github.com/UltimaWorks/ultima-sdk-python) | referência citada para `.mul/.idx/.uop` | licença não verificada; a especificação §17 cita um "Ultima SDK" com "Beerware" só no README, sem dizer se é este pacote |
| [UO Offline](https://github.com/Klein187/uo-offline) | requisitos de AI, economia, navegação e UX offline | se deriva do ModernUO, herda a GPL-3.0 (inferência da especificação); só referência de comportamento |
| MobileUO | cliente em Unity baseado no ClassicUO | GPLv3 no README |
| POL | emulador | termo de uso próprio condiciona até a leitura do código: não usar |
| xrip/uo-client | cliente | diz derivar de descompilação do cliente oficial: excluído da sala limpa |

## Perguntas de pesquisa abertas

- Qual subconjunto exato de skills e fórmulas define o perfil `Classic`? **Resolvida (2026-11-02):** especificação §4–§13, com as 49 skills da §5.1 e as decisões D1–D11.
- O produto final será somente inspirado em UO ou terá um importador pessoal de dados legítimos? **Resolvida:** modelo misto, sem importador; leitor opcional em tempo de execução, só para uso pessoal (especificação §3.0 e §17).
- Qual tamanho de região mantém exploração interessante para um único jogador? Para a primeira versão: uma cidade e arredores com uma área de criaturas (D8); o tamanho exato do recorte segue aberto.
- Quantos agentes próximos e distantes formam um mundo vivo no hardware mínimo?
- Qual é a melhor representação visual: pixel art autoral 2D, 2.5D ou low-poly isométrico?
- O combate permanece em tempo real contínuo ou ganha pausa tática opcional?
- Qual intensidade de morte, perda de itens e PK simulado é desejável? **Resolvida para a primeira versão:** o padrão são as regras da especificação §8.3 e §9; intensidade configurável fica para depois.

## Processo de atualização

1. Registrar a questão e a era.
2. Procurar a resposta na especificação (`docs/spec/especificacao.md`).
3. Se ela não responder, registrar a pergunta de volta em `docs/spec/README.md` e, enquanto espera, decidir por conta própria documentando a decisão, ou deixar o ponto parado. Nunca ir ao jogo, à evidência ou ao código de terceiros para responder.
4. Quando o adendo chegar, atualizar a cópia da especificação (novo hash) e o registro de entrega.
5. Criar teste de aceitação ou simulação de balanceamento.
6. Salvar a decisão no PRD/ADR correspondente.

