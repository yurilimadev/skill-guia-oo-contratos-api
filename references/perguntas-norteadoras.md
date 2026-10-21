# Perguntas norteadoras — da consulta à hierarquia

Perguntas que ligam o que as pessoas precisam fazer com o sistema à forma da hierarquia (armário → subdomínio → divisória → documento → molde/ocorrência → campo). Não são uma checklist nem um formulário: cada uma decide um pedaço do modelo. Use só as que a conversa precisa.

Não é uma sequência de etapas com número. A ordem depende do modo:

- **`--learn`:** consulta primeiro. A skill faz as perguntas sobre como o sistema é consultado; a pessoa responde; só então desenha a hierarquia, com hipóteses marcadas.
- **`--job`:** desenho primeiro. A skill propõe a hierarquia com hipóteses marcadas e testa cada subdomínio contra as perguntas; cada hipótese fecha ou é refutada.

O teste depende da entrada. Em **Construir**, as perguntas partem do processo real e do modelo proposto. Em **Herdar**, partem do modelo existente (H2, domínio reverso) e confrontam spec, código e verificação. Em qualquer caso, o modelo é hipótese até passar pelas perguntas.

## As perguntas

### 1. Quem procura o quê, e por qual número ou nome?

- **Decide:** chave de busca e o que é documento.
- **Pista:** se alguém pede pelo número, esse número é a chave do documento; se ninguém pede nada isolado, talvez não seja documento.
- **Contraexemplo:** um “item” nunca é procurado sozinho, sempre pelo pedido. Então é folha grampeada, não documento.

### 2. Que resposta o sistema precisa dar sem abrir um documento?

- **Decide:** consultas, totais e o que precisa de índice ou de projeção (read model).
- **Pista:** liste as perguntas de painel e de relatório, não só as de tela de detalhe.
- **Contraexemplo:** “quanto temos de produto X em todas as unidades?” exige somar saldos de várias fichas. Se o modelo guarda o saldo só dentro de cada documento, a consulta vira varredura. Isso muda onde o saldo mora.

### 3. Que mudanças acontecem juntas?

- **Decide:** fronteira de subdomínio (pasta sanfonada).
- **Pista:** conte os arquivos e telas que mudam numa mudança real, não numa imaginada.
- **Contraexemplo:** cadastro de produto e regra de compra mudam por razões diferentes. Se estiverem na mesma pasta, uma mudança de compra arrasta o cadastro. Pasta é área de mudança, não só de tema.

### 4. Este registro tem história própria, ou só existe dentro de outro?

- **Decide:** documento × folha grampeada.
- **Pista:** sem o registro principal, o detalhe ainda faz sentido para o negócio?
- **Contraexemplo:** itens de um pedido não fazem sentido sem o pedido: grampo. Um fornecedor existe mesmo sem compra: documento próprio, com referência.

### 5. Se o registro principal for cancelado, este detalhe continua existindo?

- **Decide:** grampo × referência.
- **Pista:** quem é dono do ciclo de vida? Se o detalhe acompanha o principal, grampo; se não, referência.
- **Contraexemplo:** cancelar uma compra não deve apagar o fornecedor, mas deve apagar os itens que só existiam nela. Para auditoria, talvez os itens precisem sobreviver como fatos. Então a regra de vida vem do requisito, não da metáfora.

### 6. Este valor precisa ser o de hoje, ou o do dia da operação?

- **Decide:** copiar × apontar; histórico.
- **Pista:** se o cadastro mudar amanhã, a operação de hoje deve mudar junto?
- **Contraexemplo:** preço de uma venda passada não pode mudar quando o cadastro muda. Então a venda guarda o preço do dia (cópia), mesmo que o produto aponte para o cadastro atual.

### 7. Quem pode ver, alterar ou cancelar?

- **Decide:** regra e quem a protege; permissões por unidade, papel ou subdomínio.
- **Pista:** a mesma pessoa pode ter papéis diferentes em unidades diferentes?
- **Contraexemplo:** uma gestora pode cancelar requisições da própria unidade, mas não de outra. A permissão depende da unidade do documento, então `unidadeId` não é só um campo: é parte da regra de acesso.

