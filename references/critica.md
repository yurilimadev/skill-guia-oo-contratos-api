# Lente de crítica — argumentar sobre qualquer proposta

Use em Construir, Herdar e Operar, inclusive para avaliar uma recomendação desta skill. Crítica não é procurar defeitos por esporte: é testar se o raciocínio serve ao problema e deixar claros os riscos que a pessoa aceita.

## Seis perguntas

1. Qual problema isso resolve, e como vou saber que resolveu?
2. Que alternativa foi descartada, e por quê?
3. O que aqui é caro de desfazer?
4. O que foi assumido sem me perguntar?
5. Que mudança provável no negócio quebra isso?
6. Como vou saber que está funcionando em produção?

## Uso obrigatório

- **Autocrítica:** toda proposta relevante e todo Brief terminam com a seção **Pontos fracos desta proposta**, com no mínimo dois pontos que abordam as perguntas 3, 4 e 5. Explicite suposição, custo e gatilho; não finja que a proposta é sem risco.
- **`--learn`:** no máximo uma vez por sessão, avise “isto é um exercício”, apresente uma proposta plausível com 1–2 problemas escondidos e peça à pessoa para encontrá-los usando as seis perguntas. Depois revele e explique; mantenha o exercício sobre o trabalho real.
- **Entregas de outras IAs:** se a pessoa colar proposta, arquitetura ou código, responda às seis perguntas com fatos/linhas, hipóteses, riscos, informações faltantes e alternativas. Não reescreva a entrega a menos que seja pedido.
- **Proporção:** em pedido pequeno, críticas cabem em poucas linhas; não invente risco especulativo. Para um risco sem evidência, marque como suposição ou pergunta.

## Formato de pontos fracos

```markdown
## Pontos fracos desta proposta
- **Custo de desfazer:** <qual escolha/migração fica cara e por quê>
- **Suposição:** <o que ainda não foi confirmado; status e impacto>
- **Mudança que pode quebrar:** <cenário plausível e gatilho para revisar>
```

Se algum desses itens não se aplica, diga por que; ainda mantenha pelo menos dois pontos concretos. Conecte-os a evidências, cartões de decisão e documentos vivos, sem misturar confirmado e inferido.
