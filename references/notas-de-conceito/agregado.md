# Agregado (o que precisa mudar junto)

**Uma frase:** agregado é a menor fronteira que precisa mudar junta para continuar válida.

## Exemplo no domínio

Pedido e seus itens: um pedido sem itens coerentes é inválido. Saldo e movimentações: a baixa e o registro do fato precisam concordar.

## Ponte procedural → objeto

| Em procedural você faria | Com agregado |
|---|---|
| Uma transação em volta de vários `UPDATE` soltos | A raiz do agregado expõe operações que garantem a regra |
| Cada tabela com sua função de salvar | Só a raiz grava o conjunto |
| A regra de "tudo junto" fica na cabeça de quem chama | A regra fica na raiz |

**Ganho:** consistência garantida num lugar; menos estado inválido.
**Custo:** fronteira maior, contenção sob concorrência, transações mais longas.

## Quando importa

- Há uma regra que cruza várias linhas ou tabelas.
- Duas pessoas podem alterar o mesmo conjunto ao mesmo tempo.

## Quando não importa

- Registros independentes, CRUD sem regra entre linhas.
- Leitura para relatório: pode ser uma consulta, não um agregado.

## Pergunta de decisão

"O que precisa estar certo ao mesmo tempo, sempre?"

## Custo de errar

- Agregado grande: contenção e locks desnecessários.
- Agregado pequeno demais: estados que deveriam ser impossíveis passam a existir.

## Bancada sugerida

Duas operações relacionadas, uma sem raiz e outra com raiz que valida o conjunto. Force uma falha no meio e veja o que sobra.
