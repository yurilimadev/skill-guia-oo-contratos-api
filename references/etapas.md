# Fluxo Da Folha ao Código — etapas 0 a 10

Use este guia como mapa; converse só sobre a etapa atual. A tabela mostra perguntas e artefatos, não um formulário que precise ser despejado de uma vez. O fluxo principal está na coluna central; a profundidade muda com o tamanho do pedido.

| # | Etapa | Pergunta central | Produz |
|---|---|---|---|
| 0 | Problema | Qual objetivo, para quem, como saber que deu certo e o que fica fora? | Objetivo, critérios de sucesso, fora de escopo |
| 1 | História em papel | Como funcionaria sem sistema? Quem preenche o quê e o que acontece depois? | História e cenário |
| 2 | Caneta × papel | Quem escreve? O que ou quem está escrito? | Permissões e assuntos |
| 3 | Campos da folha | Quais campos há? São referência, dado simples, assinatura ou estado? O que tem vida própria? | Tipo/modelo do objeto |
| 4 | Estados | Por quais situações passa um compromisso? O que provoca cada mudança? | Estados e ações de transição |
| 5 | Regras | O que é proibido ou obrigatório? Qual folha é dona da regra? | Regras, responsáveis e critérios de aceite |
| 6 | Contrato | O que a tela envia e o que precisa receber? | Recursos, endpoints e DTOs |
| 7 | Ação | Qual sequência cumpre o pedido? Altera um ou vários substantivos? | Caso de uso, passos e módulo dono |
| 8 | Dados e consistência | O que precisa acontecer junto? O que ocorre com dois usuários simultâneos? | Transação, concorrência, histórico e auditoria necessários |
| 9 | Falhas e bordas | O que pode falhar? E se o pedido chegar duas vezes? | Erros, mensagens, idempotência e validação |
| 10 | Prova e entrega | Como provar que funciona, publicar e desfazer? | Aceite, plano de testes, entrega e reversão |

## Triagem de tamanho

Faça a triagem antes do fluxo e justifique em uma frase. Diga o custo de modelar demais (atraso, decisões especulativas) e de modelar de menos (retrabalho, regra ou risco omitido).

| Tamanho | Exemplos | Profundidade |
|---|---|---|
| Pequeno | campo novo, ajuste de tela, CRUD simples | Etapas condensadas; etapas 8–10 como checklist de uma linha |
| Médio | funcionalidade com regras e estados | Fluxo completo |
| Grande | vários módulos, integrações, dinheiro ou dados sensíveis | Fluxo completo, riscos e plano de entrega |

Se o pedido inicialmente pequeno revelar **mais de cinco regras, estados ou módulos**, pare, explique o achado e reclassifique para médio ou grande. A triagem também é um trade-off: não gaste mais energia planejando do que a mudança exige, nem simplifique a ponto de esconder decisões caras.

## Condução etapa a etapa

### Etapa 0 — problema e sucesso

Registre quem tem o problema, o resultado que precisa obter, a mudança observável que define sucesso e o que está fora de escopo. Critérios de sucesso viram critérios de aceite em Dado/Quando/Então. Evite adjetivos não observáveis (“rápido”, “fácil”); se velocidade importa, pergunte ou proponha uma medida testável.

### Etapa 1 — história em papel

Entenda o processo real sem software: quem começa, quem age, qual folha preenche, para quem passa e o que acontece depois. Se o discovery já trouxe história e participantes, confirme esse entendimento e avance sem repetir perguntas.

### Etapa 2 — caneta × papel

Quem segura a caneta e toma uma ação vira ator/permissão; o que fica registrado vira dado; o que tem ficha própria em outro lugar é assunto/referência. Não confunda quem age com o que está escrito. A pergunta de bolso é: **quem pode segurar a caneta aqui?**

### Etapa 3 — campos da folha

Classifique cada campo como:
- **Referência**: aponta a outra ficha, geralmente com sufixo `Id` (por exemplo, `unidadeId`).
- **Dado simples**: só existe nesta folha.
- **Assinatura**: quem segurou a caneta; por padrão, vem da sessão (`criadaPor`), não do corpo do pedido.
- **Estado**: situação atual de um compromisso, definida pelo domínio.

