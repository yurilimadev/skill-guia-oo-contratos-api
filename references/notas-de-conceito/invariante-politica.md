# Invariante e política (não confunda as duas)

**Uma frase:** invariante protege a integridade e não pode ser violada; política é regra que o negócio pode ajustar.

## Exemplo no domínio

- **Invariante:** o saldo não fica negativo.
- **Política:** o limite de compra por unidade é 50.

## Ponte procedural → objeto

| Em procedural você faria | Com a distinção |
|---|---|
| `if` espalhado em cada função que mexe no saldo | Invariante mora no dono do dado e vale sempre |
| Limite fixo no código | Política em configuração ou objeto de política, com dono |
| Mudar o limite exige deploy | Mudar a política não exige deploy |

**Ganho:** o que é lei fica protegido; o que é ajuste fica ajustável.
**Custo:** exige conversa com o dono do negócio para classificar; pode parecer burocracia no começo.

## Quando importa

- A regra tem dono e chance plausível de mudar.
- A violação causa dano real (dinheiro, estoque, dados pessoais).

## Quando não importa

- Regra trivial, estável, sem dono. Não formalize por esporte.

## Pergunta de decisão

"Se a direção mudar de ideia amanhã, isso muda sem deploy?"

## Custo de errar

- Política tratada como invariante: código engessado, deploy para cada ajuste.
- Invariante tratada como política: alguém configura o impossível e o dado fica inválido.

## Bancada sugerida

Saldo negativo bloqueado no código e limite de compra vindo de configuração. Tente violar os dois e veja qual deveria ser possível.
