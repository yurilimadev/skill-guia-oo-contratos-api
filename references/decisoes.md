# Mapa de decisões de modelagem e organização

Use este mapa para saber **qual pergunta fazer** e **onde estão as opções**. Não repete o catálogo: as opções e os trade-offs estão nas referências indicadas. Aqui ficam só os critérios que faltavam.

Decisões de código (`if`/`switch`, loops, array ou lista encadeada, funções) não entram aqui. Ficam para a fase de código, registradas como `[código]` no diário.

## Como usar

1. Identifique a família da decisão (abaixo).
2. Faça a pergunta-chave à pessoa, antes de sugerir opções.
3. Leia a referência indicada só se a pessoa precisar de opções ou de exemplo.
4. Peça a razão dela, registre no cartão ([`../templates/cartao-decisao.md`](../templates/cartao-decisao.md)) e só então critique.

## Modelagem

| Família | Pergunta-chave | Opções e critérios em |
|---|---|---|
| Dado vira módulo próprio ou fica embutido? | Esse dado existe sem o registro pai? | [`objetos.md`](objetos.md), [`dominio.md`](dominio.md) (identidade × valor), trade-offs 11 |
| Estado ou fato? | Isso pode voltar a um estado anterior? | [`dominio.md`](dominio.md) (evento, histórico), trade-offs 16 |
| Fonte de verdade: copiar ou apontar? | Se o cadastro mudar amanhã, o registro antigo muda junto? | [`dominio.md`](dominio.md) (tempo e histórico), trade-offs 12 |
| Dono da regra: invariante ou política? | Isso pode mudar por decisão do negócio? | [`dominio.md`](dominio.md) (invariante × política), trade-offs 4 |
| Ação que toca vários substantivos | Quem coordena e quem protege cada regra? | [`objetos.md`](objetos.md), trade-offs 3 e 4 |
| Contrato externo × modelo interno | O que a tela pode ver, e o que precisa ver? | trade-offs 2 |

### Dependência de ciclo de vida × referência

- **Pergunta-chave:** se X for apagado, Y precisa continuar existindo?
- **Dependência de ciclo de vida:** Y só existe com X (item da requisição). Y é apagado junto, ou a exclusão de X fica bloqueada.
- **Referência:** Y guarda um identificador de X e continua existindo (requisição guarda `unidadeId`). Y precisa lidar com X ausente.
- **Sinal de falha:** registro órfão no banco, ou exclusão que apaga mais do que a pessoa imaginava.
- **Pergunta da skill:** “Apagar uma unidade deve apagar as requisições dela, marcá-las como inválidas, ou impedir a exclusão?”

## Organização

| Família | Pergunta-chave | Opções e critérios em |
|---|---|---|
| Camadas ou funcionalidades | Quando uma regra muda, quais arquivos mudam juntos? | trade-offs 1 |

### Direção das importações

- **Pergunta-chave:** a dependência aponta para a regra (centro) ou para a borda (tela, banco)?
- **Direção de referência:** tela → porta → ações → regras → tipos; ações → persistência → tipos. Um projeto existente pode ter outra direção válida; registre a fronteira em vez de forçar a forma.
- **Sinal de falha:** ciclo de importação, ou regra importando módulo de tela. Se a regra importa o banco, o teste da regra precisa de banco.
- **Pergunta da skill:** “Se a regra de saldo importar o banco, o teste dela precisa de um banco de dados?”

### Razões de mudança de um arquivo

- **Pergunta-chave:** esses trechos mudam por motivos diferentes?
- **Critério:** arquivo com uma razão de mudança é fácil de entender; arquivo com várias se enrosca. Um arquivo `utils` ou `tipos` que só cresce costuma ter várias razões.
- **Sinal de falha:** mudar o formato de saída obriga a mexer no arquivo da regra de saldo.
- **Pergunta da skill:** “Se você mudar o formato de saída, esse arquivo muda junto com a regra de saldo?”

## Como registrar

Cada decisão de modelagem ou organização vira cartão e entrada no diário com data e hora. Se atender aos três critérios de registro permanente (difícil de reverter, surpreendente sem contexto, resultado de trade-off real), vira um registro em `docs/decisoes/` conforme [`../templates/registro-decisao.md`](../templates/registro-decisao.md).
