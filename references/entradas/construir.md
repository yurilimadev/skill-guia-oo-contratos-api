# Entrada Construir — etapas 0 a 10

Leia com o vocabulário e, quando a decisão tocar em arquitetura, com `dominio.md` e `trade-offs.md`. Preserve a numeração: etapas 0–10. Faça uma por vez; triagem e modos estão em `SKILL.md` e `references/modo-*.md`.

| Etapa | Nome | Pergunta central | Produz | Chapéus típicos e por quê |
|---|---|---|---|---|
| 0 | Problema | Qual objetivo, para quem, como saber que deu certo e o que fica fora? | objetivo, sucesso observável, fora de escopo | Produto e Dono do processo: confirmar valor e medida |
| 1 | História em papel | Como funciona sem sistema; quem preenche o quê e o que ocorre depois? | processo e glossário inicial | Dono do processo e Usuário-chave: retratar trabalho e exceções reais |
| 2 | Caneta × papel | Quem escreve? O que ou quem está escrito? | atores, permissões, assuntos e contextos | Produto e Dono do processo: delimitar atores e autoridade |
| 3 | Campos da folha | Referência, dado, assinatura, estado? Tem identidade própria? | tipo, cardinalidade e mapa de domínio | Domínio e Dados: evitar identidade, cardinalidade e histórico mal colocados |
| 4 | Estados | Por quais situações passa o compromisso? O que muda cada uma? | estados, transições e eventos | Domínio e Dono do processo: validar ciclo de vida real |
| 5 | Regras | O que é invariante ou política? Qual folha protege a regra? | regras, donos, critérios e teste de estresse | Domínio, Dono do processo e QA: validar regra e como prová-la |
| 6 | Contrato | O que entra e o que precisa voltar? | recurso, endpoint, DTOs | Backend e Frontend/UX: alinhar fronteira e experiência |
| 7 | Ação | Qual sequência cumpre o pedido? Altera um ou vários substantivos? | caso de uso, passos e responsável | Backend e Domínio: definir coordenação e efeitos |
| 8 | Dados e consistência | O que precisa acontecer junto? E se duas pessoas agirem ao mesmo tempo? | agregados, transações, concorrência e histórico | Dados, Infra e Segurança quando aplicável: proteger integridade e acesso |
| 9 | Falhas e bordas | O que falha? E se o pedido chegar duas vezes? | erros, validação, idempotência e recuperação | QA, Backend e Suporte: cobrir bordas e mensagens reais |
| 10 | Prova e entrega | Como provar, entregar, desfazer e conferir sucesso em produção? | aceite, testes, plano de entrega/rollback e retorno | QA, Infra e Produto: provar, publicar e acompanhar resultado |

## Triagem

| Tamanho | Exemplo | Profundidade |
|---|---|---|
| Pequeno | campo, ajuste de tela ou CRUD direto | condensar; etapas 8–10 em checklist de até uma linha; teste de estresse com no máximo 2 cenários |
| Médio | regra e estados | fluxo completo |
| Grande | módulos, integrações, dinheiro ou dados pessoais | fluxo completo, chapéus, riscos e plano de entrega |

Justifique em uma frase o custo de modelar demais e o de modelar de menos. Se um caso pequeno revelar mais de cinco regras, estados ou módulos, pare e reclassifique. Pedido pequeno não vira reunião: normalmente envolve 1 ou 2 chapéus.

## Condução e lente de domínio

Em cada etapa, identifique chapéus típicos e por quê (`references/chapeus.md`); mantenha Ficha, cartões, suposições, perguntas e glossário. Leia `references/dominio.md` nos pontos abaixo:

### Etapa 0 — problema

Escreva quem tem o problema, objetivo, resultado observável e fora de escopo. Critérios de sucesso serão verificados depois em produção, com método e momento de retorno definidos na etapa 10.

### Etapa 1 — história em papel

Entenda o processo real, não a tela imaginada. Se história e participantes já estiverem claros, confirme e avance sem repetir discovery. Comece o glossário: quais palavras todos usam do mesmo jeito? Indique Dono do processo, Usuário-chave e Produto como chapéus típicos.

### Etapa 2 — caneta × papel

Quem segura a caneta vira ator/permissão; o que fica escrito é dado; assunto com ficha própria vira referência. Compare pessoa e papel e perceba se palavras mudam de sentido entre setores. **O ator humano não vira automaticamente uma classe**: a pessoa inicia ações no processo real; identifique separadamente qual responsabilidade o software precisa assumir. Não invente quem aprova: mapeie chapéus e pergunte quem os veste.

### Etapa 3 — campos da folha

