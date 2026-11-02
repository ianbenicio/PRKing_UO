# Integração de Claude e Codex com o Unity — pesquisa (2026-11-02)

> **Documento complementar:** a [verificação de custos, telemetria e relatos](2026-11-02-integracao-ia-unity-verificacao.md), feita em paralelo na mesma data, chega às mesmas conclusões e traz o roteiro de validação prática (§5). As duas foram conciliadas: as correções da conciliação estão na nota abaixo.

Objetivo: levantar recursos, aplicativos e sistemas que ligam agentes de código (Claude Code, Codex) ao Editor e ao fluxo de produção do Unity, e recomendar o que usar no Outrora quando o Unity 6.6 estiver instalado.

Método: busca na web e leitura das páginas primárias (blog e documentação da Unity, README e código dos repositórios, lidos por `gh api` sem clonar). Onde só há fonte secundária, o texto diz. Os produtos estão em beta e mudam rápido: conferir a versão antes de instalar.

Revisão (mesma data): conferidos o custo do plugin oficial e a telemetria do MCP for Unity, e acrescentada a opinião de desenvolvedores (seção 3). A seção 1 corrige a versão anterior: o MCP do pacote de IA da Unity foi descontinuado, e o substituto é gratuito.

Conciliação com a verificação paralela (mesma data): (1) a CLI da Unity também tem telemetria, um ping por execução, com pergunta de consentimento na primeira execução; (2) a CLI está na 1.0.0-beta.15 (29 out. 2026), com mudanças incompatíveis; (3) o `env` da configuração do cliente MCP só chega ao processo que o cliente inicia, não ao Editor; (4) o site do MCP for Unity diz "no telemetry by default", o que o código da v10.3.0 contradiz.

## 1. Opções encontradas

### Oficiais da Unity

