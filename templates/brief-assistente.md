# Brief para o Assistente — <funcionalidade>

> Este brief é autocontido para colar em outro assistente. Não dependa de prints nem de contexto implícito. Se houver suposição aberta numa porta de uma via, confirme-a antes de marcar este brief como pronto.

## Objetivo e contexto

- **Objetivo / para quem:**
- **Critérios de sucesso:**
- **Fora de escopo:**
- **Linguagem/stack e versão confirmadas:**

## Módulos e arquivos

| Módulo/caminho | Criar ou alterar | Responsabilidade |
|---|---|---|

## Modelo e estados

- Tipos/folhas, campos e referências:
- Estados e transições:

## Regras e contrato

| Regra | Dono |
|---|---|

| Ação | Verbo + endpoint | DTO entrada | DTO saída |
|---|---|---|---|

## Consistência, falhas e segurança

- Transações/concorrência/histórico:
- Falhas, validação, idempotência e recuperação:
- Limites de permissão/privacidade:

## Decisões tomadas

Resumo dos Cartões: escolha, motivo, custo e reversibilidade.

## Critérios de aceite

Use Dado/Quando/Então observável; toda regra deve estar provada por pelo menos um critério.

## Suposições e perguntas

- Suposições e status:
- Perguntas em aberto e status:
- Se alguma suposição aberta for porta de uma via, **não iniciar a implementação** até confirmar.

## O que NÃO fazer

- Não alterar arquivos fora dos caminhos acordados sem perguntar.
- Não mover arquivos sem aprovação.
- Não colocar regras de negócio na rota/tela quando a proposta registrada as atribui ao domínio.
- Não aceitar campos controlados pelo servidor sem a validação e decisão documentadas.
- Acrescente proibições específicas desta funcionalidade:

## Entrega

- Plano de testes:
- Entrega e reversão:
- Arquivos esperados ao final:

## Variante para `onp-spec-driven` (se o usuário já usar essa skill)

Ofereça uma versão estruturada em histórias de usuário, critérios Dado/Quando/Então, suposições e perguntas em aberto em tabelas com status. Preserve os mesmos trade-offs e não invente comandos, IDs globais ou etapas do `onp-spec-driven` sem contexto do projeto.
