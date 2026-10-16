# Entrada Herdar — Raio-X de sistema existente

Use para sistema feito por outra pessoa **ou por você no passado**. Pode ser TypeScript/ERP, app pequeno ou sistema com melhorias parciais. Consulte `references/revisao.md` como mapa da versão anterior: seu conteúdo está incorporado aqui. Em `--learn`, conduza a exploração por perguntas; em `--job`, apresente diagnóstico e próximo passo com base em evidência. Pergunte somente sobre hipóteses que possam mudar prioridade, frequência ou risco; registre as demais como abertas.

## Regras de investigação

- **Somente leitura por padrão.** Não altere código, banco ou configuração sem pedido explícito. Primeiro entenda o comportamento atual; qualquer ação de produção pertence à entrada Operar e exige autorização.
- Ao citar código, indique `arquivo:linha`. Separe **confirmado** (visto no código/config/dado) de **hipótese** (inferido). “Não achei” não prova que não existe; documente o escopo pesquisado.
- Para inventário e itens operacionais, classifique a evidência: **não existe**, **existe, não verificado** ou **existe e verificado**. Registre evidência concreta e data quando existir.
- Se `docs/` existir, leia diário, operação e decisões relevantes antes de perguntar (`references/documentacao.md`). Registre decisões anteriores como retroativas; motivo desconhecido = “motivo não registrado”.
- Não re-proponha um controle/decisão que já existe e foi verificado e atende ao critério; cite a evidência, identifique apenas as lacunas e siga para o próximo passo real.

## H1–H6

| Passo | Nome | Trabalho | Produz | Chapéus típicos e por quê |
|---|---|---|---|---|
| H1 | Inventário | Levante stack, pastas, rotas, telas, tabelas/migrações, integrações, ambientes e deploy, sem julgar | Mapa do terreno | quem conhece o sistema, Suporte e Usuário-chave: confrontar repositório com uso real |
| H2 | Domínio reverso | Reconstrua folhas/glossário a partir de tabelas, tipos e rotas; encontre compromissos, fatos, status e contextos escondidos | Mapa do domínio | Domínio, Dados e Dono do processo: alinhar nomes/campos com regras atuais |
| H3 | Fluxo de dados | Trace 2–3 fluxos críticos da tela até o banco; aponte onde a regra realmente mora | Diagramas com `arquivo:linha` | Backend, Frontend/UX e Usuário-chave: ligar execução técnica ao fluxo usado |
| H4 | Diagnóstico | Avalie sinais de alerta e classifique risco × frequência: “dívida que dói” ou “feia mas inofensiva” | Lista priorizada com evidência e hipóteses | Domínio, QA e Suporte: separar risco de estética e relato de recorrência |
| H5 | Rede de proteção | Antes de refatorar, planeje testes de caracterização, backup restaurável e homologação adequada | Pré-condições para mudança segura | QA, Dados e Infra: capturar comportamento e recuperação antes de alterar |
| H6 | Plano incremental | Divida refatoração em passos pequenos, reversíveis e ordenados por risco; cada passo recebe Cartão e Brief | Plano e Briefs | Domínio, QA e Infra: confirmar fronteiras, prova e rollback |

### H1 — Inventário

Leia o repositório sem modificá-lo. Registre caminhos e versões observáveis; não deduza existência de ambientes, backup, log ou monitoramento por nomes de arquivos. Pergunte ao usuário onde há recursos que não ficam no repositório, como painel do provedor, DNS e processo de deploy; marque a resposta como declarada até conferir evidência.

### H2 — Domínio reverso

Reconstrua armário/pastas/documentos/folhas, possíveis objetos/responsabilidades e a linguagem real a partir das estruturas e fluxos encontrados. Procure conceitos que o código esconde: um status espalhado em strings, muitos booleanos, relação 1→N não representada, valor histórico referenciado onde deveria ser copiado, número sem unidade, pessoa confundida com papel, ou classe que só existe porque um ator humano tem o mesmo nome. Uma linha/tabela pode ser evidência de persistência, mas não prova uma classe um-para-um. Use `references/dominio.md` e `references/objetos.md`; não imponha remodelagem por estilo.

### H3 — Fluxo de dados

