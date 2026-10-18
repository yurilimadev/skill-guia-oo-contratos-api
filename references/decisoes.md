# Decisões de modelagem e organização

Este arquivo cobre decisões de **modelagem** (quais dados existem, quem os possui, como se relacionam) e de **organização** (como agrupar arquivos e módulos). Decisões de código (`if`/`switch`, loops, array ou lista encadeada, funções) ficam para a fase de código.

Para cada decisão: a pergunta-chave, o que pesa, o sinal de que a escolha padrão falhou e a pergunta que a skill faz. As opções com padrão, quando quebrar e custos ficam em [`trade-offs.md`](trade-offs.md).

## Modelagem

### 1. O dado vira módulo próprio ou fica embutido?

- **Pergunta-chave:** esse dado existe sem o registro pai?
- **O que pesa:** vida própria, reuso por outros módulos, quem altera.
- **Sinal de falha:** módulo criado para algo que só existe dentro de outro registro; ou dado embutido que outros módulos precisam consultar sozinhos.
- **Pergunta da skill:** "Se a requisição for apagada, este item deve continuar existindo?"

### 2. Estado ou fato?

- **Pergunta-chave:** isso pode voltar a um estado anterior?
- **O que pesa:** estados mudam por ação e têm regras de transição; fatos são registrados e não desfeitos (compensa-se com outro fato).
- **Sinal de falha:** um fato guardado como campo editável, ou um estado que nunca muda.
- **Pergunta da skill:** "Se alguém errar o atendimento, você corrige o estado ou registra um novo fato?"

### 3. Quem é a fonte de verdade de um dado?

- **Pergunta-chave:** se o dado mudar na origem, quem deve refletir a mudança?
- **O que pesa:** copiar dado facilita a leitura e cria divergência; referenciar por id exige buscar o dado quando precisa.
- **Sinal de falha:** o mesmo dado aparece com valores diferentes em duas telas.
- **Pergunta da skill:** "O nome da unidade na requisição é o nome atual ou o nome do momento da criação?"

### 4. A qual folha pertence cada campo?

- **Pergunta-chave:** o campo descreve este registro ou outro?
- **O que pesa:** referência (`unidadeId`) aponta para outra folha; dado simples fica nesta; assinatura (quem fez) vem da sessão.
- **Sinal de falha:** campo que parece do registro mas muda com a vida de outro registro.
- **Pergunta da skill:** "Quem preenche `criadaPor`: a pessoa no formulário ou o sistema, a partir de quem está logado?"

### 5. Quem é o dono de uma regra?

- **Pergunta-chave:** se esta regra mudar, qual folha ou ação muda?
- **O que pesa:** regra intrínseca ao ciclo de vida fica perto do dado; regra que envolve vários módulos fica na ação; política compartilhada tem dono próprio.
- **Sinal de falha:** a mesma regra escrita em duas telas ou em duas ações.
- **Pergunta da skill:** "A regra de saldo mora na requisição ou no estoque? Quem muda quando ela mudar?"

### 6. Quem coordena a ação que toca vários substantivos?

- **Pergunta-chave:** "atender" altera a requisição, o estoque e cria movimentação: quem manda nessa sequência?
- **O que pesa:** a ação coordena; os substantivos cuidam das próprias regras.
- **Sinal de falha:** o módulo de estoque conhece detalhes de requisição.
- **Pergunta da skill:** "O estoque precisa saber que o pedido é de uma requisição, ou só precisa saber que houve saída?"

### 7. Dependência entre estruturas

- **Pergunta-chave:** se X for apagado, Y precisa continuar existindo? Y depende de X para existir ou só aponta para X?
- **O que pesa:** dependência de ciclo de vida é diferente de referência; confundir as duas gera cascata de exclusão onde não deveria, ou dado órfão.
- **Sinal de falha:** registro órfão no banco ou exclusão que apaga mais do que a pessoa imaginava.
- **Pergunta da skill:** "Apagar uma unidade deve apagar as requisições dela, marcá-las como inválidas, ou impedir a exclusão?"

### 8. Contrato externo separado do modelo interno?

- **Pergunta-chave:** o que a tela pode ver, e o que ela precisa ver?
- **O que pesa:** o contrato muda por razões de cliente; o modelo muda por razões internas.
- **Sinal de falha:** um campo interno vazando para a API, ou uma mudança interna quebrando o cliente.
- **Pergunta da skill:** "Se você trocar o nome interno do campo, a tela precisa mudar?"

### 9. Ação de negócio como sub-recurso ou alteração de status?

- **Pergunta-chave:** a mudança tem regra, autorização ou efeito colateral?
- **O que pesa:** se tem, é uma ação com nome próprio (`POST .../atendimento`); se é só edição de campo, `PATCH`.
- **Sinal de falha:** `PATCH` de status que pula transições ou esquece a movimentação.
- **Pergunta da skill:** "Se alguém mandar `status: atendida` direto, o que deveria acontecer com o estoque?"

## Organização

### 10. Camadas ou funcionalidades?

- **Pergunta-chave:** quando uma regra de negócio muda, quais arquivos mudam juntos?
- **O que pesa:** organização por funcionalidade reduz a distância entre arquivos que mudam juntos; por camada, facilita enxergar tecnologias e substituí-las. Projetos pequenos ou com estrutura já estável podem seguir o que existe.
- **Sinal de falha:** para uma mudança de regra, abrir quatro pastas diferentes; ou, no caso oposto, uma pasta de funcionalidade com código de banco, HTTP e regra misturados.
- **Pergunta da skill:** "Na última mudança de regra que você fez, quais pastas abriu?"

### 11. Quanto de um módulo cabe em um arquivo?

- **Pergunta-chave:** esses trechos mudam por motivos diferentes?
- **O que pesa:** arquivos com uma razão de mudança são fáceis de entender; arquivos com várias razões se enroscam.
- **Sinal de falha:** um arquivo de "utilitários" ou "tipos" que cresce sem dono.
- **Pergunta da skill:** "Se você mudar o formato de saída, esse arquivo muda junto com a regra de saldo?"

### 12. Quem pode importar quem?

- **Pergunta-chave:** a dependência aponta para o centro (regra) ou para a borda (tela, banco)?
- **O que pesa:** dependências que apontam para a borda tornam a regra difícil de testar e reaproveitar.
- **Sinal de falha:** ciclo de importação, ou regra importando um módulo de tela.
- **Pergunta da skill:** "Se a regra importar o banco, o teste da regra precisa de banco?"

## Como usar este catálogo

- Não percorra as 12 decisões de uma vez. Escolha as que a etapa atual toca.
- Para cada decisão tomada, peça a razão da pessoa antes da crítica (ver [`socratico.md`](socratico.md)).
- Se a pergunta for de código, registre como pendência `[código]` no diário ([`registros.md`](registros.md)) e volte à modelagem.
