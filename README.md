# Da Folha ao Código — Skill

Skill em português para pensar uma funcionalidade de ponta a ponta, explicitar trade-offs e preparar um brief pronto para implementação. Mantém o método Da Folha ao Código e acrescenta dois modos: `--learn` (aprender fazendo) e `--job` (entregar com raciocínio).

## Instalação

No projeto em que você quer usar a skill, execute:

```bash
npx skills add yurilimadev/skill-guia-oo-contratos-api
```

Para instalar globalmente para o seu usuário, acrescente `-g`:

```bash
npx skills add yurilimadev/skill-guia-oo-contratos-api -g
```

O comando usa o CLI `skills`, que instala skills em agentes compatíveis. Para escolher um agente específico, consulte `npx skills add --help`.

## Como usar

Inclua a flag de modo no pedido e confirme a linguagem/stack. Se houver manifests no projeto, a skill sugere o que detectou e pede confirmação antes de decidir. Ela começa cada resposta com uma linha de estado, por exemplo: `Modo: learn · Etapa 3/10 · Campos da folha`.

### `--learn` — aprender fazendo

Mais perguntas que respostas, uma decisão real de cada vez. Antes de recomendar, a skill pergunta quais opções você vê e o que perde com cada uma.

```text
--learn
Quero modelar a funcionalidade [nome] antes de implementar.
Linguagem/stack: [por exemplo, Python 3.12 — confirme se detectou outra no projeto]
Contexto: [problema, para quem e processo atual sem sistema]
Resultado esperado: [o que a pessoa deve conseguir fazer]
Regras ou limites que já conheço: [se houver]
Conduza-me por Da Folha ao Código uma etapa por vez. Ajude-me a pensar antes
de apresentar a recomendação. Ao final, entregue a Ficha, o Diário de Decisões,
suposições/perguntas em aberto, Brief para Assistente e um exercício parecido
para eu resolver sozinho. Não implemente ainda.
```

### `--job` — entregar com raciocínio

A skill propõe a decisão, explica o trade-off em poucas frases e pergunta quando falta uma decisão de negócio ou há uma porta de uma via.

```text
--job
Modele [funcionalidade] para [usuário/objetivo]. A stack é [linguagem e versão].
Processo atual: [como funciona hoje]
Resultado e limites: [o que precisa acontecer e o que fica fora]
Conduza as etapas necessárias sem pular nenhuma; agrupe as perguntas de cada
etapa numa rodada. No final, entregue Ficha, Cartões de Decisão e Brief
autocontido para outro assistente implementar. Não escreva o código da feature.
```

### Trocar de modo e revisar

Você pode mudar de modo durante a conversa: diga `--job` ou `--learn` (ou “muda para o modo X”). A skill anuncia a troca, mantém Ficha, Diário e perguntas, e continua na etapa atual.

Para revisar código existente, por exemplo:

```text
--job
Revise o módulo [nome/caminho] usando Da Folha ao Código. Reconstrua a folha
e os estados, aponte sinais de alerta com arquivo e motivo e proponha
de → para → por quê e os trade-offs. Não mova nem altere arquivos sem minha aprovação.
Linguagem/stack: [confirme a detectada]
```

O fluxo completo está em [`SKILL.md`](./SKILL.md); as referências são carregadas sob demanda.

## O que a skill faz

- Ajuda a transformar um processo real em tipos, estados, regras, contrato de API e ações.
- Conduz uma etapa por vez, fazendo perguntas e mostrando o resultado antes de avançar.
- Entrega uma Ficha de Modelagem pronta para orientar a implementação.
- Inclui um modo de revisão para identificar responsabilidades fora do lugar em código existente.

Leia [`SKILL.md`](./SKILL.md) para ver as instruções completas.

## Créditos

Este guia adapta, com crédito, ideias de linguagem simples, dimensionamento pelo tamanho do problema, registro de suposições/perguntas, critérios observáveis e referências carregadas sob demanda da skill [`onp-spec-driven`](https://github.com/onovoprogramador/onp-spec-driven), de Vitor Manoel — O Novo Programador, disponibilizada sob licença MIT. O texto e o método de modelagem deste repositório foram reorganizados e escritos para Da Folha ao Código; o motor/scripts daquela skill não foram copiados.
