# Ficha de Modelagem — <nome da funcionalidade>

- **Modo:** learn / job
- **Linguagem/stack confirmada:** <linguagem, versão e ferramentas ou “a confirmar”>
- **Tamanho:** pequeno / médio / grande — <justificativa e custo de modelar demais/de menos>

## 0. Problema

- **Objetivo e para quem:**
- **Critérios de sucesso observáveis:**
- **Fora de escopo:**

## 1. História em papel

<3–5 linhas: quem começa, o que preenche, para quem passa e o que acontece depois>

## 2. Caneta × papel

| Pessoa/papel/sistema | Segura a caneta? | Ação/permissão | O que fica registrado? |
|---|---|---|---|

## 3. Substantivos

| Substantivo | Tipo (quem/o quê/compromisso/fato) | Vida própria? | Módulo sugerido |
|---|---|---|---|

### Campos da folha principal

| Campo | Referência/simples/assinatura/estado | Quem fornece? | Observações |
|---|---|---|---|

## 4. Estados e ações

`ESTADO_A → (ação) → ESTADO_B ...`

| Transição | Quem pode agir? | Regra/efeito |
|---|---|---|

## 5. Regras

| Regra | Dono (folha/ação/política) | Arquivo/módulo | Critério de aceite |
|---|---|---|---|

## 6. Contrato

| Ação | Verbo | Endereço | DTO de entrada | DTO de saída |
|---|---|---|---|---|

## 7. Ações/casos de uso

| Ação | Mora em (proposta) | Passos em português | Módulos chamados |
|---|---|---|---|

## 8. Dados e consistência

- O que precisa ser atômico:
- Concorrência esperada e como evitar conflito:
- Histórico/auditoria necessários:
- Decisões de armazenamento/transação e cartões associados:

## 9. Falhas e bordas

| Situação | Resultado/mensagem observável | Validação/responsável | Idempotência ou recuperação |
|---|---|---|---|

## 10. Prova e entrega

### Critérios de aceite (Dado / Quando / Então)

```text
Dado <situação verificável>
Quando <ação verificável>
Então <resultado observável para a pessoa> (<detalhe técnico opcional>)
```

- Plano de testes:
- Plano de entrega:
- Como desfazer/reverter:

## Suposições

| ID | Suposição e impacto | Status: aberta/confirmada/invalidada |
|---|---|---|

Escreva “Nenhuma.” se não houver; faça uma última busca por lacunas. Não feche um Brief com suposição aberta numa decisão de porta de uma via.

## Perguntas em aberto

| ID | Pergunta que só o dono do negócio pode responder | Status: aberta/respondida |
|---|---|---|

Escreva “Nenhuma.” se não houver.

## Diário de Decisões

Inclua um Cartão de Decisão por decisão relevante usando [`cartao-decisao.md`](cartao-decisao.md). Mantenha opções, custos, reversibilidade, gatilho de revisão e modo.

## Pastas e arquivos sugeridos

```text
<árvore baseada nas convenções existentes do projeto>
```

## Próximo passo / pontos ainda por decidir

<dúvidas, responsável e o que destrava o Brief>
