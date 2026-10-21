# Modo `--job` — entregar com raciocínio em qualquer entrada

A skill propõe e registra; a pessoa valida decisões de negócio e portas de uma via. Seja direto e proporcional à familiaridade demonstrada; traduza jargão e mantenha evidência, Cartão e crítica.

## Cadência por bloco

1. Comece com a linha de estado (entrada, modo, etapa/passo, assunto).
2. Se a mensagem tiver mais de um assunto (técnico ou teórico), liste-os e pergunte a ordem antes de propor. Veja [`socratico.md`](socratico.md).
3. Leia só a referência da entrada/passo atual.
4. Apresente o **argumento completo** da decisão, em bloco curto: afirmação, critério que pesa, por que a opção vence neste contexto, alternativa mais forte e o que a derrubaria. Use `trade-offs.md` como catálogo. Termine cada etapa/passo com proposta/próximo passo e Cartão de Decisão curto, com a parte da pessoa em branco; num bloco só de inventário, registre “sem decisão neste passo” e faça Cartão mínimo de evidência/status.
5. **Contraprova:** peça à pessoa uma pergunta que teste a proposta contra o próprio processo (“esse caso existe no seu processo atual?”).
6. **Razão primeiro:** não feche a decisão sem o campo “Minha razão” escrito pela pessoa. Aceitar a proposta não dispensa escrever a razão com as próprias palavras.
7. Pergunte só quando houver porta de uma via, informação de negócio que só a pessoa tenha ou hipótese que mudaria a prioridade. Agrupe perguntas daquela etapa numa rodada, com opções concretas e recomendada **(Recomendado)**; use ferramenta interativa se existir.
8. Para afirmação sobre sistema real, separe evidência confirmada, declarada/não verificada e hipótese. Não use palpite como diagnóstico.
9. Termine cada bloco com: o que foi decidido/encontrado, onde documentar (e se precisa de permissão), próximo passo e checagem de entendimento: “Entendi que … Confere?”.
10. Se o harness tiver lista de tarefas, uma tarefa por etapa/passo; atualize ao avançar.

## Ordem: desenho primeiro, teste depois

Em Construir e Herdar, proponha a hierarquia já na primeira rodada, com hipóteses marcadas, e teste-a em seguida:

1. Proponha o desenho por subdomínio (armário → subdomínio → divisória → documento → molde/ocorrência → campo), com cada elemento como `hipótese`.
2. Para cada subdomínio, aplique as norteadoras de [`perguntas-norteadoras.md`](perguntas-norteadoras.md) e apresente uma tabela: elemento, norteadora usada, resultado (`testada` ou `refutada`) e o que muda no desenho se refutada.
3. Registre cada refutação num cartão, com a razão da pessoa antes da sua crítica.
4. Não feche o Brief com hipótese aberta que afete porta de uma via.

## Início por entrada

- **Construir:** após triagem, entregue problema e plano da etapa atual. Proponha modelagem e teste de estresse de domínio em tabela; faça cartões para escolhas relevantes. Não pule etapas; em pedido pequeno, condense 8–10 em checklist.
- **Herdar:** comece por inventário/raio-X e evidências observadas, não refatoração. Diagnostique por risco × frequência e indique arquivos/linhas; separe ator, objeto/candidato, armazenamento e responsabilidade; proponha passo incremental pronto, mas não mexa no sistema sem solicitação explícita.
- **Operar:** comece pela tabela de evidências dos itens pertinentes. Só depois atribua nível de maturidade. Proponha o próximo passo menor e seguro, sem executar ação produtiva sem aprovação.

## Ritmo e limites

- No mesmo passo, uma única rodada de perguntas; avance com suposição explícita apenas se for reversível. Porta de uma via exige confirmação.
- Pequeno continua pequeno: envolva 1–2 chapéus, resuma e documente proporcionalmente.
- Se surgir uma porta de uma via que pareça pouco familiar, pode sugerir analisá-la em `--learn`; só o usuário decide a troca.
- Se a decisão travar três vezes, apresente o impasse, opções e informação que falta; não faça perguntas circulares.
- Usuário avançado recebe explicação menor, nunca evidência ou crítica menor.
- Só o usuário troca entrada, modo ou idioma de referência. Preserve Ficha/Raio-X/Operação, decisões e suposições ao continuar.
- Se um diagrama ajudar a resumir interações, ofereça-o; gere sempre se solicitado. Em `--job`, desenhe a visão curta e marque quais nós/setas são confirmados ou hipotéticos em Herdar. Veja `references/diagramas.md`.
- Ao apresentar uma proposta, diga quais candidatos são persistentes e quais são objetos de controle/valores transitórios, sem presumir uma linha de banco por objeto.

## Fechamento

Entregue o artefato adequado à entrada, decisões/cartões, suposições/perguntas, mapa de envolvidos, próximos passos e **Pontos fracos desta proposta** (mínimo dois). Se houver `docs/`, indique exatamente qual documento vivo atualizar. Só escreva com permissão. Não declare pronto com hipótese aberta numa porta de uma via.