| Recurso | O que faz | Requisitos e custo | Fonte |
|---|---|---|---|
| **Plugin oficial da Unity** para Claude Code, Codex, Grok e Antigravity (set. 2026; v0.1.8-beta) | Instala skills escritas pela Unity (UI, 2D, renderização, áudio, cenas, pacotes, plataformas etc.), entre elas `/unity-cli`, `/new-unity-project` e `/unity-package-management`. Segundo a documentação, "só traz skills" e não usa servidor MCP; a página de recursos diz que o plugin já inclui a CLI e o MCP (as fontes divergem; conferir na instalação). O README avisa que as skills mexem direto no projeto: fazer commit antes | **Gratuito**, sem limite de uso. Unity 6 ou mais nova. Licença Unity Companion License (termos completos não lidos). Instalação no Claude Code: `/plugin marketplace add Unity-Technologies/unity-agent-plugin` e `/plugin install unity@unity-agent-plugin` (ou, no terminal, `claude plugin install unity@unity-agent-plugin --scope local`). No Codex: `codex plugin marketplace add Unity-Technologies/unity-agent-plugin` e `codex plugin add unity@unity-agent-plugin` | [repositório](https://github.com/Unity-Technologies/unity-agent-plugin); [docs](https://docs.unity.com/en-us/ai/unity-plugin/claude-code); [recursos de IA](https://unity.com/features/ai); [blog](https://unity.com/en/blog/unity-plugin-for-claude-code) |
| **Unity CLI com `unity mcp`** (1.0.0-beta.15, 29 out. 2026) | Comanda o Unity pelo terminal (instalar Editor, criar e abrir projeto, pacotes, testes, logs) e expõe um servidor MCP que liga o agente ao Editor aberto. Usa o pacote Unity Pipeline. Comandos: `unity pipeline install`, `unity mcp configure <cliente>`, `unity skill install <agente>`, `unity doctor`, `unity status` | **Gratuito e separado do Unity AI**: sem assinatura nem créditos. A ponte com o Editor é só localhost e funciona offline. **Telemetria:** um ping anônimo por execução, perguntado na primeira execução; `unity analytics status` mostra o estado e `UNITY_NO_CLI_INVOKED_TELEMETRY` desliga. Editor aberto pede Unity 6.0 LTS ou mais nova. Beta: a beta.14 e a beta.15 mudaram códigos de saída, o formato JSON e removeram comandos (`unity projects new`, `unity list`). Fixar a versão por projeto em `ProjectSettings/UnityCliConfig.json` (`cli.version`) | [substituição do MCP pela CLI](https://docs.unity.com/en-us/unity-cli/replace-mcp-server-unity-cli); [notas de versão](https://docs.unity.com/en-us/unity-cli/release-notes); [anúncio da beta.15](https://discussions.unity.com/t/unity-cli-1-0-0-beta-15-is-rolling-out/1740651); [recursos de IA](https://unity.com/features/ai) |
| ~~Unity MCP server do pacote `com.unity.ai.assistant`~~ | **Descontinuado.** A Unity diz que descontinuou o componente MCP do assistente de IA do Editor; o substituto é o `unity mcp` acima. Exigia projeto ligado à Unity Cloud e assinatura das ferramentas de IA | Não usar. A versão 2.13 ou mais nova do AI Assistant corrige o conflito com a CLI | [substituição do MCP pela CLI](https://docs.unity.com/en-us/unity-cli/replace-mcp-server-unity-cli) |
| **AI Gateway** (pacote `com.unity.ai.assistant`, pré-lançamento 2.9) | Roda agentes de terceiros dentro da janela do Assistant: Claude Code, Codex CLI, Cursor CLI, Gemini CLI, com as credenciais do próprio usuário | O assistente dentro do Editor gasta créditos: no Personal é um trial que vira assinatura de US$ 10/mês. Não é necessário para o Outrora | [docs AI Gateway](https://docs.unity3d.com/Packages/com.unity.ai.assistant@2.9/manual/integration/ai-gateway-intro.html); [preços](https://unity.com/es/pricing/compare-plans) |

### Comunidade (código aberto, sem vínculo com a Unity)

| Projeto | Licença | Unity | Clientes | Observações |
|---|---|---|---|---|
| [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp) ("MCP for Unity") | MIT | 2021.3 LTS a 6.x | Claude Desktop e Code, Codex, Cursor, VS Code, Windsurf, Gemini CLI | v10.3.0 (4 out. 2026); cerca de 15,6 mil estrelas; cerca de 50 ferramentas (cenas, scripts, assets, testes, profiling, build); precisa de Python 3.10+ via `uv`; instala por Git URL ou OpenUPM. **Telemetria ligada por padrão** (detalhes abaixo) |
| [CoderGamester/mcp-unity](https://github.com/CoderGamester/mcp-unity) | MIT | 6 ou mais nova | Claude Code, Codex CLI, Cursor, Windsurf, Copilot, Claude Desktop | Ponte WebSocket local (porta 8090) com token por projeto; `add_package` desligado por padrão porque pacote roda código no Editor; precisa de Node 18+. Diz aceitar caminho com espaço, mas recomenda tirar o espaço se a conexão falhar |
| [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) ("AI Game Developer") | Apache-2.0 | não informado | Claude Code, Codex, Copilot, Cursor e outros | Mais de 70 ferramentas, inclusive compilar e executar C# arbitrário e chamar qualquer método por reflexão. **O caminho padrão passa por servidor na nuvem (`ai-game.dev`) com login**; autenticação local padrão "none". **Exige caminho do projeto sem espaços** |

Há outros forks menores do mcp-unity e integrações de produto (por exemplo, Convai para personagens com IA) que não interessam ao projeto agora.

### Telemetria do MCP for Unity (conferida no código, ramo `beta`)

- **Ligada por padrão** (opt-out): `telemetry_enabled: bool = True` em `Server/src/core/config.py`, igual no ramo `beta` e na tag v10.3.0. O site do projeto diz o contrário ("no telemetry by default"); vale o código.
- **Destino:** `https://api-prod.coplay.dev/telemetry/events`, por HTTPS POST, timeout de 10 s. O endereço pode ser trocado por `UNITY_MCP_TELEMETRY_ENDPOINT`, mas o código recusa localhost.
- **O que envia, segundo a documentação do projeto:** ferramentas usadas, tempo de execução e sucesso ou falha, versão do Unity, sistema operacional, versão do MCP, eventos de primeira vez, falhas de conexão, mensagens de erro cortadas em 200 caracteres. Diz não enviar código, nomes de projeto, nomes de arquivo nem caminhos. Identificador: UUID aleatório por instalação, guardado em `%APPDATA%\UnityMCP\customer_uuid.txt` e nas EditorPrefs.
- **Ponto de atenção:** uma mensagem de erro de 200 caracteres pode conter caminho ou nome de arquivo, apesar da promessa.
- **Como desligar:** as variáveis `DISABLE_TELEMETRY`, `UNITY_MCP_DISABLE_TELEMETRY` e `MCP_DISABLE_TELEMETRY` são equivalentes, e o servidor Python e o lado C# leem as três. Usar o valor `1`, que os dois lados aceitam. **A variável precisa chegar aos dois processos:** o bloco `env` da configuração do cliente MCP (`"DISABLE_TELEMETRY": "1"`) só alcança o servidor que o cliente inicia; o Editor do Unity precisa da variável no próprio ambiente ou da chave `MCPForUnity.TelemetryDisabled` nas EditorPrefs. O menu "Disable Telemetry" ainda consta como "em breve" na documentação.

Fontes: [documentação de telemetria](https://github.com/CoplayDev/unity-mcp/blob/beta/website/docs/architecture/telemetry.md); [`config.py`](https://github.com/CoplayDev/unity-mcp/blob/beta/Server/src/core/config.py); [`TelemetryHelper.cs`](https://github.com/CoplayDev/unity-mcp/blob/beta/MCPForUnity/Editor/Helpers/TelemetryHelper.cs).

## 2. Pontos que pesam no Outrora

1. **Caminho com espaços.** O projeto está em `H:\King\Clientes e Projetos\PRKing_UO`. O Unity-MCP do IvanMurzak não aceita espaço; o mcp-unity pode falhar. Não há issue aberta sobre espaço no MCP for Unity. A renomeação da pasta (Anexo A, A7) não resolve, porque o espaço está em "Clientes e Projetos".
2. **Nada sai da máquina sem decisão.** O NFR-006 vale para o build, mas a mesma lógica pede cautela no Editor: a CLI da Unity liga ao Editor só por localhost, mas pergunta se pode mandar um ping de uso por execução (responder não, ou `UNITY_NO_CLI_INVOKED_TELEMETRY`); o MCP for Unity manda telemetria por padrão (desligável); o Unity-MCP do IvanMurzak usa nuvem por padrão. O próprio agente (Claude ou Codex) roda na nuvem e recebe o que o projeto lhe mostrar. Nenhuma ponte pode entrar no build do jogo.
3. **Execução de código no Editor.** Todas as opções deixam o agente criar e editar scripts, e algumas executam C# arbitrário. Nenhuma pede aprovação por ação: o controle é o do agente (permissões do Claude Code ou do Codex) e o git.
4. **Sala limpa.** As ferramentas não trazem código de clientes nem de emuladores do jogo de referência; as skills oficiais são documentação da própria Unity. A regra continua valendo para o que o agente buscar por conta própria: regras só da especificação.
5. **Monetização.** O plugin oficial traz skills de monetização (LevelPlay, compras no app). O Outrora não tem monetização (NFR-010): essas skills não se usam.
6. **Versão.** O plugin oficial e a CLI pedem Unity 6 ou mais nova; o projeto usa a 6.6, então serve.
7. **Custo de contexto.** Muitas ferramentas MCP ocupam contexto do agente a cada sessão (medições na seção 3). A própria Unity diz que comandos da CLI são mais rápidos e gastam menos tokens que o MCP.

## 3. O que os desenvolvedores falam

### Plugin oficial e CLI da Unity

- **Fórum da Unity, anúncio do plugin** ([tópico](https://discussions.unity.com/t/1736143)): poucas respostas. Um elogio, uma reclamação da propaganda de IA no Unity Hub e perguntas sobre o Codex e sobre como tirar skills instaladas à mão antes. Nenhum relato de defeito.
- **Dicas da equipe da Unity** ([tópico](https://discussions.unity.com/t/1740182)): rodar `/init` para gerar `CLAUDE.md` ou `AGENTS.md`; instalar os plugins oficiais; cortar ferramentas MCP que não se usam; `unity doctor` quando algo falhar; remover skills sobrepostas; usar o modo de planejamento; rodar testes com `unity test . --mode EditMode`; commit antes de cada tarefa; mexer em cenas e prefabs pelas APIs do Editor ou pela CLI, não editando o YAML à mão.
- **MCP oficial contra o assistente** ([tópico](https://discussions.unity.com/t/1719106), maio de 2026): 5 de 8 mensagens reclamam que o MCP exigia assinatura. A reclamação ficou velha: o MCP pago foi descontinuado e o `unity mcp` da CLI é gratuito.
- **CLI sem ferramentas** ([tópico](https://discussions.unity.com/t/cli-unity-mcp-no-tool-available/1739074), 13 out. 2026, beta.12 e beta.13, cliente Mistral Vibe): `unity mcp` conectou sem nenhuma ferramenta; só apareceram depois de instalar outro pacote, e a equipe da Unity não achou a causa.
- **Anúncio da beta.15** ([tópico](https://discussions.unity.com/t/unity-cli-1-0-0-beta-15-is-rolling-out/1740651), 29 out. 2026): a única queixa nas respostas foi a de skills desatualizadas depois da atualização ("Update your skills, or it's not usable").
- **StraySpark** ([blog](https://www.strayspark.studio/blog/unity-mcp-vs-unity-cli-2026), 21 out. 2026; a empresa vende um MCP próprio, então ler com cautela): a CLI ainda é experimental (1.0.0-beta.13); o MCP da Unity expõe 171 ferramentas, cerca de 27,7 mil tokens de contexto; relato de "toda chamada MCP devolve 401 depois de entrar em Play Mode".

### MCP for Unity (CoplayDev)

É o MCP comunitário mais usado. Das 56 issues abertas no GitHub, os temas que se repetem são:

- **Domain reload, Play Mode e `run_tests`** (cerca de 10 issues): a ponte cai ou trava quando o Unity recompila ou entra em Play Mode.
- **Editor sem foco** ([#1491](https://github.com/CoplayDev/unity-mcp/issues/1491), [#1454](https://github.com/CoplayDev/unity-mcp/issues/1454), [#1407](https://github.com/CoplayDev/unity-mcp/issues/1407)): comandos param até alguém clicar no Editor.
- **Quedas de conexão** (cerca de 8 issues).
- **Windows** ([#1407](https://github.com/CoplayDev/unity-mcp/issues/1407), [#1394](https://github.com/CoplayDev/unity-mcp/issues/1394), [#1248](https://github.com/CoplayDev/unity-mcp/issues/1248)): erro do `pywin32` (erro 32) no transporte stdio via `uvx`.
- **Claude Code** ([#1397](https://github.com/CoplayDev/unity-mcp/issues/1397)): o painel mostra "Not Configured" com a conexão ativa.
- **Vários Editores abertos** ([#1023](https://github.com/CoplayDev/unity-mcp/issues/1023), [#1299](https://github.com/CoplayDev/unity-mcp/issues/1299)).
- **Contexto** ([#1482](https://github.com/CoplayDev/unity-mcp/issues/1482)): a lista padrão de ferramentas custa cerca de 16,7 mil tokens.
- **Janela "Reload scene"** ([#1411](https://github.com/CoplayDev/unity-mcp/issues/1411)): o popup do Unity bloqueia o agente.

Ninguém reclama da telemetria nas issues; a única issue que a cita ([#654](https://github.com/CoplayDev/unity-mcp/issues/654), fechada) é um defeito em que o despachante caía e a telemetria seguia.

## 4. Recomendação

1. **Plugin oficial da Unity** (gratuito), junto com a instalação do Unity 6.6, no escopo do projeto (`--scope local`), não no global. Cobre a CLI, a criação de projeto e os pacotes, que a Story 1.1a usa primeiro. Commit antes de cada tarefa, como pede o README.
2. **Para controlar o Editor aberto: `unity mcp` da CLI da Unity**, primeira escolha. É oficial, gratuito e local; recusar o ping de uso na primeira execução. Fixar a versão da CLI no projeto (`cli.version`), porque cada beta muda comandos. Preferir comandos da CLI (`unity test`, `unity doctor`) e ligar o MCP só quando o agente precisar mexer na cena aberta. Não há comparação independente de estabilidade entre a CLI e o MCP for Unity: a escolha é de arquitetura (oficial, sem segunda ponte), não resultado de teste.
3. **Alternativa: MCP for Unity (CoplayDev)**, se a CLI da Unity, ainda experimental, não der conta. Fixar a versão (tag), auditar antes, como as ferramentas do PRKing_REA, **desligar a telemetria nos dois processos** (`DISABLE_TELEMETRY=1` no servidor e no Editor, ou a chave das EditorPrefs) e cortar as ferramentas que não se usam. Esperar os defeitos de domain reload e de Editor sem foco. Uma ponte de escrita por tarefa: não deixar duas mexendo na mesma cena.
4. **Evitar por ora** o Unity-MCP do IvanMurzak: nuvem por padrão, execução de C# arbitrário e incompatível com o caminho atual.
5. **Codex:** o plugin oficial tem instalação documentada no README da Unity; dá para usar o Codex em paralelo com as mesmas skills.

Antes de adotar, seguir o roteiro de validação prática da [verificação](2026-11-02-integracao-ia-unity-verificacao.md) (§5): caminho com espaços, reconexão após recompilar, Play Mode, testes e build sem a ponte.

Decisões do responsável antes de instalar: aceitar a CLI da Unity em beta como primeira escolha e, se for preciso o MCP for Unity, autorizar a instalação com a telemetria desligada.