Pergunte se o dado tem vida própria. Se pode existir sozinho, considerar um módulo próprio é o padrão; se só existe dentro do registro-pai, costuma ser um item embutido. Registre exceções e custo de separá-lo ou mantê-lo junto.

### Etapa 4 — estados

Estados descrevem compromissos que ainda podem mudar. Fatos já aconteceram: registre-os, não os faça “voltar” a estados anteriores. Cada transição de estado corresponde a uma ação ou evento que precisa de regra e autorização. Modelar estados é um padrão quando há ciclo de vida, não uma exigência para todo registro.

### Etapa 5 — regras

Escreva condições proibidas/obrigatórias em linguagem simples e dê a cada regra um dono: a folha, uma ação ou uma política compartilhada. Pergunta de bolso: **se essa regra mudar, qual folha muda?** Para cada regra, crie pelo menos um critério de aceite que mostre o efeito observável. Uma regra compartilhada pode morar fora da entidade; registre por que.

### Etapa 6 — contrato

Descreva o recurso (substantivo na URL), o verbo HTTP, o endpoint e os DTOs de entrada e saída. O DTO é o formulário que atravessa a porta; mantenha o contrato externo separado do modelo interno por padrão, e registre quando reutilizar o tipo interno é seguro.

Uma ação de negócio costuma ser expressa como sub-recurso criado com `POST` (por exemplo, `POST /requisicoes/123/atendimento`) em vez de uma alteração cega de status via `PATCH`. Isso preserva validações e efeitos da ação. `PATCH` continua adequado para edição parcial simples; compare no cartão de decisão.

Como padrão de segurança, `id`, status controlado pelo servidor, datas de auditoria e autor não são aceitos do corpo quando pertencem ao servidor. Se uma dessas informações for uma escolha legítima do usuário, explicite a regra e o limite do contrato. O DTO de saída pode incluir nomes legíveis ou dados derivados (como `unidadeNome`) sem adicionar esses campos à folha.

### Etapa 7 — ação/caso de uso

Escreva em português a sequência de passos que realiza o pedido. Se coordena mais de um substantivo, trate-a como ação/caso de uso. Um ponto de partida é colocá-la no módulo do substantivo mais alterado e chamar especialistas dos outros módulos; se isso gerar acoplamento ou esconder uma responsabilidade, use outro dono e registre o trade-off. Banco costuma buscar e salvar sem decidir regra de negócio.

### Etapa 8 — dados e consistência

Identifique quais alterações precisam ser atômicas, o risco de duas pessoas agirem ao mesmo tempo e se é preciso histórico ou auditoria. Não presuma transação distribuída, lock ou event sourcing: compare simplicidade, consistência, volume, operação e custo de recuperação. Dinheiro, estoque e dados pessoais tendem a tornar a decisão uma porta de uma via.

### Etapa 9 — falhas e bordas

Considere permissões negadas, dado ausente/inválido, objeto removido, conflito de estado, dependência externa indisponível e repetição do mesmo pedido. Para cada caso relevante, registre o resultado observável, mensagem adequada, validação responsável e se é necessária idempotência. Não adicione mecanismos para riscos que não se aplicam sem dizer qual custo evitam.

### Etapa 10 — prova e entrega

Escreva critérios de aceite testáveis em Dado/Quando/Então. A frase principal descreve o que a pessoa vê; o detalhe técnico pode ir entre parênteses: “Então a tela avisa ‘você não tem acesso’ (resposta 403)”. “Deve ser rápido” não é observável; “responde em menos de 300 ms” pode ser, se houver necessidade e medida acordadas.

Ligue ao menos um critério de aceite a cada regra da etapa 5. Combine como testar, entregar e desfazer a mudança, em proporção ao risco. Um ajuste simples pode ter checklist curto; dinheiro, dados sensíveis, migrações e integrações podem exigir plano de reversão explícito.

## Sequência de dados e dependências (referência original)

O mesmo pedido muda de forma: a tela monta o DTO de entrada → vira JSON → a porta confere e obtém a identidade da sessão → a ação completa a folha (`id`, data, estado inicial, assinatura) → o banco guarda → um tradutor como `paraSaida` monta o DTO de saída → JSON volta à tela.

Os limites de importação e a árvore de pastas foram preservados em [`vocabulario.md`](vocabulario.md). Gere a Ficha expandida usando [`../templates/ficha-modelagem.md`](../templates/ficha-modelagem.md).
