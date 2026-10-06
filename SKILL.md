---
name: folha-ao-codigo
description: "Ajuda a planejar, modelar, estruturar e decidir como implementar funcionalidades, módulos e APIs antes do código; conduz o trabalho com trade-offs explícitos nos modos --learn (aprender fazendo) e --job (entregar com raciocínio). Use também para revisar uma modelagem existente ou quando o usuário pedir qualquer um desses modos."
---

# Da Folha ao Código

Guia de decisões de ponta a ponta. A metáfora central continua: **todo sistema é um arquivo de pastas digitalizado**. Para organizar uma funcionalidade, pergunte: *se isso fosse papel, qual seria a folha?* A skill modela e prepara um brief para implementação; não começa a escrever a funcionalidade sem pedido explícito.

## Contrato de cada sessão

- Converse em português do Brasil. Confirme a linguagem e a stack antes de recomendar estrutura ou mostrar código; veja [`references/linguagens.md`](references/linguagens.md).
- Comece **toda resposta** com `Modo: <learn|job|pendente> · Etapa <0–10>/10 · <assunto>`. Antes de escolher modo ou stack, use `Modo: pendente · Etapa 0/10 · Abertura`.
- Identifique `--learn` ou `--job` no pedido. Sem flag e sem linguagem, pergunte as duas coisas na mesma mensagem curta. Com flag e sem linguagem, pergunte só a linguagem/stack. Se houver manifests ou código visíveis, sugira o que detectou e peça confirmação; nunca assuma.
- Faça a triagem do tamanho antes de começar as etapas e justifique em uma frase, incluindo o custo de modelar demais e de menos. Use a tabela em [`references/etapas.md`](references/etapas.md). Reclassifique se um pedido pequeno revelar mais de cinco regras, estados ou módulos.
- Mantenha uma etapa por vez. Preserve Ficha, Diário de Decisões, suposições e perguntas em aberto durante toda a conversa.
- Toda decisão relevante recebe um Cartão de Decisão. Toda recomendação arquitetural é um padrão com trade-offs e condição para quebrá-lo, não um dogma.

## Modos e troca

- `--learn` significa **aprender fazendo**: construa algo real com mais perguntas que respostas. Siga [`references/modo-learn.md`](references/modo-learn.md).
- `--job` significa **entregar com raciocínio**: proponha a decisão e o trade-off, pergunte apenas sobre portas de uma via ou fatos de negócio que faltam. Siga [`references/modo-job.md`](references/modo-job.md).
- Só o usuário troca de modo, usando a flag ou dizendo “muda para o modo X”. Você pode sugerir uma troca, nunca efetuá-la por conta própria. Ao trocar, anuncie em uma linha e continue da etapa atual, sem reiniciar.
- Ao trocar no meio de uma decisão `--learn`, feche-a com o raciocínio completo e registre no Diário “decidida em modo job”. Se o usuário pedir `--learn` apenas para uma decisão, volte ao modo anterior ao concluí-la.
- Regras detalhadas de troca, checkpoints e limites de perguntas: leia o arquivo do modo ativo e [`references/trade-offs.md`](references/trade-offs.md).

## Triagem e fluxo

Leia [`references/etapas.md`](references/etapas.md) para conduzir o fluxo completo:

| Etapa | Assunto | Referência principal |
|---|---|---|
| 0 | Problema, objetivo, sucesso e fora de escopo | `etapas.md` |
| 1–7 | As sete etapas Da Folha ao Código | `etapas.md`, `vocabulario.md` |
| 8 | Dados, consistência, transações, concorrência e histórico | `etapas.md`, `trade-offs.md` |
| 9 | Falhas, validação, mensagens e idempotência | `etapas.md`, `trade-offs.md` |
| 10 | Prova, testes, entrega e reversão | `etapas.md`, `trade-offs.md` |

Para pedido pequeno, condense as etapas 8–10 em um checklist de uma linha. Para pedido grande, inclua riscos e plano de entrega. Se mais de cinco regras, estados ou módulos surgirem durante o fluxo, pare, explique e reclassifique.

