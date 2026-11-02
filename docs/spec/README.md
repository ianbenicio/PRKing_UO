# Especificação entregue — registro de entrega e pergunta de volta

`especificacao.md`, nesta pasta, é a cópia da especificação de comportamento em sala limpa escrita no PRKing_REA (alvo `uo-classic`). O projeto implementa **esta cópia**, não o arquivo vivo do PRKing_REA, e registra aqui qual versão implementa.

## Versão implementada

| Campo | Valor |
|---|---|
| Origem | `PRKing_REA@e5706ff7cf2912e454e4012c5ae4e3444ffb6c61`, arquivo `registros/uo-classic/especificacao.md` |
| Entregue em | 2026-11-02 |
| SHA-256 do arquivo de origem | `806a8d5d1cc7073ed8f6e7e7e13952cfcff92653d7459d33517ac912772bccbd` |
| Cabeçalho acrescentado na cópia | três linhas no topo do YAML: `origem`, `entregue_em` e `sha256_origem` |

Conferência: tirando as linhas 2 a 4 da cópia, o SHA-256 do resto é igual ao da origem. A conta vale para o conteúdo guardado no git (fim de linha LF), não para a cópia de trabalho, que o `core.autocrlf` pode converter. Exemplo em Git Bash: `git show HEAD:docs/spec/especificacao.md | sed '2,4d' | sha256sum`.

## Regras de uso (sala limpa)

- Sobre o jogo de referência, a especificação é a única fonte de regras, fórmulas, formatos de arquivo e escopo.
- O projeto nunca lê `alvos/`, `saidas/`, evidência, pseudocódigo nem os arquivos de pesquisa do PRKing_REA, mesmo que a especificação os cite como fonte. As citações (`[Mn]`, `01-analise.md, pergunta N` etc.) servem só para rastrear de onde veio cada regra.
- O projeto não abre o código de clientes e emuladores de código aberto (ClassicUO, RunUO, ModernUO, ServUO, MobileUO, UO Offline e outros) ao escrever o seu.
- Nomes de skills, magias e reagentes na especificação são só rótulos. O jogo usa nomes, textos e ícones próprios e o nome "Outrora".
- Nada lido da instalação do jogador vai para o git, para `Assets/`, para o build, para o save ou para capturas versionadas. O `.gitignore` da raiz bloqueia os formatos de dados do jogo.
- Não se edita a cópia. Mudança chega como adendo numa nova entrega.

## Histórico de entregas

| Data | Origem (commit do PRKing_REA) | O que trouxe | Commit no PRKing_UO |
|---|---|---|---|
| 2026-11-02 | `fd30f89` | Primeira entrega: seções 1–18, adendos até 2026-11-01 (D1–D11 e reavaliação legal) e Anexo A com a resolução de 2026-11-02 (A.4 e A.5) | `20f5e8c` |
| 2026-11-02 | `5d11c0d` | Adendo A.6: decisões do responsável sobre A2, A7, A18 e A26; Anexo A resolvido; seção 2 marcada com o passo de 25 ms | `4cbf739` |
| 2026-11-02 | `e5706ff` | Respostas às perguntas de volta 1 a 4: D12 na seção 16 e adendos na §3.0 (regra 1) e na §15 (pergunta 1); adendo corrigido na §5.5 sobre atrofia e ganho no cap (pergunta 2); adendo na §6.1 sobre alcance de contato e timer do golpe, com nota na §13.1 (pergunta 3); adendo corrigido na §15 sobre o estado do gerador de sorteio no save (pergunta 4); nova convenção de referência `[Vn]` | commit de entrega desta versão |

## Decisões do responsável

O Anexo A está resolvido (especificação, A.6, 2026-11-02): a Story 1.2 saiu do repositório (A2); os arquivos do PRD e do dossiê ganharam nomes neutros, e a pasta e o repositório serão renomeados antes de o repositório ficar público (A7); passo de 25 ms para timers de ação e IA, com sistemas pesados em fatias de 10 Hz (A18); todos os direitos reservados enquanto local, com a licença decidida antes de publicar (A26).

Seguem em aberto: a D3 (resistência a magia), adiada até a magia, e a renomeação da pasta e do repositório, com prazo antes da publicação.

