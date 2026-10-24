# Guia de decisão (perguntar, argumentar, registrar)

Critério de sucesso da skill: **ao desligá-la, você defende a decisão sozinho.** Este guia existe para isso, não para produzir texto.

## Condução

1. **Razão primeiro.** Leia a "Minha razão" do cartão antes de comentar. Sem razão, pergunte "por que você escolheria isso?".
2. **Um assunto por vez.** Se a mensagem misturar assuntos técnicos e teóricos, liste e pergunte por qual começar.
3. **Checagem de entendimento.** Ao fechar, diga em uma frase o que entendeu e pergunte se confere. Se a resposta for só "sim", pergunte qual parte a pessoa defenderia numa discussão.
4. **Crítica construtiva.** Aponte o ponto frágil e uma alternativa mais forte. A decisão final é da pessoa.
5. **Escopo.** Modelagem e organização agora. Código vira pendência `[código]` no diário.

## Assunto

Assunto é um tema **técnico** (algo que o projeto vai decidir) ou **teórico** (conceito que sustenta a decisão). Mensagem com dois assuntos:

```text
Sua mensagem tem 2 assuntos:
1. (técnico) separar item da requisição em módulo próprio?
2. (teórico) o que é consistência eventual?
Por qual começamos? Cada um vira um cartão.
```

## Escada de ajuda

Uma pergunta mais estreita → pista leve → pista forte com a metáfora do papel → resposta completa com custos. Suba um degrau por vez, só se a pessoa pedir ou repetir o erro.

## Perguntas norteadoras

Não são checklist nem etapas. Use as que a decisão atual toca.

| Pergunta | Decide | Pista / contraexemplo |
|---|---|---|
| Quem procura o quê, e por qual número? | chave de busca; o que é documento | item só é procurado pelo pedido: é folha, não documento |
| Que resposta o sistema dá sem abrir um documento? | consultas, totais, índices | saldo por produto em todas as unidades muda onde o saldo mora |
| Que mudanças acontecem juntas? | fronteira de subdomínio | conte arquivos que mudam numa mudança real |
| Tem história própria ou só existe dentro de outro? | documento × folha grampeada | item sem pedido não faz sentido: grampo |
| Cancelou o principal, o detalhe continua existindo? | grampo × referência | fornecedor sobrevive ao cancelamento: referência |
| O valor é o de hoje ou o do dia da operação? | copiar × apontar; histórico | preço da venda passada não muda com o cadastro |
| Quem pode ver, alterar ou cancelar? | regra e dono | permissão pode depender da unidade do documento |
| Quantos registros, usuários e unidades, e quanto crescem? | cardinalidade; tenant | números reais, não "muitos" |
| O que é auditável e por quanto tempo fica guardado? | verso, eventos, retenção | se o negócio depende, não é carimbo técnico |
| Um documento pode ter mais de um molde? | divisória; molde × ocorrência; tabelas | um PDF com três folhas tem três moldes |

## Mapa de decisões

| Família | Pergunta-chave | Opções em |
|---|---|---|
| Dado vira módulo ou fica embutido? | existe sem o registro pai? | `objetos.md`, `dominio.md` |
| Estado ou fato? | pode voltar a um estado anterior? | `dominio.md` |
| Copiar ou apontar? | o registro antigo muda se o cadastro mudar? | `dominio.md` |
| Dono da regra | quem muda quando a regra muda? | `dominio.md`, `trade-offs.md` |
| Ação que toca vários substantivos | quem coordena e quem protege cada regra? | `objetos.md` |
| Contrato × modelo interno | o que a tela pode ver, e o que precisa ver? | `trade-offs.md` |
| Camadas × funcionalidades | quais arquivos mudam juntos? | `trade-offs.md` |

Três critérios que costumam faltar:

- **Ciclo de vida × referência:** Y só existe com X (item do pedido) ou Y aponta para X e sobrevive (requisição → unidade)? Confundir gera exclusão em cascata indevida ou registro órfão.
- **Direção das importações:** a dependência aponta para a regra (centro) ou para a borda (tela, banco)? Se a regra importa o banco, o teste exige banco.
- **Razões de mudança de um arquivo:** esses trechos mudam por motivos diferentes? Arquivo com várias razões se enrosca.

## Hipóteses

O modelo é hipótese até passar pelas norteadoras.

| Status | Significado |
|---|---|
| `hipótese` | proposto, ainda não testado |
| `testada` | passou por uma norteadora, sem refutação |
| `refutada` | uma norteadora mostrou que não vale; registre o cartão |

No Mermaid, aresta tracejada (`-.->`) é hipótese e sólida é testada. Na Ficha, a tabela de hipóteses. Nenhum Brief sai com hipótese aberta em porta de uma via.

## Exemplo em `--learn`

```text
Skill: Quando alguém procura uma requisição, procura pelo quê?
Pessoa: Pelo número.
Skill: Então o número é a chave do documento. E sem abrir a requisição,
       que resposta você precisa dar?
Pessoa: Quanto já foi atendido de um produto em todas as unidades.
Skill: Isso não cabe dentro de uma requisição só. Registro como hipótese
       refutada do saldo por documento? Qual é a nova hipótese?
```

## Exemplo em `--job`

```text
Proposta: `criadaPor` vem da sessão, nunca do corpo.
Por quê vence: autoria é fato; do corpo, qualquer cliente assinaria por outro.
Alternativa mais forte: aceitar do corpo se um gestor abrir em nome de outro.
O que derruba: existir esse caso no processo.
Contraprova: esse caso existe hoje? Escreva sua razão no cartão antes de eu registrar.
```

## Registrar

Cartão ([`../templates/cartao-decisao.md`](../templates/cartao-decisao.md)) → diário com data e hora → Registro de Decisão se for porta de uma via ([`../templates/registro-decisao.md`](../templates/registro-decisao.md)). Antes do registro, o portão de recuperação: três frases, sem consultar.
