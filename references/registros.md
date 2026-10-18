# Registros: cartão, diário de bordo e ADR

Toda decisão relevante gera **um cartão** e **uma entrada no diário**. Decisões que atendem aos três critérios de ADR também geram **um ADR**. Os três são documentos do projeto, não da skill.

## 1. Cartão de decisão (durante a conversa)

Modelo em [`../templates/cartao-decisao.md`](../templates/cartao-decisao.md). A ordem é fixa:

1. **Sua parte:** decisão, minha razão, assunto (técnico, teórico ou ambos), alternativa descartada, o que me faria mudar de ideia.
2. **Crítica da skill:** ponto frágil, alternativa que merece teste, pergunta que falta responder.
3. **Decisão final:** mantenho ou mudo, razão final, reversibilidade, quando revisitar.

Se a sua parte estiver vazia, a skill não comenta a decisão; pergunta a razão primeiro.

## 2. Diário de bordo — `docs/diario-de-bordo.md`

Um arquivo por projeto, em ordem cronológica. Toda entrada começa com data e hora, para que você saiba quando decidiu.

```markdown
## 2026-10-18 14:30 · Etapa 5 · Regra de saldo fica em estoque/regras

- **Decisão:** a regra de saldo mora em `estoque/regras`, chamada pela ação de atendimento.
- **Minha razão:** o saldo é do estoque; a requisição não deve saber como ele é calculado.
- **Descartei:** regra dentro da ação de atendimento, porque espalharia a regra se outra ação precisar de saldo.
- **Mudaria se:** o saldo passar a depender de dados que só a requisição conhece.
- **Registro:** diário (reversível, duas vias).
- **Modo:** learn.
```

Entradas de código ficam com a marca `[código]` no título, por exemplo `## 2026-10-18 16:00 · [código] if ou switch no tipo de status`. Elas são pendências para a fase de código.

### Quando dividir o diário

Ao abrir uma sessão sobre o projeto, se o arquivo passar de **400 linhas**, sugira arquivar o período anterior:

- mover as entradas fechadas para `docs/diario-de-bordo/AAAA-MM.md`;
- manter no arquivo principal um índice com um link para cada mês e as decisões ainda abertas;
- nunca apagar entradas; só mover.

A decisão de arquivar é sua.

## 3. ADR — `docs/adr/NNNN-slug.md`

Use o formato do skill `domain-modeling` (`ADR-FORMAT.md`), com uma linha de data acrescentada para você lembrar quando decidiu:

```markdown
# Regra de saldo fica no módulo de estoque

Data: 2026-10-18

Contexto e decisão em 1 a 3 frases, com a razão. Alternativas e consequências só quando agregam.
```

Numere sequencialmente a partir do maior número existente em `docs/adr/`.

### Quando virar ADR

Os três critérios precisam ser verdadeiros:

1. **Difícil de reverter:** mudar depois teria custo real.
2. **Surpreendente sem contexto:** alguém que ler o código vai se perguntar por que foi feito assim.
3. **Resultado de um trade-off real:** havia alternativas, e você escolheu por razões específicas.

Se faltar algum, fica só no diário. Decisão fácil de reverter se reverte sem ADR.

## Relação entre os três

- Toda decisão: cartão + entrada no diário.
- Decisão com os três critérios: também ADR. A entrada do diário aponta para o número do ADR (`Registro: ADR-0003`).
- Decisão revista: a entrada original não é apagada. A nova entrada diz qual decisão substitui, e o ADR antigo recebe o status `superseded` se houver campo de status.
