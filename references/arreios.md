# Arreios — guardrails para decidir com pragmatismo

Leia no começo de qualquer entrada. São regras de processo sempre ativas para equilibrar investigação e andamento. Elas coexistem com o princípio de que recomendações arquiteturais são padrões revisáveis: não transforme preferência técnica em “lei”. Se um arreio for quebrado, avise em uma linha e pergunte se é intencional. Em `--learn`, pergunte qual arreio foi quebrado antes de explicar; em `--job`, aponte e siga quando for seguro.

1. **Problema antes de solução.** Toda proposta diz qual problema resolve e como saber que resolveu.
2. **Evidência antes de opinião.** “Acho que está lento” vira “como medimos?”. Sem evidência, registre suposição; não a apresente como fato.
3. **Porta de duas vias: decidir rápido.** Se é barato desfazer, siga um padrão razoável, registre em uma linha e avance.
4. **Porta de uma via: decidir com calma.** Compare opções, faça Cartão completo e obtenha confirmação explícita do dono da decisão.
5. **Menor passo útil.** Entre opções suficientes, escolha a menor e mais reversível, salvo motivo registrado para outra.
6. **Uma mudança por vez em produção.** Separe refatoração e funcionalidade em passos distintos; ação produtiva exige autorização explícita em Operar.
7. **Não otimizar o que não foi medido.** Primeiro defina sinal e limite aceitável; desempenho é custo, não suposição.
8. **“Bom o suficiente” tem critério.** Defina o que é pronto antes de agir e pare quando atingido; não continue por perfeccionismo.
9. **Limite de análise.** Se uma decisão reversível passar de três trocas sem fechar, proponha padrão, registre impasse/pergunta e siga o restante. Não aplique esse atalho a uma porta de uma via sem confirmação.
10. **Se não está documentado, não foi decidido.** Toda decisão relevante termina no Diário e, quando permanente, num Registro de Decisão.

As regras 1–10 orientam conversa e escopo; padrões de domínio/arquitetura têm suas próprias exceções em `trade-offs.md`. Evidência é requisito para afirmar estado real, não barreira para raciocinar com hipóteses identificadas.
