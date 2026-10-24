# Objeto (o que muda em relação a procedural)

**Uma frase:** objeto é um pedaço de software que junta dados com o comportamento que protege esses dados.

## Exemplo no domínio

`Estoque` guarda o saldo de cada produto e a regra "saldo não fica negativo".

## Ponte procedural → objeto

| Em procedural você faria | Com objeto |
|---|---|
| `saldo = {produto: 10}` e uma função `baixar(saldo, produto, qtd)` | `estoque.baixar(produto, qtd)`, com a regra dentro do objeto |
| Qualquer código pode alterar o dicionário direto | O dado só muda pelo método que valida |
| A regra vive numa função que alguém pode esquecer de chamar | A regra vive onde o dado está |

**Ganho:** um lugar só para a regra; difícil burlar.
**Custo:** mais arquivos, cerimônia, curva de aprendizado; em script curto, pode ser exagero.

## Quando importa

- A regra precisa valer sempre, em qualquer entrada.
- Várias operações diferentes mexem no mesmo dado.
- Você precisa testar a regra sem tela e sem banco.

## Quando não importa

- Script descartável, transformação sem estado, protótipo de uma tarde.

## Pergunta de decisão

"Se eu esquecer de chamar a função, o dado pode ficar inválido?"

## Custo de errar

- Regra espalhada: dados inválidos e correção em vários lugares.
- Classe para tudo: cerimônia sem ganho, mais difícil de ler.

## Bancada sugerida

Baixar saldo com uma função e depois com um objeto que recusa saldo negativo. Compare o que cada versão impede.
