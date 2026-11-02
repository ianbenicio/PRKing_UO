# Integração de Claude e Codex com o Unity — pesquisa (2026-11-02)

Objetivo: levantar recursos, aplicativos e sistemas que ligam agentes de código (Claude Code, Codex) ao Editor e ao fluxo de produção do Unity, e recomendar o que usar no Outrora quando o Unity 6.6 estiver instalado.

Método: busca na web e leitura das páginas primárias (blog e documentação da Unity, README dos repositórios). Onde só há fonte secundária, o texto diz. Os produtos estão em beta e mudam rápido: conferir a versão antes de instalar.

## 1. Opções encontradas

### Oficiais da Unity

| Recurso | O que faz | Requisitos e custo | Fonte |
|---|---|---|---|
| **Plugin oficial da Unity para Claude Code** (9 set. 2026) | Instala 29 skills escritas pela Unity (UI, 2D, renderização, áudio, cenas, pacotes, plataformas etc.), entre elas `/unity-cli` (comanda o Unity pelo terminal, inclusive o Editor aberto), `/new-unity-project` e `/unity-package-management`. Instala pelo terminal ou pelo Claude Code do app Claude Desktop | Versão do Editor e custo não informados na página. A Unity diz que as skills passaram por revisão de segurança. Claude Code é o primeiro agente; outros "ainda este ano" | [blog da Unity](https://unity.com/en/blog/unity-plugin-for-claude-code); [plugin](https://claude.com/plugins/unity); [docs](https://docs.unity.com/en-us/ai/unity-plugin/claude-code) |
| **Plugin oficial da Unity para Codex** (set. 2026) | Mesma linha do plugin do Claude Code: skills da Unity e `unity-cli` (instalar Editor, criar e abrir projeto, pacotes, logs). Unity 6 ou mais nova | **Só fonte secundária**; a página do GameDev.net recusou a leitura (403) | [Inven Global](https://www.invenglobal.com/articles/26087/unity-releases-official-plugin-dedicated-to-openai-codex) |
| **Unity MCP server** (pacote `com.unity.ai.assistant`, beta) | Liga o agente ao Editor aberto: hierarquia da cena, GameObjects, componentes, build settings, console, criar e editar scripts C#. Aceita ferramentas próprias registradas em C#. O painel Integrations configura Claude Code, Cursor, Windsurf e Claude Desktop | Unity 6 (6000.0) ou mais nova, projeto ligado à Unity Cloud e **trial ou assinatura das ferramentas de IA da Unity**. Segundo a página de preços, o MCP não gasta créditos; o plano Personal tem 1 conexão MCP e a assinatura custa US$ 10/mês (conferir: preços em beta). Segurança: aprovação só na primeira conexão ("Pending Connection" → Accept) | [blog MCP](https://unity.com/blog/unity-ai-mcp-how-to-get-started); [docs MCP](https://docs.unity3d.com/Packages/com.unity.ai.assistant@2.3/manual/unity-mcp-get-started.html) |
| **AI Gateway** (mesmo pacote, pré-lançamento 2.9) | Roda agentes de terceiros dentro da janela do Assistant: Claude Code, Codex CLI, Cursor CLI, Gemini CLI, com as credenciais do próprio usuário. Usa versões empacotadas do Codex CLI e do Gemini CLI | Segundo a Unity, não gasta créditos; a página de preços marca "assinatura mensal exigida" (não confirmado se o Personal libera) | [docs AI Gateway](https://docs.unity3d.com/Packages/com.unity.ai.assistant@2.9/manual/integration/ai-gateway-intro.html); [preços](https://unity.com/es/pricing/compare-plans) |

### Comunidade (código aberto, sem vínculo com a Unity)

| Projeto | Licença | Unity | Clientes | Observações |
|---|---|---|---|---|
| [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp) ("MCP for Unity") | MIT | 2021.3 LTS a 6.x | Claude Desktop e Code, Codex, Cursor, VS Code, Windsurf, Gemini CLI | v10.3.0 (4 out. 2026); cerca de 50 ferramentas (cenas, scripts, assets, testes, profiling, build); precisa de Python 3.10+ via `uv`; instala por Git URL ou OpenUPM; configuração automática dos clientes no menu do Unity. Telemetria não informada no README |
| [CoderGamester/mcp-unity](https://github.com/CoderGamester/mcp-unity) | MIT | 6 ou mais nova | Claude Code, Codex CLI, Cursor, Windsurf, Copilot, Claude Desktop | Ponte WebSocket local (porta 8090) com token por projeto; `add_package` desligado por padrão porque pacote roda código no Editor; precisa de Node 18+. Diz aceitar caminho com espaço, mas recomenda tirar o espaço se a conexão falhar |
| [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) ("AI Game Developer") | Apache-2.0 | não informado | Claude Code, Codex, Copilot, Cursor e outros | Mais de 70 ferramentas, inclusive compilar e executar C# arbitrário e chamar qualquer método por reflexão; também roda dentro do jogo compilado. **O caminho padrão passa por servidor na nuvem (`ai-game.dev`) com login**; autenticação local padrão "none". **Exige caminho do projeto sem espaços** |

Há outros forks menores do mcp-unity e integrações de produto (por exemplo, Convai para personagens com IA) que não interessam ao projeto agora.

## 2. Pontos que pesam no Outrora

1. **Caminho com espaços.** O projeto está em `H:\King\Clientes e Projetos\PRKing_UO`. O Unity-MCP do IvanMurzak não aceita espaço; o mcp-unity pode falhar. A renomeação da pasta (Anexo A, A7) não resolve, porque o espaço está em "Clientes e Projetos".
2. **Nada sai da máquina sem decisão.** O NFR-006 vale para o build, mas a mesma lógica pede cautela no Editor: o MCP oficial exige projeto ligado à Unity Cloud, e o Unity-MCP do IvanMurzak usa servidor na nuvem por padrão. O mcp-unity e o MCP for Unity rodam localmente.
3. **Execução de código no Editor.** Todas as opções deixam o agente criar e editar scripts, e algumas executam C# arbitrário. Nenhuma pede aprovação por ação: o controle é o do agente (permissões do Claude Code ou do Codex) e o git.
4. **Sala limpa.** As ferramentas não trazem código de clientes nem de emuladores do jogo de referência; as skills oficiais são documentação da própria Unity. A regra continua valendo para o que o agente buscar por conta própria: regras só da especificação.
5. **Monetização.** O plugin oficial traz skills de monetização (LevelPlay, compras no app). O Outrora não tem monetização (NFR-010): essas skills não se usam.
6. **Versão.** O plugin oficial e o MCP oficial pedem Unity 6 ou mais nova; o projeto usa a 6.6, então serve.

## 3. Recomendação

1. **Plugin oficial da Unity para Claude Code**, junto com a instalação do Unity 6.6. É da Unity, cobre `unity-cli`, criação de projeto e pacotes, e a Story 1.1a começa justamente por isso. Instalar no escopo do projeto, não no global, e conferir se pede assinatura.
2. **Para controlar o Editor aberto**, escolher um MCP:
   - **MCP oficial da Unity**, se o responsável aceitar a assinatura das ferramentas de IA e o projeto ligado à Unity Cloud;
   - senão, **MCP for Unity (CoplayDev)**: MIT, local, cobre a 6.6, roda testes e profiling, que a Story 1.1a usa. Fixar a versão (tag) e auditar antes, como as ferramentas do PRKing_REA.
3. **Evitar por ora** o Unity-MCP do IvanMurzak: nuvem por padrão, execução de C# arbitrário e incompatível com o caminho atual.
4. **Codex:** o plugin oficial existe, segundo fonte secundária; vale reavaliar se o responsável quiser usar o Codex em paralelo.

Decisões do responsável antes de instalar: assinatura das ferramentas de IA da Unity (sim ou não) e qual MCP adotar.