## Pergunta de volta (como pedir um adendo)

Quando a especificação não responde a uma pergunta do projeto:

1. Registre a pergunta na tabela abaixo, com a seção da especificação e a decisão provisória. Enquanto espera, decida por conta própria e documente a decisão na story ou num ADR, ou deixe o ponto parado. Nunca vá ao jogo, à evidência do PRKing_REA, à pesquisa ou ao código de terceiros para responder.
2. Leve a pergunta ao responsável. Ele abre, no PRKing_REA, uma sessão própria do alvo `uo-classic`, onde a pergunta é respondida dentro dos níveis aprovados (análise) ou por pesquisa pública, conforme o protocolo de especificação daquele repositório.
3. A resposta volta como adendo datado na especificação, com fonte, num commit próprio do PRKing_REA.
4. O projeto recebe a nova cópia presa ao novo hash, acrescenta uma linha ao histórico de entregas e marca a pergunta como respondida, sem apagar a linha.

| # | Data | Seção | Pergunta | Decisão provisória | Situação |
|---|---|---|---|---|---|
| 1 | 2026-11-02 | §3.0 (regra 1), §15, D8 | Trocar a origem do conteúdo ("próprio" ↔ "instalação") vale só para arte e IDs, ou também para mundos com geometria diferente? Se vale para geometria, o que acontece com posições, regiões, spawns e containers que não existem no outro mapa? (revisão técnica, R06 e Q01) | Até a resposta, o save fica preso ao mundo em que foi criado (origem e revisão da geometria); carregar com outra geometria é recusado com mensagem. O sandbox e a primeira região usam só a origem "próprio". Não bloqueia a Fase 1 | respondida em 2026-11-02 (`PRKing_REA@034e52f`): decisão D12 do responsável. O save guarda a identidade do mundo e a revisão da geometria e só carrega no mesmo mundo; levar personagem e inventário para outro mundo é PODE, fora da primeira versão; migrar posições, spawns e containers fica FORA (seção 16, §3.0 regra 1, §15). A decisão provisória fica confirmada |
| 2 | 2026-11-02 | §5.4, §5.5 | No ganho com o total no cap, a atrofia de um skill em "desce" acontece antes da checagem `T + ganho ≤ c_T` (abrindo espaço) ou só depois? Como se escolhe o skill que desce e o que acontece se nenhum puder descer ou se só parte do ganho couber? (Q02) | Ponto parado: a 1.1b usa skills fixas e o ganho entra na Fase 2 | respondida em 2026-11-02 (`PRKing_REA@5e92447`): a atrofia vem antes da checagem do cap e só com o total no cap; desce o primeiro skill em "desce", na ordem fixa da lista, com pelo menos o valor do ganho, que perde exatamente esse valor; sem skill elegível, nada muda; não há ganho parcial (§5.5, adendo corrigido) |
| 3 | 2026-11-02 | §6.1, §13.1 | Qual é o alcance de contato do golpe corpo a corpo (distinto dos 15 tiles para iniciar o ataque) e quando ele é conferido: ao iniciar, a cada golpe ou no impacto? (Q03) | Na 1.1b: contato a 1 tile em qualquer das oito direções (coerente com "persegue até 1 tile" da §13.1), conferido no momento do golpe; fora do alcance, o golpe espera e o timer continua. Valor em dados | respondida em 2026-11-02 (`PRKing_REA@a31add6`): alcance por arma em dados, 1 tile para corpo a corpo (oito direções); conferido a cada tentativa de golpe; sem checagem no impacto; fora do alcance, o golpe fica pronto e espera, sem acumular (§6.1, adendo; nota na §13.1). A decisão provisória fica confirmada, com o detalhe do não acúmulo |
| 4 | 2026-11-02 | §15 | O DEVE "semente do sorteio" no save se cumpre guardando o estado atual do gerador? Guardar só a semente faz o load repetir o início da sequência (revisão técnica, R01) | A arquitetura (§12) guarda o estado atual do gerador, que inclui a continuação da sequência | respondida em 2026-11-02 (`PRKing_REA@e5706ff`): o save guarda o estado atual do gerador, não só a semente, e a versão do gerador das regras (§15, adendo corrigido). A decisão provisória fica confirmada |
