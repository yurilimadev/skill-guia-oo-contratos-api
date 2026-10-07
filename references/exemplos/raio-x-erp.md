# Exemplo fictício — Raio-X e Operar com o bonde andando

**Todos os dados, caminhos e evidências abaixo são inventados para demonstração.** Não representam servidor, domínio, IP, nome de pessoa, credencial nem configuração real.

## Pedido inicial e abertura

> “Herdei um ERP de estoque e financeiro para uma clínica com várias unidades. É TypeScript/Next.js, Postgres numa VPS própria e uma integração externa. Já temos ambientes separados e backup, mas nunca testei restauração e não há monitoramento. Às vezes falha e volta com F5.”

Classificação sugerida: **Entrada Herdar**, modo `--job` (confirmar); stack alvo TypeScript/Next.js (confirmar); referência sugerida Python (confirmar). Ponto de partida: **bonde andando**. O erro/F5 é sinal relatado, não prova de causa. Antes da primeira pergunta, se `docs/` existir, ler o diário, `operacao.md` e decisões relevantes.

## Evidência no exemplo

| Item | Estado | Evidência fictícia redigida | O que não se conclui |
|---|---|---|---|
| Ambientes | existe e verificado | dois manifests de deploy revisados com valores sensíveis ocultos; referências de banco lógico distintas e smoke check feito em homologação isolada | não prova que homologação está igual à produção em todos os aspectos |
| Backup diário | existe, não verificado | painel redigido indica job diário concluído | job concluído não prova restauração; nunca houve restore testado |
| Monitor de disponibilidade | não existe | inventário não encontrou probe externo nem destinatário de alerta | ausência no repositório, sozinha, não prova ausência no painel do provedor; perguntar/confirmar |
| Coleta de erro no navegador | existe, não verificado | pessoas relatam erro/retry com F5, mas não há evento capturado | não sabemos frequência, request, navegador nem causa |
| Produção | existe e verificado | a pessoa confirma que há VPS, DNS e banco em uso; detalhes sensíveis ficam fora do Raio-X | não operar, copiar domínio ou consultar segredo sem permissão |

## H1 — inventário

| Área | Confirmado no exemplo | A confirmar |
|---|---|---|
| Stack | TypeScript e Next.js declarados; Postgres; VPS própria | versões, arquitetura exata e runner de deploy |
| Dados | estoque, financeiro, várias unidades | cardinalidade por tabela, backups/retenção e relatórios críticos |
| Integração | existe uma integração externa | dono, contrato, política de retry e ambiente de teste |
| Ambientes | produção e homologação separadas e verificadas no escopo visto | paridade, seed de dados redigidos, acesso e rollback |
| Operação | backup diário configurado, ainda sem restore validado; sem monitor confirmado | retenção, pessoa de plantão e destino de alertas |

Não registrar valores de `.env`, segredo do provedor, domínio ou IP. Isso é inventário de estrutura, não convite para abrir painel real sem autorização.

## H2 — domínio reverso

Glossário provisório: **Produto** pode significar item com saldo em Estoque, custo no Financeiro e item de serviço na Agenda; confirmar sentidos antes de unir os modelos. **Movimentação** registra um fato ocorrido. **Unidade** define o escopo de acesso/saldo. A mesma pessoa pode ser paciente, funcionária ou gestora.

No snippet fictício abaixo, o fato observado é que um handler lê `produtoId` e atualiza saldo; o significado de “custo atual” como valor da movimentação é **hipótese** até confirmar com o negócio.

```text
src/app/api/estoque/route.ts:40–55    recebe produtoId e quantidade
src/estoque/atualizar-saldo.ts:18–31 valida saldo e grava
src/db/movimentacoes.ts:22–36        persiste o fato de movimentação
```

| Folha/termo | Confirmado ou hipótese | Pergunta que falta |
|---|---|---|
| Produto | tipos e tabela fictícios têm id e saldo | Estoque, Financeiro e Agenda querem a mesma definição? |
| Quantidade | armazenada como número | Qual unidade? Caixa, item, fracionada? |
| Custo histórico | rota consulta custo atual (hipótese ilustrativa) | Movimentação deve copiar custo/preço na data do fato? |
| Pessoa/papel | aplicação declara papéis em sessão | Uma pessoa pode ter vários papéis e unidades? |

## H3 — fluxos de dados

1. **Registrar entrada de estoque (exemplo hipotético):** tela de recebimento → `src/app/api/estoque/route.ts:40–55` → `src/estoque/atualizar-saldo.ts:18–31` → `src/db/movimentacoes.ts:22–36` → resposta. Regra de saldo parece estar em `atualizar-saldo.ts`; confirmado somente dentro do snippet fictício.
2. **Ver custo financeiro (hipótese):** tela Financeiro → API de relatório (arquivo ainda não inspecionado) → consulta de custo. Não afirmar que os dois fluxos usam a mesma regra sem verificar.

Perguntas de discovery: o que a equipe vê antes de apertar F5? A falha some sem repetir ação? Há risco de duplicar movimento? Qual registro confirma o saldo esperado? Em `--learn`, peça à pessoa para localizar uma rota e apontar onde acha que a regra mora antes de revelar o mapa.

## H4 — diagnóstico risco × frequência

| Achado | Risco | Frequência | Estado da evidência | Prioridade |
|---|---|---|---|---|
| Erro relatado que “resolve com F5” | potencialmente alto se movimento duplicar/perder | desconhecida; não há medição | relato, não log | instrumentar e caracterizar antes de concluir causa |
| Backup sem restore testado | alto para recuperação de dados | frequência de falha desconhecida | backup configurado, restauração não verificada | planejar restore isolado antes de refatoração de dados |
| Sem monitor externo confirmado | alto tempo de descoberta | falha observada por reclamação, sem taxa | ausente no escopo conferido | próximo passo de visibilidade |
| Código com regras em vários arquivos | variável | depende de quantas alterações repetem regra | só marcar após busca com caminhos/linhas | não refatorar até teste de caracterização |

