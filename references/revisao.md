# Modo revisão de código existente

Use quando o usuário pedir para revisar um módulo, fluxo ou contrato existente. Não mova, renomeie nem reescreva arquivos sem aprovação.

## Procedimento

1. Reconstrua a folha a partir do código: campos, referências, dados simples, assinaturas, vida própria e dono.
2. Reconstrua estados e transições dos compromissos; não trate fatos passados como estados reversíveis.
3. Siga uma regra até onde está validada e descubra quem a altera (tela, porta, ação, persistência).
4. Aponte alertas com arquivo, trecho/responsabilidade e motivo. Para cada recomendação, registre trade-off e condição em que o padrão pode ser quebrado.
5. Proponha a mudança em tabela `de → para → por quê (qual pergunta de bolso)`, com risco/reversibilidade quando relevante.
6. Peça aprovação antes de qualquer movimentação. Se aprovado, delimite o escopo e só então faça alterações.

## Sinais herdados e como interpretá-los

| Sinal | Por que merece atenção | Quando pode ser aceitável |
|---|---|---|
| Arquivo genérico que só cresce (`types`, `utils`, `domain`) | responsabilidade e dono ficam difíceis de localizar | utilitário pequeno compartilhado, com escopo explícito e estável |
| Regra de negócio na rota ou na tela | duplicação, bypass por outra entrada, teste difícil | validação de apresentação/contrato pertence à borda, sem decidir política de domínio |
| `PATCH` que troca estado | pode pular transições, autorização e efeitos da ação | estado é um campo diretamente editável sem regra/efeito especial, conforme o domínio |
| Campo de entidade adicionado só porque a tela precisa | mistura modelo persistido e formato de apresentação | o campo também é dado real do domínio, não apenas projeção |
| Tela importa regras, banco ou tipos internos | acopla interface a implementação e dificulta mudança | app pequeno com fronteiras deliberadamente simples; registre como evoluir sem migração custosa |
| `criadaPor`, status ou `id` vêm do corpo do pedido | pode permitir falsificação de autoria/estado ou colisão | dado é explicitamente fornecido pelo usuário com validação e sem confundir com metadado do servidor |
| `regras` importa `acoes` | direção de dependência invertida e ciclos prováveis | abstração comum explicitamente separada pode mudar a topologia; não faça regra depender de fluxo de aplicação por conveniência |
| Arquivo com mais de uma responsabilidade | mudanças independentes se enroscam | agrupamento pequeno pode reduzir cerimônia enquanto as razões de mudança forem as mesmas |

Os sinais são convites a investigar, não veredictos automáticos. Consulte [`trade-offs.md`](trade-offs.md) e aplique o padrão com custos/exceções.

## Exemplo de tabela

| De | Para | Por quê / pergunta de bolso | Trade-off e reversibilidade |
|---|---|---|---|
| `rota.ts` valida saldo e grava movimentação | regra em `estoque/regras` e coordenação em `requisicao/acoes` | “Qual folha muda se a regra de saldo mudar?” | mais módulos; reduz bypass por outras entradas. Duas vias se contrato não publicado. |

Na entrega, liste fatos observados separados de suposições e perguntas. Não invente o comportamento que o código não revela.
