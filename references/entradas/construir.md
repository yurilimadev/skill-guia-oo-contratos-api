# Construir — etapas 0 a 10

Uma etapa por vez. Leia só o que a decisão atual pede: [`guia-de-decisao.md`](../guia-de-decisao.md), [`dominio.md`](../dominio.md), [`notas-de-conceito/`](../notas-de-conceito/README.md).

| # | Nome | Pergunta central | Produz |
|---|---|---|---|
| 0 | Problema | Objetivo, para quem, como saber que deu certo, o que fica fora? | objetivo e sucesso observável |
| 1 | História em papel | Como funciona sem sistema? | processo e glossário |
| 2 | Caneta × papel | Quem age e o que fica registrado? | atores, permissões |
| 3 | Campos da folha | Referência, dado, assinatura ou estado? | modelo e cardinalidade |
| 4 | Estados | Que situações o compromisso atravessa? | estados e transições |
| 5 | Regras | O que é invariante, o que é política, qual folha protege? | regras, donos e critérios |
| 6 | Contrato | O que entra e o que volta? | endpoints e DTOs |
| 7 | Ação | Que sequência cumpre o pedido? | caso de uso e dono |
| 8 | Dados | O que precisa mudar junto? E em concorrência? | agregado, transação, histórico |
| 9 | Falhas | O que falha? E se repetir o pedido? | erros, validação, idempotência |
| 10 | Prova | Como provar, entregar, desfazer e conferir? | aceite, testes, rollback |

## Triagem

| Tamanho | Profundidade |
|---|---|
| Pequeno | etapas condensadas; 8–10 em checklist de uma linha; até 2 cenários de estresse |
| Médio | fluxo completo |
| Grande | fluxo completo, riscos e plano de entrega |

Justifique em uma frase o custo de modelar demais e de menos. Se um caso pequeno revelar mais de cinco regras, estados ou módulos, pare e reclassifique.

## Hierarquia antes de desenhar

- **`--learn`:** consulta antes do desenho. Pergunte como o dado é procurado e que resposta o sistema precisa dar sem abrir um documento.
- **`--job`:** proponha a hierarquia com hipóteses marcadas e teste cada subdomínio depois.
- Cada elemento nasce `hipótese` e vira `testada` ou `refutada`. Nenhum Brief sai com hipótese aberta em porta de uma via.
- Tabela é decisão de dados, com cartão. Não é consequência automática de molde.

## Etapas

**0 — Problema.** Quem tem o problema, objetivo, resultado observável e fora de escopo. Sucesso será conferido em produção na etapa 10.

**1 — História em papel.** O processo real, não a tela imaginada. Comece o glossário com as palavras que todos usam do mesmo jeito.

**2 — Caneta × papel.** Quem age vira ator/permissão; o que fica escrito é dado. O ator humano não vira classe automaticamente: descubra a responsabilidade que o software assume. Não invente quem aprova.

**3 — Campos da folha.** Classifique cada campo: referência (`...Id`), dado simples, assinatura (vem da sessão) ou estado. Pergunte identidade × valor, cardinalidade, cópia × referência e unidade. Candidatos a objeto nascem da necessidade, não do substantivo ([`notas-de-conceito/objeto-vs-procedural.md`](../notas-de-conceito/objeto-vs-procedural.md)).

**4 — Estados.** Estados mudam por ação; fatos já ocorridos não são desfeitos por status. Muitos booleanos costumam esconder um ciclo de vida. Não crie estado para todo registro.

**5 — Regras.** Separe invariante de política ([`notas-de-conceito/invariante-politica.md`](../notas-de-conceito/invariante-politica.md)). Cada regra tem dono e um critério que a prova. Antes do contrato, teste o modelo contra 3–5 mudanças plausíveis: aguenta? quanto custa mudar? Pequeno: até 2.

**6 — Contrato.** Recurso, verbo, endpoint, DTO de entrada e saída. DTO separado é padrão, não dogma. Ação de negócio com regra ou efeito usa sub-recurso `POST`; `PATCH` fica para edição simples. Campos de servidor não vêm do corpo.

**7 — Ação.** Escreva a sequência. Regra local fica no dono do dado; ação coordena vários substantivos e pode ser transitória. Persistência busca e grava; não decide negócio.

**8 — Dados.** O que precisa mudar junto ([`notas-de-conceito/agregado.md`](../notas-de-conceito/agregado.md)), risco de concorrência e necessidade de histórico. Não adote transação, evento ou lock sem motivo.

**9 — Falhas.** Permissão, dado inválido, conflito de estado, integração fora do ar e repetição. Diga o resultado visível, a mensagem, quem valida e como recupera.

**10 — Prova.** Dado/Quando/Então observável, um critério por regra da etapa 5. Combine teste, deploy e rollback proporcionais ao risco. Registre quando e como conferir em produção.

## Fechamento

Preencha a Ficha, os cartões, o diário e o Brief. Termine com dois **Pontos fracos desta proposta**. Não declare pronto com suposição aberta em porta de uma via. Diga o que documentar e onde.
