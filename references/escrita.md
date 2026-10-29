# Padrão de escrita (contrato de quem escreve)

Vale para os textos que a skill produz. O objetivo é simples: o texto existe para a pessoa decidir melhor, não para mostrar cobertura.

## Três alvos

| Alvo | Onde | Regra |
|---|---|---|
| Documentação da skill | `SKILL.md`, `references/*` | respeitar os tetos abaixo; uma ideia por parágrafo; zero duplicação |
| Saída na conversa | explicações, crítica, notas | exemplo antes da regra; jargão explicado na primeira aparição |
| Modelos | cartão, ficha, brief, registro | só o campo pede; sem texto de enfeite |

## Duas exclusões

- **O texto da pessoa não é editado.** "Minha razão" e as respostas são dela.
- **O estilo do projeto não é nosso.** Nomes, formatação e convenções de código seguem o repositório.

## Tetos

| Artefato | Teto |
|---|---|
| `SKILL.md` | 70 linhas |
| Referência de etapa ou entrada | 80 linhas |
| Nota de conceito | 40 linhas |
| Cartão de Decisão | 10 linhas de campos |
| Registro de Decisão | 6 campos, 1–3 frases no contexto |
| Texto carregado por passo | ~800 palavras |
| Bancada | 40 linhas de código, 15 minutos |

Ao estourar, corte o que não muda a decisão. Não corte exemplo nem custo de errar.

## Regras

1. Exemplo antes da regra. Regra sem exemplo vira decoreba.
2. Uma ideia por parágrafo. Parágrafo com duas ideias esconde a segunda.
3. Nenhum termo novo sem uma linha de explicação na primeira aparição.
4. Frase curta. Se a frase tem três orações, provavelmente são três frases.
5. Cada conceito mora em um arquivo. Os outros apontam, não repetem.
6. Escreva o custo de errar. Sem custo, não há decisão.
7. Prefira número a adjetivo: "responde em 300 ms", não "é rápido".
8. Não venda a solução. Diga em que contexto ela vence e em qual perde.

## Antes e depois

| Antes | Depois |
|---|---|
| "É importante considerar a possibilidade de utilizar um DTO separado para evitar acoplamento entre as camadas de apresentação e domínio." | "DTO separado quando o formato da tela muda por motivo diferente do modelo. Custo: traduzir nos dois sentidos." |
| "Podemos observar que existem diversas abordagens possíveis, cada uma com seus prós e contras." | "Duas opções. A primeira vence se X; a segunda, se Y." |
| "O agregado é uma fronteira de consistência transacional que garante invariantes." | "Agregado é o que precisa mudar junto para continuar válido. Exemplo: pedido e itens." |

## Teste final

Leia em voz alta. Se uma frase não muda o entendimento ou a decisão, corte. Se você precisou reler, reescreva.
