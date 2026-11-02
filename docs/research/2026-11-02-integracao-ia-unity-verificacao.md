# Integração de IA com Unity: custos, telemetria e experiência de uso

Data da verificação: **02/11/2026**. Projeto: **Outrora**.

Escopo: conferir as duas dúvidas do pedido anexado — assinatura do plugin oficial e telemetria da CoplayDev — e confrontar a recomendação inicial com relatos de desenvolvedores. Esta verificação complementa e corrige o [levantamento inicial](2026-11-02-integracao-ia-unity.md).

Método: documentação e anúncios oficiais para condições atuais; leitura estática de arquivos da tag **v10.3.0** da CoplayDev para comportamento de telemetria; Unity Discussions, issues e relatos de primeira mão para experiência de uso. Não houve instalação, execução dos pacotes nem captura de tráfego. Compatibilidade com o Editor **6000.6.7f1** e com o caminho deste projeto permanece por validar em execução.

## 1. Respostas verificadas

| Dúvida | Conclusão | Evidência |
|---|---|---|
| O plugin oficial para Claude Code exige assinatura adicional da Unity? | **Não.** O plugin é gratuito. O custo do agente utilizado continua separado. | [FAQ atual da Unity](https://unity.com/features/ai) |
| O MCP oficial ainda depende da assinatura antiga do Assistant? | **O caminho atual é gratuito via Unity CLI.** A implementação antiga dentro do Assistant foi descontinuada; o controle do Editor usa o pacote Unity Pipeline. | [Migração oficial](https://docs.unity.com/en-us/unity-cli/replace-mcp-server-unity-cli) |
| O plugin oficial existe também para Codex? | **Sim, confirmado em fonte primária.** O repositório oficial lista os agentes suportados. | [Unity-Technologies/unity-agent-plugin](https://github.com/Unity-Technologies/unity-agent-plugin) |
| A CoplayDev coleta telemetria? | **Sim: habilitada por padrão na v10.3.0**, salvo configuração de desligamento. | [Configuração da versão inspecionada](https://raw.githubusercontent.com/CoplayDev/unity-mcp/v10.3.0/Server/src/core/config.py) |
| É possível desligá-la? | **Sim.** Há opt-out por variável de ambiente e controle no lado do Editor. A configuração deve alcançar os processos corretos. | [Servidor Python](https://raw.githubusercontent.com/CoplayDev/unity-mcp/v10.3.0/Server/src/core/telemetry.py), [ponte C#](https://raw.githubusercontent.com/CoplayDev/unity-mcp/v10.3.0/MCPForUnity/Editor/Helpers/TelemetryHelper.cs) |

A cobrança do assistente da Unity dentro do Editor é uma oferta distinta. O FAQ atual distingue explicitamente esse produto do plugin, da CLI e do MCP gratuitos. A comparação anterior que condicionava o MCP oficial atual a uma assinatura adicional ficou superada. A gratuidade da integração não altera as condições da licença do Editor ou do provedor de IA. [Fonte: FAQ da Unity](https://unity.com/features/ai).

## 2. O que o código da CoplayDev mostra

Na tag v10.3.0, `telemetry_enabled` começa como `True`, e o destino configurado é `https://api-prod.coplay.dev/telemetry/events`. [Fonte: config.py](https://raw.githubusercontent.com/CoplayDev/unity-mcp/v10.3.0/Server/src/core/config.py).

O módulo de telemetria mantém um UUID de instalação e identificador de sessão. Os eventos incluem plataforma e versões, uso de ferramentas, sucesso, duração e mensagens de erro truncadas. O código inspecionado não demonstra upload integral do projeto por esse mecanismo; mensagens livres de erro, porém, podem conter informações contextuais. Isso não equivale a uma auditoria de todos os dados que o produto pode transmitir. [Fonte: telemetry.py](https://raw.githubusercontent.com/CoplayDev/unity-mcp/v10.3.0/Server/src/core/telemetry.py).

Para um eventual uso, o valor comum reconhecido pelo servidor e pela ponte do Editor é:

```text
UNITY_MCP_DISABLE_TELEMETRY=1
```

No cliente MCP, um bloco `env` só alcança o processo que ele inicia. Se o servidor HTTP for iniciado separadamente, a variável precisa estar no ambiente desse servidor. O servidor documenta as opções de opt-out. [Fonte: README do servidor](https://raw.githubusercontent.com/CoplayDev/unity-mcp/v10.3.0/Server/README.md).

A ponte C# também verifica essa variável; sua preferência de telemetria no Editor começa habilitada. Portanto, a configuração precisa alcançar também o Editor, ou usar o controle equivalente em `EditorPrefs`. Não se deve supor que configurar apenas o cliente altere um Editor já aberto. [Fonte: TelemetryHelper.cs](https://raw.githubusercontent.com/CoplayDev/unity-mcp/v10.3.0/MCPForUnity/Editor/Helpers/TelemetryHelper.cs).

Há uma divergência documental: o site público anuncia ausência de telemetria por padrão, enquanto a configuração da tag inspecionada a habilita. Para decidir sobre a v10.3.0, prevalece a evidência do código dessa versão. [Site público](https://coplaydev.github.io/unity-mcp/), [configuração v10.3.0](https://raw.githubusercontent.com/CoplayDev/unity-mcp/v10.3.0/Server/src/core/config.py).

O caminho oficial também possui controles de analytics: suas notas documentam `unity analytics status` e `unity analytics opt-out`. Ser uma integração oficial ou operar com uma ponte local não comprova ausência de toda comunicação externa. [Fonte: notas da Unity CLI](https://docs.unity.com/en-us/unity-cli/release-notes).

## 3. O que os desenvolvedores relatam

| Origem e contexto | Relato observado | Como interpretar para o Outrora |
|---|---|---|
| [Unity Discussions: lançamento do plugin](https://discussions.unity.com/t/unity-s-plugin-is-now-available-in-claude-code/1736143), setembro de 2026 | Desenvolvedores perguntam sobre sobreposição com skills instaladas manualmente e suporte a outros agentes. A equipe esclarece a combinação de skills com CLI e confirma Codex. | O plugin organiza instruções e ferramentas; sua origem oficial não garante resultados corretos de gameplay ou de cena. |
| [Unity Discussions: MCP sem ferramentas](https://discussions.unity.com/t/cli-unity-mcp-no-tool-available/1739074), 13/10/2026, CLI beta.12/13 e Pipeline 0.8.0-exp.1 | Usuário relata conexão sem ferramentas e dificuldades ao gerar um botão. A equipe participa do diagnóstico e esclarece a migração do MCP. | Há atrito real de configuração. O cliente era Mistral Vibe e o resultado também depende do modelo: não é uma comparação controlada com Claude/Codex ou CoplayDev. |
| [Unity Discussions: CLI beta.15](https://discussions.unity.com/t/unity-cli-1-0-0-beta-15-is-rolling-out/1740651), 29/10/2026 | Anúncio oficial inclui mudanças incompatíveis nos comandos e no formato das respostas, além de fixação de versão por projeto. | Ferramenta ainda evolui. Fixar CLI e Pipeline e registrar versões antes de automatizar tarefas repetíveis. |
| [r/Unity3D: cobrança do antigo MCP](https://www.reddit.com/r/Unity3D/comments/1t4zlyp/they_made_using_the_official_mcp_server_a_paid/) | Há críticas à cobrança antiga e usuários relatando uso satisfatório da CoplayDev. | Evidência de adoção e satisfação individual. A discussão antiga não define o custo do caminho oficial atual. |
| [CoplayDev issue #1238](https://github.com/CoplayDev/unity-mcp/issues/1238), v10.0.0, Windows 11 e Unity 6000.3.9f1 | Desenvolvedor documenta congelamentos ao focar a janela MCP, associados a descoberta síncrona de executáveis e sondagens de conexão; Claude CLI não estava no PATH. | Relevante para nosso Windows. Validar PATH e responsividade; não presumir que o mesmo defeito persista na v10.3.0. |
| [Relato de desenvolvimento com Claude Code e Unity](https://www.reddit.com/r/ClaudeAI/comments/1s7mfil/i_built_a_steam_game_in_10_days_with_claude_code/) | Autor relata progresso até revisão técnica da Steam, mas também horas de configuração no Windows, limites de uso e regressões durante correções. | Mostra potencial e custo operacional de agentes. O relato não permite atribuir o resultado a uma das duas integrações em particular. |

São relatos e registros técnicos, não uma amostra estatística. Não foi encontrada, nas fontes consultadas, uma comparação independente suficientemente controlada para afirmar que o caminho oficial é mais estável, ou que a CoplayDev produz jogos melhores. Estrelas, número de ferramentas e demonstrações isoladas não resolvem essa comparação.

## 4. Recomendação técnica para o Outrora

**Começar pelo plugin oficial do agente escolhido e Unity CLI/Pipeline.** É uma recomendação de arquitetura: o caminho gratuito atual atende ao objetivo de comandar o Editor e reduz a necessidade inicial de uma segunda ponte. Essa preferência não é uma conclusão de benchmark.

**Manter CoplayDev como alternativa**, se uma necessidade concreta não for atendida ou a validação local favorecer suas ferramentas. Nesse caso, fixar a tag, desligar telemetria nos processos envolvidos e validar o fluxo. O projeto publica seu MCP sob MIT; o plugin oficial tem repositório público sob Unity Companion License. Não confundir o MCP gratuito da CoplayDev com seu assistente comercial. [CoplayDev](https://github.com/CoplayDev/unity-mcp), [licença do plugin oficial](https://github.com/Unity-Technologies/unity-agent-plugin/blob/main/LICENSE.md).

Para este projeto, a operação offline do jogo permanece distinta do ambiente de desenvolvimento. Um agente baseado em nuvem continua recebendo o conteúdo que lhe for enviado, mesmo que a conexão entre ferramenta e Editor use localhost. O pacote de integração deve ficar restrito ao Editor e não criar dependência de rede no build do jogo.

Não há benefício demonstrado em deixar duas pontes modificando a mesma cena simultaneamente. Recomenda-se um único responsável por escrita em cada tarefa, com revisão das alterações em arquivos e no Editor. A documentação registra ainda conflito do Assistant com a CLI corrigido a partir do Assistant 2.13. [Fonte: migração oficial](https://docs.unity.com/en-us/unity-cli/replace-mcp-server-unity-cli).

## 5. Validação prática pendente

Antes de adotar uma integração na Story 1.1a:

1. Registrar versões de Editor, plugin, CLI e pacote da ponte, incluindo a combinação exata com Unity 6000.6.7f1.
2. Validar conexão no caminho real do projeto, que contém espaços; ler hierarquia e console.
3. Criar um objeto e componente simples, compilar e verificar o comportamento em Play Mode.
4. Salvar a cena, fechar e reabrir; confirmar referências e mudanças persistidas.
5. Executar os testes do projeto e verificar reconexão após recompilação de scripts.
6. Se usar CoplayDev, confirmar opt-out no servidor e no Editor; observar o tráfego para verificar seu efeito na versão instalada.
7. Confirmar que a integração não entra no build Windows nem acrescenta requisito de conexão ao jogo.

Esta entrega é pesquisa documental e inspeção estática. As etapas acima não foram executadas e não constituem resultados de testes.
