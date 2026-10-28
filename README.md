# PRKing UO Offline

Projeto de pesquisa e arquitetura para um RPG isométrico local em Unity inspirado nos sistemas de *Ultima Online*. A primeira entrega não inclui rede, autenticação nem servidor dedicado: toda a simulação é autoritativa e executada na máquina do jogador.

## Estado atual

- Pesquisa inicial de fontes oficiais, comunidades e projetos open source concluída.
- Arquitetura offline e estratégia de dados definidas.
- PRD, roadmap e primeira story preparados.
- Implementação Unity ainda não iniciada; o Unity Editor não está instalado nesta máquina.

## Decisões iniciais

- Unity 6.6 (6000.6.7f1) como versão de produção (decisão do responsável em 2026-10-28). Não é LTS: migrar para a 6.7 LTS quando sair.
- Perfil clássico T2A/Renaissance como primeiro conjunto de regras, sem impedir outros perfis.
- Código de domínio em C# puro, separado de `MonoBehaviour` e da apresentação.
- Mundo local vivo por NPCs/agentes, economia, rotinas, crafting e persistência.
- Conteúdo original como padrão para uma distribuição segura.
- Importador opcional de dados de uma instalação legítima de UO pode ser estudado separadamente; nenhum asset da EA será versionado ou distribuído.

## Documentação

- [Dossiê de pesquisa](docs/research/ultima-online-dossier.md)
- [Fontes e política de evidências](docs/research/source-ledger.md)
- [PRD](docs/prd/uo-offline-prd.md)
- [Arquitetura Unity offline](docs/architecture/unity-offline-architecture.md)
- [Roadmap](docs/roadmap.md)
- [Story 1.1 — fundação e vertical slice](docs/stories/1.1-unity-foundation-and-local-vertical-slice.md)

## Ferramentas auxiliares

- [Hardware Monitor Gadget](packages/hardware-monitor-gadget/README.md) — overlay para Windows 10 com temperatura de CPU e GPU.

## Ferramentas auxiliares

- [Hardware Monitor Gadget](packages/hardware-monitor-gadget/README.md) — overlay para Windows 10 com temperatura de CPU e GPU.

## Próximo passo operacional

Instalar o Unity 6.6 (6000.6.7f1), abrir a Story 1.1 e construir um vertical slice pequeno: mapa isométrico em chunks, personagem, interação, inventário, um recurso coletável, uma receita, um NPC e save/load local.