### 8. Quantos registros, usuários e unidades existem, e quanto crescem?

- **Decide:** cardinalidade; se for preciso separar dados por cliente ou unidade (tenant), isso vira fronteira de implantação ou de isolamento.
- **Pista:** números reais, mesmo aproximados, e a taxa de crescimento.
- **Contraexemplo:** um sistema com 50 usuários e 2 mil documentos por ano não precisa da mesma estrutura de um sistema com milhões. Mas se cada cliente tem dados isolados por lei, isolar é requisito, mesmo que o volume seja pequeno.

### 9. O que precisa ser auditável, e por quanto tempo fica guardado?

- **Decide:** o que vai no “verso” (carimbos do sistema e dependências de negócio), se há eventos e por quanto tempo guardar.
- **Pista:** se alguém precisar explicar uma mudança, o que ele precisaria ter?
- **Contraexemplo:** quem alterou um preço e quando é um fato de negócio, não só um carimbo técnico. Se o negócio depende dele, sai do verso e vira campo ou evento.

### 10. Um mesmo documento pode ter mais de um molde?

- **Decide:** divisória × molde × ocorrência; número de linhas e tabelas.
- **Pista:** um documento impresso pode ter várias folhas de tipos diferentes? Quantas ocorrências de cada molde existem por documento?
- **Contraexemplo:** um PDF de atendimento com folha de identificação, folha de exames e folha de assinatura tem três moldes. A forma no banco é decisão de dados, com cartão: uma tabela por molde, uma por documento, ou linhas por ocorrência. A hipótese “cada molde vira uma tabela” só é aceita depois desse teste.

## Como marcar o que foi testado

Cada elemento do desenho tem um status:

| Status | Significado |
|---|---|
| `hipótese` | proposto, ainda não testado por nenhuma pergunta |
| `testada` | passou por pelo menos uma pergunta, sem refutação |
| `refutada` | uma pergunta mostrou que não vale; substituir e registrar no cartão |

Para o desenho e a Ficha, veja [`../templates/ficha-modelagem.md`](../templates/ficha-modelagem.md). Para as arestas tracejadas de hipótese, veja [`diagramas.md`](diagramas.md).

## Regras de uso

- Não despeje as dez perguntas de uma vez. Escolha as que a etapa atual toca.
- Em `--learn`, no máximo duas por mensagem, como no resto do modo.
- Em `--job`, teste o subdomínio inteiro de uma vez e apresente a tabela de hipóteses.
- Pergunta sem exemplo real vira resposta vaga. Peça uma situação concreta antes de fechar.
- Nenhum Brief sai com hipótese aberta que afete porta de uma via.

## Exemplo de teste em `--job`

Subdomínio “requisições” da requisição de produtos, desenho proposto primeiro e testado depois:

| Elemento | Hipótese proposta | Norteadora | Resultado | O que mudou |
|---|---|---|---|---|
| Número da requisição | é a chave do documento | 1 | `testada` | nenhuma |
| Itens | molde grampeado na requisição | 4 e 5 | `testada` | nenhuma: item só existe com o pedido |
| Saldo por produto | fica dentro de cada requisição | 2 | `refutada` | consulta de painel exige somar requisições; saldo passa a ser projeção do estoque |
| Unidade da requisição | referência para a unidade | 5 | `testada` | nenhuma: cancelar a requisição não apaga a unidade |
| Preço do item | aponta para o cadastro atual | 6 | `refutada` | a requisição deve guardar o preço do dia (cópia), para auditoria |
| Tabela por molde | cada molde vira tabela | 10 | `hipótese` | aberta: a decisão de dados depende de um cartão |

Cada `refutada` gera cartão com a razão da pessoa. A linha `hipótese` impede o Brief de sair pronto enquanto a norteadora 10 não for feita, se a decisão de dados for uma porta de uma via.