Classifique campo a campo:
- **referência**: aponta para outra ficha, muitas vezes `...Id`;
- **dado simples/valor**: pertence a esta folha e pode precisar ser copiado para preservar o passado;
- **assinatura**: quem agiu; quando é identidade autenticada, vem da sessão;
- **estado**: situação atual, se houver ciclo de vida.

Pergunte identidade × valor, cardinalidade (um ou vários?), cópia × referência e unidade de medida. Gere candidatos a objeto a partir de necessidades, responsabilidades, dados e interações; não crie uma classe por substantivo nem por linha de tabela. Um objeto persistido, uma estrutura de valor e um coordenador transitório cumprem papéis diferentes. O que tem vida própria pode virar módulo; item que só existe no registro-pai pode ficar junto. Essas são escolhas com trade-off, não regras automáticas. Leia `references/objetos.md`. Indique Domínio, Dono do processo e Dados como chapéus usuais.

### Etapa 4 — estados

Estados descrevem compromissos que mudam; fatos já ocorridos não são desfeitos por trocar um status. Cada transição corresponde a ação/evento. Investigue se muitos booleanos ou valores de status podem se tornar um ciclo de vida ou entidade própria; não adicione um estado a todo substantivo.

### Etapa 5 — regras e teste de estresse do domínio

Separe **invariante** (condição que protege a integridade do modelo) de **política** (regra que o negócio pode ajustar). Pergunte qual raiz/folha precisa proteger cada invariante, quem pode alterar a política e como provar cada regra. Regra compartilhada pode ter outro dono; registre a justificativa.

Antes de seguir para o contrato, teste o modelo contra **3–5 mudanças plausíveis**, tiradas de `references/dominio.md`: ele aguenta? Quanto custa mudar depois? Em porta de uma via, resolva agora; em porta de duas vias, registre suposição e gatilho de revisão. Para pedido pequeno, use no máximo 2 cenários. `--learn` começa perguntando “que mudança no negócio em um ano quebraria este modelo?”; `--job` começa com uma tabela de cenários e recomendação.

### Etapa 6 — contrato

Defina recurso (substantivo na URL), verbo HTTP, endpoint, DTO de entrada e saída. DTO separado do tipo interno é um padrão, não dogma. Ação de negócio com regra/efeito costuma usar sub-recurso POST (por exemplo, `POST /requisicoes/123/atendimento`) em vez de alteração cega de status; `PATCH` é razoável para edição parcial simples. Campos de servidor (`id`, estado controlado, datas, autor) não vêm do corpo por padrão; se o negócio permitir, declare o limite.

### Etapa 7 — ação

Descreva a sequência que cumpre o pedido. Um método/regra lida com responsabilidade localizada; uma ação/caso de uso coordena vários substantivos e pode ser objeto transitório sem linha de banco. Comece pelo módulo mais alterado, mas mude o dono se isso reduzir acoplamento. Persistência busca e grava; a regra de negócio fica na camada responsável pelo domínio.

Se o fluxo envolver várias pessoas/componentes, ofereça um diagrama Mermaid conforme `references/diagramas.md`; em `--learn`, peça à pessoa para sugerir atores, objetos e setas antes de desenhá-lo.

### Etapa 8 — dados e consistência

Pergunte o que precisa ser consistente ao mesmo tempo (agregado), o risco de concorrência e se é preciso histórico/auditoria. Não adote transação, evento ou lock sem motivo. Investigue preço/custo copiado, quantidade com unidade de medida, referências históricas e a fronteira entre contextos.

### Etapa 9 — falhas e bordas

Considere falha de permissão, dado inválido/ausente, conflito de estado, integração fora do ar e repetição do pedido. Especifique o resultado visível, mensagem, responsável pela validação e recuperação. Não chame falhas que “resolveram com F5” de ruído sem evidência; esse sinal pode alimentar Operar.

### Etapa 10 — prova, entrega e compromisso de retorno

Critério de aceite usa Dado/Quando/Então, é observável e escrito para pessoas; detalhe técnico pode vir entre parênteses. Cada regra da etapa 5 tem pelo menos um critério que a prova. Combine testes, deploy e rollback proporcionais ao risco.

**Compromisso de retorno:** registre na Ficha de Operação quando e como conferir em produção cada critério de sucesso da etapa 0, quem acompanha e que evidência serve. Isto fecha Construir e alimenta a entrada Operar; não significa executar algo em produção.

## Fechamento

Preencha [`../../templates/ficha-modelagem.md`](../../templates/ficha-modelagem.md), Cartões/Diário e Brief. Toda proposta e Brief terminam com pelo menos dois **Pontos fracos desta proposta**, incluindo custo de desfazer, suposições e mudança de negócio provável. Liste pontos em aberto; não declare pronto com suposição aberta numa porta de uma via. Escreva no fim o que documentar e onde, oferecendo-se para criar arquivos apenas com permissão.
