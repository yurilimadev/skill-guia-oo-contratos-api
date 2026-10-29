# Bancada (exercício executável descartável)

Bancada é um experimento pequeno, executável e descartável que responde **uma** pergunta de decisão. Não é projeto: sem framework, sem banco, sem dependência, sem vida própria.

## Quando usar

- o conceito é o que trava uma decisão de porta de uma via; ou
- a pessoa errou a mesma decisão duas vezes.

Não use para conceito simples nem para "explorar". Explorar sem pergunta vira procrastinação.

## Formato: três passos

1. **Versão que você já sabe.** A solução procedural: função, lista, dicionário. É o ponto de partida honesto de quem vem de dados.
2. **Versão com o conceito.** O mesmo problema com objeto, agregado, interface ou padrão.
3. **Comparação.** O que ficou mais claro, o que custou, quando cada versão vence.

## Regras

- **Escreva a pergunta antes do código**, em uma frase. Sem pergunta, não há bancada.
- Rode e **mostre a saída**. Código que não roda não prova nada.
- ≤40 linhas e 15 minutos. Se passar, o problema está grande demais: corte.
- Roda em `/tmp/bancada-<tema>/`. Nunca no repositório do projeto.
- Zero dependência. Só a biblioteca padrão da linguagem-alvo.
- Apague no fim. O que fica é a conclusão, não o arquivo.
- Teto de **duas bancadas por sessão**. Na terceira, pare e revise o mapa de pré-requisitos ([`pre-requisitos.md`](pre-requisitos.md)).

## Mermaid como auxiliar

Opcional e **subordinado ao executável**:

- o código responde "funciona? o que mudou na saída?";
- o Mermaid responde "quem fala com quem? qual responsabilidade ficou onde?";
- no máximo um diagrama, até 10 nós, preferindo `sequenceDiagram`;
- aresta tracejada para hipótese, conforme [`diagramas.md`](diagramas.md);
- se o desenho divergir da execução, vale a execução.

## Modelo

```markdown
Pergunta: <uma frase>
Versão procedural: <o que você já sabe>
Versão com o conceito: <o que muda>
Saída observada: <o que rodou>
Comparação: <ganho, custo, quando cada uma vence>
Conclusão para o cartão: <o que isso muda na decisão>
```

## O que a bancada não faz

Não decide. Ela informa. A decisão continua no Cartão de Decisão, com a razão da pessoa. No modo `--job`, não há bancada; no máximo um contraexemplo de 5 linhas colado na resposta.
