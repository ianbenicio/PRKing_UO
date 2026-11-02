# Especificação entregue — registro de entrega e pergunta de volta

`especificacao.md`, nesta pasta, é a cópia da especificação de comportamento em sala limpa escrita no PRKing_REA (alvo `uo-classic`). O projeto implementa **esta cópia**, não o arquivo vivo do PRKing_REA, e registra aqui qual versão implementa.

## Versão implementada

| Campo | Valor |
|---|---|
| Origem | `PRKing_REA@5d11c0d64b69d5c0a5801ed5e4e6766b8ce71c05`, arquivo `registros/uo-classic/especificacao.md` |
| Entregue em | 2026-11-02 |
| SHA-256 do arquivo de origem | `7711e74f04b261b47c85c74f83964fb38453c7ba66759bb67d1ff7f6df24183a` |
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
| 2026-11-02 | `5d11c0d` | Adendo A.6: decisões do responsável sobre A2, A7, A18 e A26; Anexo A resolvido; seção 2 marcada com o passo de 25 ms | commit de entrega desta versão |

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
| — | — | — | nenhuma pergunta aberta | — | — |
