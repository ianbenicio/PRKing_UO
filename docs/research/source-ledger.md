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

| Repositório | Extração permitida nesta fase | Restrição |
|---|---|---|
| [ClassicUO](https://github.com/ClassicUO/ClassicUO) | estudar arquitetura e formatos; reutilização apenas com atribuição/licença | BSD-2-Clause não concede assets da EA |
| [ModernUO](https://github.com/modernuo/ModernUO) | comparar comportamento e eras | GPL-3.0 exige decisão de licenciamento antes de código derivado |
| [UOFiddler](https://github.com/polserver/UOFiddler) | avaliar pipeline de importação local | Beerware; dados lidos continuam protegidos |
| [Ultima SDK Python](https://github.com/UltimaWorks/ultima-sdk-python) | referência para `.mul/.idx/.uop` | verificar licença do pacote antes de integrar |
| [UO Offline](https://github.com/Klein187/uo-offline) | requisitos de AI, economia, navegação e UX offline | não copiar implementação sem auditoria de licença |

## Perguntas de pesquisa abertas

- Qual subconjunto exato de skills e fórmulas define o perfil `Classic`?
- O produto final será somente inspirado em UO ou terá um importador pessoal de dados legítimos?
- Qual tamanho de região mantém exploração interessante para um único jogador?
- Quantos agentes próximos e distantes formam um mundo vivo no hardware mínimo?
- Qual é a melhor representação visual: pixel art autoral 2D, 2.5D ou low-poly isométrico?
- O combate permanece em tempo real contínuo ou ganha pausa tática opcional?
- Qual intensidade de morte, perda de itens e PK simulado é desejável?

## Processo de atualização

1. Registrar a questão e a era.
2. Procurar fonte oficial ou documentação histórica.
3. Comparar pelo menos uma implementação e uma fonte comunitária quando houver ambiguidade.
4. Escrever uma especificação própria, sem copiar texto ou código.
5. Criar teste de aceitação ou simulação de balanceamento.
6. Salvar a decisão no PRD/ADR correspondente.