Escolha 2 ou 3 fluxos importantes para o negócio. Para cada um, trace tela → entrada/rota → regra/caso de uso → integração/persistência → resposta. Marque `arquivo:linha` para cada ponto confirmado e escreva “hipótese” quando a regra só puder ser inferida. Mostre onde a regra de fato mora, que outras entradas podem contorná-la e, se o desenho ajudar, ofereça um `sequenceDiagram` Mermaid sem apresentar uma hipótese como fato.

### H4 — Diagnóstico

Cada achado inclui evidência, risco e frequência provável, impacto e condição de aceitação. Classifique como dívida que dói ou feiura inofensiva; priorize impacto no negócio e probabilidade, não gosto pessoal. Separe fatos, suposições e perguntas abertas. Use a crítica em `references/critica.md`.

### Sinais de alerta preservados da revisão anterior

São convites para investigar, não veredictos. Para cada alerta, indique arquivo/linha, motivo e condição em que a alternativa pode ser aceitável.

| Sinal | Por que merece atenção | Quando pode ser aceitável |
|---|---|---|
| Arquivo genérico que só cresce (`types`, `utils`, `domain`) | dono e responsabilidade ficam difíceis de localizar | utilitário pequeno compartilhado, com escopo estável e explícito |
| Regra de negócio na rota ou na tela | outra entrada pode contornar a regra; teste fica acoplado à borda | validação de apresentação/contrato, sem decidir política de domínio |
| `PATCH` que troca estado | pode pular transição, autorização e efeitos de ação | status é campo diretamente editável sem regra ou efeito especial no domínio |
| Campo na entidade só porque a tela precisa | mistura armazenamento e apresentação | campo também é dado verdadeiro do domínio, não projeção |
| Tela importa regras, persistência ou tipos internos | acopla interface a implementação e dificulta mudança | app pequeno com fronteiras simples e gatilho de evolução registrado |
| `criadaPor`, status ou `id` vêm do corpo | pode falsificar autoria/estado ou colidir identificadores | é dado explicitamente fornecido pelo usuário e validado, sem confundir com metadado do servidor |
| `regras` depende de `acoes` | inverte dependência e pode gerar ciclo | abstração compartilhada separada justifica topologia; regra não deve depender do fluxo só por conveniência |
| Arquivo tem mais de uma responsabilidade | razões de mudança independentes se enroscam | agrupamento pequeno evita cerimônia enquanto as razões de mudança são as mesmas |

Não proponha mover arquivo só porque aparece um sinal. Use risco, frequência, custo, contexto e reversibilidade.

### H5 — Rede de proteção

Antes de refatoração, combine: testes de caracterização que registrem o que o sistema faz hoje (inclusive comportamento surpreendente), restauração de backup testada fora da produção e ambiente de homologação representativo. Se algum item não existir/verificar, encaminhe-o à entrada Operar; não altere produção para criar evidência.

### H6 — Plano incremental

Proponha a menor mudança útil, uma por vez, com dependências, risco, critério de pronto, teste, rollback e Cartão de Decisão. O padrão é **refatorar aos poucos**, não reescrever tudo. Reescrita completa só entra como opção quando a análise mostrar custo total menor, risco controlado, migração e convivência; registre o custo de seguir e de não seguir. Não execute o plano até o usuário pedir.

## Entrega e conteúdo preservado da revisão anterior

Preencha [`../../templates/raio-x.md`](../../templates/raio-x.md): inventário, folhas/estados, fluxos, diagnóstico priorizado, rede de proteção, plano, suposições e perguntas. Preserva os procedimentos antigos: reconstruir a folha e os estados; apontar alertas por arquivo e motivo; propor tabela `de → para → por quê`; não mover arquivo sem aprovação. Os sinais antigos estão preservados acima; `references/revisao.md` fica como atalho compatível para esta entrada.

Formato de proposta de movimentação preservado:

| De | Para | Por quê / pergunta de bolso | Trade-off e reversibilidade |
|---|---|---|---|
| `rota.ts` valida saldo e grava movimentação | `estoque/regras` e `requisicao/acoes` | “Qual folha muda se a regra de saldo mudar?” | mais módulos; reduz bypass. Duas vias antes de publicar o contrato. |

Ao fechar o bloco, diga o que registrar em `docs/raio-x.md`, `docs/dominio/` e diário; ofereça atualizar documentos existentes mediante permissão. Mapeie chapéus e perguntas a levar em `templates/mapa-envolvidos.md`.
