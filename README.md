# Outrora

Projeto de pesquisa e arquitetura para um RPG isométrico local em Unity, inspirado nos sistemas de um MMO clássico na era T2A (1998–2000). A primeira entrega não inclui rede, autenticação nem servidor dedicado: toda a simulação é autoritativa e executada na máquina do jogador.

O nome do produto é **Outrora** (adotado pelo responsável em 2026-11-01). Nunca usar "Ultima", "Ultima Online" nem "Britannia" como nome, título ou marca do produto, do repositório ou de material de divulgação (especificação §17).

## Fonte das regras (sala limpa)

- A única fonte de regras, fórmulas, formatos de arquivo e escopo é a [especificação](docs/spec/especificacao.md), cópia da especificação do PRKing_REA presa ao hash do commit de origem. O registro de entrega e o caminho para pedir adendos estão em [docs/spec/README.md](docs/spec/README.md).
- O projeto nunca lê `alvos/`, `saidas/`, evidência, pseudocódigo nem os arquivos de pesquisa do PRKing_REA.
- O projeto não abre o código de clientes e emuladores de código aberto (ClassicUO, RunUO, ModernUO, ServUO, MobileUO, UO Offline e outros) ao escrever o seu.
- Pergunta que a especificação não responde vira pergunta de volta (registrada em `docs/spec/README.md`), nunca pesquisa por conta própria no jogo ou no código de terceiros.

## Estado atual

- Pesquisa inicial de fontes oficiais, comunidades e projetos open source concluída (contexto; as regras vêm da especificação).
- Arquitetura offline e estratégia de dados definidas.
- PRD, roadmap e primeiras stories (1.1a e 1.1b) preparados e alinhados à especificação em 2026-11-02 (Anexo A da especificação).
- Implementação Unity ainda não iniciada; o Unity Editor não está instalado nesta máquina.

## Decisões iniciais

- Unity 6.6 (6000.6.7f1) como versão de produção (decisão do responsável em 2026-10-28). Não é LTS: migrar para a 6.7 LTS quando sair.
- Perfil de regras T2A, no estado de março de 2000 (decisão D1); Renaissance, Trammel e Age of Shadows ficam fora. Os números ficam em tabelas de dados, com dois perfis: "T2A estrito" e "pré-AOS final" (especificação, "Modelo numérico").
- Código de domínio em C# puro, separado de `MonoBehaviour` e da apresentação.
- Visão de longo prazo: mundo local vivo por NPCs/agentes, economia, rotinas, crafting e persistência. A primeira versão jogável é o loop básico (abaixo).
- Modelo misto de conteúdo (decisão de 2026-10-28): conteúdo próprio como base e padrão; leitor opcional da instalação do próprio jogador, só em tempo de execução, desligado por padrão e só para uso pessoal (decisão de 2026-11-01). Não há importador offline nem cache em disco.
- Nada da EA no git, no build, no save ou em capturas versionadas.
- Nenhum build é distribuído por ora. Sem monetização, nem doação ligada ao jogo. Nenhum porte para outras plataformas sem nova avaliação do responsável.
- Decisões D1–D11 da especificação aceitas em 2026-11-01; a D3 (resistência a magia) fica adiada até a magia. Resumo no [PRD](docs/prd/prd.md), §12.

## Primeira versão jogável (loop básico)

Mundo isométrico navegável; stats e skills com ganho; combate corpo a corpo contra criaturas com IA simples; loot; morte e ressurreição; uma cidade com vendedor e banco; dia e noite; salvar e carregar (especificação §1). Fora dela: magia (segunda versão), crafting e coleta, housing e barcos, pets e domesticação, guildas, multijogador e protocolo de rede.

## Documentação

- [Especificação (cópia entregue)](docs/spec/especificacao.md) e [registro de entrega](docs/spec/README.md)
- [Dossiê de pesquisa](docs/research/dossie-pesquisa-inicial.md)
- [Fontes e política de evidências](docs/research/source-ledger.md)
- [Integração de Claude e Codex com o Unity (pesquisa)](docs/research/2026-11-02-integracao-ia-unity.md)
- [PRD](docs/prd/prd.md)
- [Arquitetura Unity offline](docs/architecture/unity-offline-architecture.md)
- [Sandbox de testes](docs/architecture/sandbox-de-testes.md)
- [Roadmap](docs/roadmap.md)
- [Story 1.1a — fundação Unity e sandbox de testes](docs/stories/1.1a-unity-foundation-and-sandbox.md)
- [Story 1.1b — combate, IA, loot, vendedor e save completo](docs/stories/1.1b-combat-ai-loot-vendor-and-save.md)

## Licença

Todos os direitos reservados enquanto o projeto for local (decisão do responsável em 2026-11-02). A licença será decidida antes de o repositório ficar público. Nenhuma licença permite copiar código de terceiros: a sala limpa vale em qualquer caso.

A pasta e o repositório `PRKing_UO` serão renomeados para um nome neutro antes de o repositório ficar público (decisão de 2026-11-02, especificação, Anexo A, A7).

## Próximo passo operacional

Instalar o Unity 6.6 (6000.6.7f1), implementar a Story 1.1a (projeto, cena sandbox em chunks, movimento, save básico e painel de depuração) e depois a Story 1.1b (combate corpo a corpo contra uma criatura, loot, um vendedor e save/load completo).
