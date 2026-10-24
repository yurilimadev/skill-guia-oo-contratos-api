# Acoplamento (quem depende de quem)

**Uma frase:** acoplamento é o quanto mudar um pedaço obriga a mudar outro; a direção importa mais que a quantidade.

## Exemplo no domínio

A tela importa a regra de saldo: mudar a regra quebra telas. A regra importa o banco: testar a regra exige banco.

## Ponte procedural → objeto

| Em procedural você faria | Com direção definida |
|---|---|
| Um arquivo gigante chamando tudo | Regra no centro; tela e banco na borda |
| A regra lê a tela ou o banco direto | A regra recebe o que precisa e não conhece a borda |
| Teste da regra sobe a aplicação inteira | Teste da regra roda sozinho |

**Ganho:** regra testável e reaproveitável; mudança na borda não quebra a regra.
**Custo:** mais camadas e tradução entre elas; em script pequeno, pode ser exagero.

## Quando importa

- A regra precisa ser provada sem tela e sem banco.
- Partes têm donos ou ritmos de mudança diferentes.

## Quando não importa

- Protótipo descartável, script de uma execução.

## Pergunta de decisão

"Dá para testar a regra sem subir tela e banco?"

## Custo de errar

- Ciclo de importação e mudança em cascata.
- Teste lento e frágil, que esconde regressão.

## Bancada sugerida

A mesma regra duas vezes: uma lendo direto de um "banco" falso e outra recebendo os dados. Compare o esforço de testar.