Não rotular o código de “espaguete” como evidência: diga quais responsabilidades/direções concretas aparecem e que dano causam. Feio mas inofensivo fica abaixo de erro de saldo, duplicação ou indisponibilidade.

## Registros retroativos de decisão

Os registros seguintes são exemplos do artefato permanente, não arquivos criados em sistema real. Motivos não conhecidos permanecem assim.

### `docs/decisoes/0001-ambientes-separados.md`

```markdown
# 0001 — Ambientes separados

- Data: 2026-10-07 (exemplo fictício)
- Status: aceita (registro retroativo)
- Origem: configuração existente antes do Raio-X
- Evidência: dois manifests redigidos e referências de banco lógico distintas; smoke check isolado registrado
- Contexto: produção e homologação já existem
- Opções consideradas: manter separação; unificar ambientes
- Decisão observada: manter separados
- Motivo: motivo não registrado
- Consequências: continuar validando paridade; nunca usar produção como homologação
- Como verificar: revisar manifests redigidos e repetir smoke check em homologação
- Pontos fracos: paridade completa não foi provada; a razão original pode conter requisito não registrado
```

### `docs/decisoes/0002-backup-diario.md`

```markdown
# 0002 — Backup diário configurado

- Data: 2026-10-07 (exemplo fictício)
- Status: aceita (registro retroativo, benefício não verificado)
- Origem: job de backup já configurado
- Evidência: painel fictício mostra execução diária concluída; não há restore registrado
- Contexto: proteção de dados do ERP
- Opções consideradas: manter rotina e testar restore; substituir estratégia após medir requisitos
- Decisão observada: manter rotina enquanto se verifica restauração
- Motivo: motivo não registrado
- Consequências: backup não conta como recuperação comprovada até restore seguro
- Como verificar: restaurar cópia em ambiente isolado e documentar integridade/data
- Pontos fracos: retenção e RPO/RTO ainda não foram confirmados
```

## H5–H6 — proteção e plano incremental

1. Antes de refatorar movimentação: teste de caracterização que registra saldo e evento atuais, inclusive qualquer comportamento desconhecido.
2. Teste de restauração do backup mais recente em instância isolada, sem substituir produção; evidência e data no Registro/Operação.
3. Confirme homologação redigida/segura, integração de teste e processo de rollback antes de qualquer mudança.
4. Instrumente uma falha/retry com contexto mínimo e privacidade; investigue duplicação versus erro transitório.
5. Só então proponha refatoração pequena do fluxo de saldo, protegida por teste, separada de nova funcionalidade.

Em `--job`, hipótese que muda a prioridade (por exemplo, impacto financeiro de saldo duplicado) recebe uma pergunta. Em `--learn`, pergunte qual risco a pessoa protegeria primeiro e por quê.

## Operar — nível e próximo passo menor

**Nível sustentado para visibilidade/recuperação no exemplo: abaixo do nível 1 (nível 0 nesse eixo).** A separação de homologação foi verificada, mas isso sozinho não satisfaz nível 3 (faltam alertas úteis e checagens de dados); backup sem restore e deploy/rollback comprovados não satisfaz nível 2; sem monitor e erros capturados, nível 1 não foi alcançado. A régua mede conjuntos de capacidades, não uma média: registre os controles parciais em vez de esconder a combinação num único número.

**Próximo passo menor recomendado:** planejar e executar, com aprovação, um restore do backup existente em instância isolada e registrar prova. Não toca produção; valida primeiro a promessa de recuperação. Próximo depois disso: monitor externo simples para health/indisponibilidade e coleta de erro redigida. Categorias/ferramentas só após comparar custo e acesso.

| Passo | Status ilustrativo | Próxima evidência |
|---|---|---|
| O1 mapa | parcial, existentes verificados | responsável pelo acesso/deploy; detalhes sensíveis não entram em docs |
| O2 vida | monitor: não existe; F5: relato não verificado | probe externo e erro correlacionável, com privacidade |
| O3 correto | não verificado | checar invariantes de saldo em consulta aprovada e redigida |
| O4 dados | backup existe, não verificado | restaurar isolado e validar conteúdo/tempo |
| O5 mudança | homologação existe e verificada parcialmente | paridade, deploy repetível e rollback |
| O6 incidente | não verificado | roteiro de registrar impacto/linha do tempo sem culpabilizar |

## Exemplo de diário, máximo 10 linhas

### `docs/diario.md`

```markdown
## 2026-10-07 — Herdar/job — ERP fictício
- Mapeei estoque/financeiro; stack declarada TypeScript/Next.js + Postgres/VPS.
- Homologação separada: existe e verificada no escopo descrito.
- Backup diário: existe, restauração não verificada; sem monitor externo confirmado.
- Registros retroativos: [0001 ambientes](decisoes/0001-ambientes-separados.md), [0002 backup](decisoes/0002-backup-diario.md); motivo não registrado.
- Relatos de F5 continuam hipótese de falha intermitente; frequência/causa sem medida.
- Próximo passo proposto: restore isolado do backup, com aprovação e evidência; nada alterado em produção.
```

## Pontos fracos desta proposta (exemplo)

- Restauração isolada ainda não prova todo o plano de continuidade (retenção e tempos alvo faltam).
- Motivos históricos das decisões e frequência/causa do erro F5 seguem não confirmados.
- A próxima mudança provável em unidade de medida/custo pode exigir preservar snapshot histórico; validar com Dono do processo e Dados.
