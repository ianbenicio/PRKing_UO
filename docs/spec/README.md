# Especificação entregue — registro de entrega e pergunta de volta

`especificacao.md`, nesta pasta, é a cópia da especificação de comportamento em sala limpa escrita no PRKing_REA (alvo `uo-classic`). O projeto implementa **esta cópia**, não o arquivo vivo do PRKing_REA, e registra aqui qual versão implementa.

## Versão implementada

| Campo | Valor |
|---|---|
| Origem | `PRKing_REA@fd30f89c5bf02479c10cfb98b7c5baca3e786200`, arquivo `registros/uo-classic/especificacao.md` |
| Entregue em | 2026-11-02 |
| SHA-256 do arquivo de origem | `4847f3340194631cd404025473af20c224582199637bd2c2d161a7adc9438099` |
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
| 2026-11-02 | `fd30f89` | Primeira entrega: seções 1–18, adendos até 2026-11-01 (D1–D11 e reavaliação legal) e Anexo A com a resolução de 2026-11-02 (A.4 e A.5) | commit de entrega desta pasta |

## Decisões pendentes do responsável

Listadas na especificação, Anexo A, A.5: destino da Story 1.2 (A2), nome da pasta e dos arquivos do repositório (A7), tique da simulação (A18) e licença do Outrora (A26). A D3 (resistência a magia) fica adiada até a magia.

## Pergunta de volta (como pedir um adendo)

Quando a especificação não responde a uma pergunta do projeto:

1. Registre a pergunta na tabela abaixo, com a seção da especificação e a decisão provisória. Enquanto espera, decida por conta própria e documente a decisão na story ou num ADR, ou deixe o ponto parado. Nunca vá ao jogo, à evidência do PRKing_REA, à pesquisa ou ao código de terceiros para responder.
2. Leve a pergunta ao responsável. Ele abre, no PRKing_REA, uma sessão própria do alvo `uo-classic`, onde a pergunta é respondida dentro dos níveis aprovados (análise) ou por pesquisa pública, conforme o protocolo de especificação daquele repositório.
3. A resposta volta como adendo datado na especificação, com fonte, num commit próprio do PRKing_REA.
4. O projeto recebe a nova cópia presa ao novo hash, acrescenta uma linha ao histórico de entregas e marca a pergunta como respondida, sem apagar a linha.

| # | Data | Seção | Pergunta | Decisão provisória | Situação |
|---|---|---|---|---|---|
| — | — | — | nenhuma pergunta aberta | — | — |
