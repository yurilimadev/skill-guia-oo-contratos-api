# Anki — glossário e regras de negócio

Estes arquivos transformam o glossário em **recuperação ativa**: em vez de reler a definição, você tenta responder antes de virar o cartão. É o mesmo princípio do portão de recuperação da skill: se você não consegue explicar sem consultar, ainda não é seu.

## Arquivos

- `glossario-sistema.tsv` — termos, distinções e protocolo de leitura.
- `regras-de-negocio.tsv` — cartões para treinar regras e investigar as do ERP.
- Este `LEIA-ME.md` — instruções e critérios para criar novos cartões.

Os dois TSV já têm o cabeçalho do Anki:

```text
#separator:tab
#html:true
#tags column:3
```

## Como importar no Anki

1. Abra o Anki e escolha **Arquivo → Importar**.
2. Selecione `glossario-sistema.tsv`.
3. Confirme:
   - separador: **Tab**;
   - campo 1: **Front**;
   - campo 2: **Back**;
   - campo 3: **Tags**;
   - tipo de nota: **Basic**;
   - baralho: crie um chamado **Glossário de sistemas**.
4. Importe. Depois repita com `regras-de-negocio.tsv` em outro baralho, por exemplo **Regras de negócio — DreneHub**.

Se o Anki não reconhecer o cabeçalho, selecione o separador **Tab** manualmente. O arquivo é UTF-8; acentos e `→` devem aparecer corretamente.

### Ao atualizar os TSV

Use a opção **“Atualizar notas existentes quando o primeiro campo coincidir”**. Assim você corrige uma definição sem criar cartão duplicado. O primeiro campo é a pergunta; mantenha-o estável.

## Tags

- `glossario::nucleo` — vocabulário que deve ser colado em toda sessão.
- `glossario::fluxo` — estado, regra, invariante, risco.
- `glossario::design` — camada, dependência, fronteira, contrato.
- `glossario::decisao` — ADR, alternativa, trade-off, premissa, suposição.
- `glossario::distincao` — pares que costumam se confundir.
- `glossario::protocolo` — os sete passos de leitura.
- `glossario::confianca` — confirmado, declarado, hipótese.
- `glossario::meta` — o que o glossário não é e como usar.
- `regra::meta` — como classificar e registrar uma regra.
- `regra::erp` — regras ou investigações do ERP.
- `status::hipotese` — ainda não confirmado; não trate como verdade.
- `status::exemplo` — exemplo didático, não regra do ERP.

## Cartões de regra: cuidado com o status

Os cartões marcados `status::hipotese` vieram do print da análise do ERP. Eles são **perguntas de investigação**, não regras confirmadas. Quando você confirmar uma regra, troque a tag para `status::confirmado` e atualize a resposta com a evidência.

Os cartões de regra devem responder, no mínimo:

- é **política** ou **invariante**?
- onde está **declarada**, **implementada** e **verificada**?
- qual é o **efeito observável** e o **efeito irreversível**?
- o que acontece se a regra for violada?
- quem pode mudar a regra?
- qual é a **evidência** (`arquivo:linha`, teste ou consulta)?

## Modelo para uma regra nova

Use uma linha TSV com três campos. O segundo campo pode usar `<br>` para quebrar linhas.

```text
Regra: <nome da regra>	Tipo: política/invariante<br>Declarada em: <spec/ADR><br>Implementada em: <arquivo:linha><br>Verificada por: <teste/consulta><br>Efeito: <observável/irreversível><br>O que a viola: <condição><br>Status: aberta/confirmada	regra::erp status::aberta
```

## Critérios para um bom cartão

- **Um fato por cartão.** Se a resposta tem duas ideias independentes, divida.
- **Pergunte antes de afirmar.** O front deve exigir uma resposta, não apenas reconhecer.
- **Resposta curta.** Duas a quatro linhas; detalhes vão para o glossário ou para a evidência.
- **Use suas palavras.** Copiar a definição não prova entendimento.
- **Marque a confiança.** `confirmado`, `declarado` ou `hipótese`.
- **Não transforme hipótese em verdade.** Se não há evidência, o cartão deve dizer isso.
- **Revise os cartões.** Um cartão confuso ensina a confusão; reescreva ou apague.

## Relação com o glossário

A fonte é `Glossário compartilhado eu + LLMs.md`. Se o glossário mudar, atualize os cartões correspondentes. O baralho de termos treina o vocabulário; o de regras treina a decisão sobre o domínio. O segundo só deve crescer com regras que você consegue defender.
