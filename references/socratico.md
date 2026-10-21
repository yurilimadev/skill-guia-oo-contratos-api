# Conduzir por perguntas — socrático

A decisão é da pessoa. A skill faz perguntas, testa o argumento e registra o que a pessoa escolheu e por quê. Não substitui o entendimento dela por uma resposta pronta.

## Regras de condução

1. **Razão primeiro.** Antes de comentar uma decisão, leia a “Minha razão” escrita pela pessoa no cartão ([`../templates/cartao-decisao.md`](../templates/cartao-decisao.md)). Sem razão, pergunte: “por que você escolheria isso?”.
2. **Um assunto por vez.** “Assunto” é um tema técnico ou teórico, confirmado pela pessoa:
   - **técnico:** algo que o projeto vai decidir ou construir (modelo, fronteira, contrato, organização, dados);
   - **teórico:** conceito que sustenta a decisão (idempotência, acoplamento, consistência eventual).
3. **Checagem de entendimento.** Ao fechar uma decisão, diga em uma frase o que entendeu e pergunte se confere.
4. **Escopo.** Modelagem e organização agora. Perguntas sobre código (`if`/`switch`, loops, array ou lista encadeada, funções) são registradas como pendência `[código]` no diário e voltam na fase de código.
5. **Crítica construtiva.** Aponte o ponto frágil da razão e uma alternativa mais forte. Se a pessoa mantiver a escolha, registre a versão dela.

## Múltiplos assuntos na mesma mensagem

Sinais: duas perguntas no mesmo pedido, “e também”, tema técnico misturado com teórico, ou mais de um substantivo decidido de uma vez.

A skill não responde tudo junto. Lista os assuntos, marca cada um e pergunta por qual começar:

```text
Sua mensagem tem 2 assuntos:
1. (técnico) separar item da requisição em módulo próprio?
2. (teórico) o que é consistência eventual?
Por qual começamos? Cada um vira um cartão separado.
```

## Checagem de entendimento

```text
Entendi que você decidiu manter o item dentro da requisição porque
ele não existe sem ela, e descartou módulo próprio por enquanto.
Confere? O que eu deixei de fora?
```

Se a resposta for só “sim”, pergunte: “Qual parte você defenderia numa discussão com alguém que discorda?”

## Escada de ajuda

Mesma escada de [`modo-learn.md`](modo-learn.md): pergunta mais estreita → pista leve → pista forte com a metáfora do papel → resposta completa com custos. Suba um degrau por vez.

## Banco de perguntas

Cada item tem uma pergunta, uma pista e um contraexemplo para testar a resposta. Os exemplos usam a requisição de produtos ([`exemplos/requisicao.md`](exemplos/requisicao.md)).

### Dono da informação

- **Pergunta:** quem cria, quem altera e quem só lê este dado?
- **Pista:** se duas telas mostrarem o mesmo dado, de onde ele deveria vir?
- **Contraexemplo:** a requisição guarda o nome da unidade ou só o `unidadeId`? Se a unidade for renomeada, a requisição antiga mostra o nome novo ou o do momento? Qual você quer e por quê?

### Vida própria e dependência de ciclo de vida

- **Pergunta:** se o registro pai for apagado, este dado precisa continuar existindo?
- **Pista:** dependência de ciclo de vida (item só existe na requisição) é diferente de referência (requisição aponta para unidade). Confundir as duas gera exclusão em cascata onde não deveria, ou dado órfão.
- **Contraexemplo:** se a unidade for apagada, as requisições dela somem, são marcadas como inválidas, ou a exclusão é bloqueada? Cada resposta leva a uma regra diferente.

### Estado ou fato

- **Pergunta:** isso pode voltar a um estado anterior, ou já aconteceu?
- **Pista:** fatos são registrados, não desfeitos; estados mudam por ação.
- **Contraexemplo:** se alguém errar o atendimento, você corrige o estado ou registra um novo fato? Qual parte da história é estado e qual é fato?

### Quem importa quem

- **Pergunta:** se A importa B e B importa A, qual é a regra e qual é o detalhe?
- **Pista:** a regra de negócio tende ao centro; tela e banco tendem à borda.
- **Contraexemplo:** a tela importa a regra de saldo diretamente. Se a regra mudar, quantos arquivos de tela mudam?

### Organização: camadas ou funcionalidades

- **Pergunta:** quando uma regra muda, quantas pastas você precisa abrir?
- **Pista:** conte os arquivos que mudaram juntos numa mudança real, não numa mudança imaginada.
- **Contraexemplo:** pastas por camada (`controllers/`, `services/`, `repositories/`) com a regra de requisição espalhada em quatro pastas. Organizar por funcionalidade concentraria a regra em `requisicao/`. Que custo você aceita em troca?

### Ação ou dado

- **Pergunta:** este verbo mexe em um substantivo ou em vários?
- **Pista:** se mexe em vários, a ação coordena; se mexe em um, a regra pode morar nele.
- **Contraexemplo:** “atender requisição” baixa estoque e cria movimentação. Em qual módulo ela mora e quais módulos chama?

Para dúvidas de objeto, classe ou ator humano, use [`objetos.md`](objetos.md). Para mudanças de negócio que quebram o modelo, use [`dominio.md`](dominio.md).

## Exemplo em `--learn`

```text
Skill: Você disse que a requisição guarda `unidadeId`. Por que o id, e
       não o nome da unidade?
Pessoa: Para não duplicar.
Skill: Entendi: você quer uma única fonte de verdade para o nome.
       Pista: a unidade pode ser renomeada. O que a requisição de ontem
       deveria mostrar?
Pessoa: O nome que tinha quando foi criada.
Skill: Então o nome vira um dado da requisição no momento da criação,
       ou um fato. Qual dos dois você escolhe e por quê?
```

A skill não diz qual resposta está certa. Ela leva a pessoa a perceber que “não duplicar” e “mostrar o histórico” são critérios diferentes.

## Exemplo em `--job`

```text
Decisão proposta: `criadaPor` vem da sessão, nunca do corpo do pedido.
Argumento: quem abriu a requisição é um fato de autoria; se viesse do
corpo, qualquer cliente poderia assinar em nome de outra pessoa.
Alternativa mais forte: aceitar `criadaPor` do corpo se um gestor puder
abrir a requisição em nome de outro.
O que derrubaria a proposta: um caso real em que o gestor abre em nome
de outro.
Contraprova: esse caso existe no seu processo atual? Escreva sua razão
no cartão antes de eu registrar.
```

No `--job`, a skill apresenta o argumento completo e pede contraprova. Não fecha a decisão sem a razão da pessoa.

## Perguntas por norteadora

As perguntas completas, com pista e contraexemplo, estão em [`perguntas-norteadoras.md`](perguntas-norteadoras.md). Ao conduzir uma norteadora, use a mesma regra das demais: uma pergunta por vez, a razão da pessoa antes da crítica, e o resultado registrado como `testada` ou `refutada`.

Exemplo em `--learn`, antes de qualquer desenho:

```text
Skill: Quando alguém procura uma requisição, pelo quê procura: número,
       unidade, data ou produto?
Pessoa: Pelo número, sempre.
Skill: Então o número é a chave do documento. E sem abrir a requisição,
       que resposta você precisa dar? Por exemplo, quanto já foi atendido
       de um produto em todas as unidades?
Pessoa: Isso é do relatório, e ele é lento.
Skill: Anotei como hipótese: o saldo por produto não pode morar só
       dentro do documento. Você quer que eu a registre como pergunta
       aberta para a norteadora 2?
```