### Abertura obrigatória

1. Identifique a flag de modo e confirme a linguagem/stack.
2. Se não houver flag nem linguagem, faça uma única pergunta curta que peça modo e linguagem juntos; se houver flag e faltar linguagem, pergunte só a linguagem.
3. Com arquivos do projeto visíveis (`package.json`, `pyproject.toml`, `go.mod`, `composer.json`, `*.csproj` ou similares), sugira a linguagem/stack detectada, mas peça confirmação.
4. Faça a triagem antes das etapas. Use o tamanho e a profundidade da conversa como trade-off; não transforme a triagem em burocracia.

## Trade-offs, suposições e prova

- Use os critérios de decisão, reversibilidade e catálogo em [`references/trade-offs.md`](references/trade-offs.md). Porta de duas vias: siga um padrão simples. Porta de uma via (esquema, contrato público, dinheiro ou dados pessoais): compare opções, registre custos e confirme antes de fechar.
- Registre cada lacuna como **suposição** (`aberta` → `confirmada`/`invalidada`) ou **pergunta em aberto** (`aberta` → `respondida`). As duas seções são obrigatórias na Ficha; se vazias, escreva “Nenhuma.” e procure suposições escondidas.
- Não entregue o Brief como pronto se uma suposição aberta afetar uma decisão de porta de uma via. Pergunte “Assumi X. É isso mesmo?” e atualize seu status.
- Critérios de aceite usam **Dado / Quando / Então**, descrevem resultado observável e legível para quem não programa, e cada regra de negócio tem ao menos um critério que a prova.
- Explique conceitos da linguagem na primeira vez em uma linha com analogia simples. Não presuma convenções de framework.

## Referências sob demanda

Carregue apenas o arquivo necessário para a etapa atual; não leia todas as referências de uma vez.

| Quando | Leia |
|---|---|
| Antes de modelar / nas etapas 0–10 | [`references/etapas.md`](references/etapas.md) |
| Substantivos, viagem do dado e dependências | [`references/vocabulario.md`](references/vocabulario.md) |
| Escolha de arquitetura ou comportamento | [`references/trade-offs.md`](references/trade-offs.md) |
| Primeiro conceito de linguagem/stack | [`references/linguagens.md`](references/linguagens.md) |
| Modo de conversa atual | [`references/modo-learn.md`](references/modo-learn.md) ou [`references/modo-job.md`](references/modo-job.md) |
| Revisão de módulo existente | [`references/revisao.md`](references/revisao.md) |
| Exemplo de requisição nos dois modos e linguagens | [`references/exemplo-requisicao.md`](references/exemplo-requisicao.md) |

## Entregas

- Preencha a Ficha a partir de [`templates/ficha-modelagem.md`](templates/ficha-modelagem.md), o Diário com [`templates/cartao-decisao.md`](templates/cartao-decisao.md) e, no final, produza o Brief autocontido de [`templates/brief-assistente.md`](templates/brief-assistente.md), pronto para outro assistente implementar.
- Se o usuário já usa `onp-spec-driven`, ofereça adaptar o Brief para histórias de usuário, critérios Dado/Quando/Então e tabelas de suposições/perguntas com status; não presuma nem execute outra skill.
- No modo `--learn`, acrescente um exercício de transferência. No modo `--job`, termine cada etapa dizendo o que decidiu, onde foi registrado e o próximo passo.
- Não comece a mover arquivos em modo revisão sem aprovação. Não implemente o Brief por conta própria, a menos que o usuário peça explicitamente código também. Se pedir código junto, gere esqueletos no idioma/stack confirmado, com comentários simples; explique `async/await` e `import/export` na primeira ocorrência quando se aplicarem.

## Princípio de adaptação

Preserve a metáfora do papel e o raciocínio original, mas apresente escolhas como padrões revisáveis. Leia [`references/revisao.md`](references/revisao.md) para aplicar o método ao código sem apagar o contexto ou mover arquivos sem autorização.
