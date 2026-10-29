# Pré-requisitos do projeto (escopo, não currículo)

Um projeto não usa todos os conceitos de programação. O mapa de pré-requisitos diz o que **este** projeto exige, onde cada item morde e quando carregá-lo. É o que impede o JIT de virar adivinhação: você sabe o que pode precisar antes de precisar.

## Onde vive

Em `docs/prerequisitos.md` do projeto, criado na primeira sessão e revisto quando muda a entrada ou o escopo. Não é documento da skill.

## Formato (≤15 linhas preenchidas)

```markdown
# Pré-requisitos — <projeto>

## Essencial (sem isso a decisão sai errada)
- <conceito> — morde em <onde>: <o que acontece se faltar>

## Sob demanda (carregar quando a decisão tocar)
- <conceito> — carregar quando <gatilho>

## Fora de escopo / desconhecido
- <conceito> — fora porque <motivo>
- <desconhecido> — investigar se <sinal>
```

## Os três grupos

| Grupo | Critério | Exemplo |
|---|---|---|
| Essencial | a decisão fica errada sem ele | invariante × política, se há regra de saldo |
| Sob demanda | só importa em certas decisões | agregado, quando surge consistência entre tabelas |
| Fora de escopo | não muda este projeto | fila assíncrona, se tudo é síncrono e curto |

## Como a skill usa

- Antes de abrir uma nota de conceito, confira se o item está no mapa. Se não estiver, pergunte se entra ou fica fora; não ensine por conta própria.
- Um item "essencial" que a pessoa não domina é candidato a bancada.
- O mapa não autoriza despejar conceito. Ele só evita esquecer o que importa.
- No fim da sessão, atualize o mapa com o que apareceu de novo.

## Exemplo curto (requisição de produtos)

```markdown
## Essencial
- Estado × fato — no status da requisição e na movimentação de estoque
- Invariante × política — saldo não negativo é invariante; limite por unidade é política

## Sob demanda
- Agregado — quando decidir o que muda junto no atendimento
- DTO — quando definir o contrato

## Fora de escopo
- Event sourcing — não há requisito de auditoria além do histórico simples
- Multi-tenant — uma empresa, uma base
```
