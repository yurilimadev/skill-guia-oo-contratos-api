# Lente de domínio — modelar para mudanças reais

Use nas entradas **Construir** e **Herdar**. O objetivo não é aplicar um vocabulário de arquitetura por si só: é revelar decisões que ficariam caras depois que dados e contratos já estivessem em uso. Mantenha a metáfora da folha; introduza termos técnicos entre parênteses e explique em uma linha. Para cada conceito, confira o que é, pergunta de bolso, exemplo, risco de remodelagem e quando a ideia não paga seu custo.

## Conceitos

| Conceito | O que é / metáfora | Pergunta de bolso | Exemplo de clínica/ERP | Remodelagem que previne | Quando não vale aplicar em detalhe |
|---|---|---|---|---|---|
| Linguagem do domínio (glossário) | Dicionário da casa: nomes compartilhados para coisas, ações e estados | “Todo mundo entende a mesma coisa por X?” | “Produto” é algo estocado para Estoque, custo para Financeiro e item oferecido para Agenda | Mesmo nome codifica regras incompatíveis ou uma regra recebe nomes diferentes | Protótipo de um único fluxo com poucas pessoas; mantenha uma lista curta, não um glossário formal |
| Contextos (fronteiras de significado; bounded contexts) | Cada setor pode ter uma ficha própria para a mesma palavra | “A palavra muda de sentido entre setores?” | Produto em Estoque, Financeiro e Agenda pode ter atributos/regras distintos | Um modelo global cresce com campos opcionais e regras condicionais para sentidos diferentes | Sistema simples em que o conceito tem o mesmo significado e muda junto em todas as áreas |
| Identidade × valor (entidade × objeto de valor) | Tem número de protocolo ou só descreve? | “Se eu trocar o conteúdo, ainda é a mesma coisa?” | Cliente/paciente com cadastro persistente tem identidade; endereço ou preço unitário descreve um valor | Dados que mudam são tratados como identidades independentes ou valores são duplicados sem razão | Campo simples sem identidade, comparação própria ou ciclo de vida; não crie classe para cada texto |
| Agregado e raiz (fronteira de consistência) | O que vai grampeado junto; a folha de capa é a porta | “O que precisa estar certo ao mesmo tempo, sempre?” | Atendimento e baixa de saldo talvez precisem confirmar a mesma quantidade atomicamente | Transação grande demais, dado que deveria mudar junto escapa, ou módulos disputam estado | CRUD pequeno sem invariantes compartilhadas; não invente agregado só por seguir DDD |
| Invariante × política | Regra que protege integridade × regra que a direção pode ajustar | “Isso pode mudar por decisão do negócio?” | Saldo não pode ficar negativo (possível invariante); limite de compra por unidade pode ser política | Regra fixa no código impede mudança de negócio, ou política configurável quebra integridade | Não formalize distinção quando não há regra ou chance plausível de mudança |
| Evento de domínio | Carimbo com data que comunica que algo ocorreu | “Que acontecimento alguém precisa saber que ocorreu?” | `ProdutoMovimentado`, `ConsultaAgendada`, `PagamentoRegistrado` | Histórico sobrescrito; consumidores precisam inferir algo que nunca foi registrado | Fluxo sem necessidade de histórico/notificação; um campo atualizado pode bastar |
| Tempo e histórico | A folha copia o valor daquele dia ou aponta para a ficha atual? | “Se a ficha mudar amanhã, a folha antiga muda junto?” | Copiar preço/custo/unidade de medida na venda ou movimentação preserva o que foi praticado | Relatórios antigos mudam ao editar o cadastro atual; impossível explicar valor de venda passada | Dado que por definição é sempre atual, sem auditoria/relatório histórico e com custo de cópia não justificado |
| Cardinalidade (quantos) | Um campo ou uma lista de linhas? | “Pode ser mais de um? Hoje ou em um ano?” | Uma clínica pode abrir várias unidades; uma venda pode conter vários produtos | Campo singular precisa virar lista, migração, reescrita de contrato e telas | Uma ocorrência é uma regra real do negócio e não há cenário plausível de repetição; não use lista “por garantia” |
| Pessoa × papel | A pessoa tem uma ficha; cada papel é outra relação | “A mesma pessoa pode ter mais de um papel?” | Uma cliente também pode ser funcionária; a identidade é uma, os papéis/permissões diferem | Duplicar a pessoa por função, misturar dados de funcionário e cliente ou autorizar pelo tipo de cadastro | Protótipo sem múltiplos papéis ou permissões distintas; comece simples e registre o gatilho para separar |

### Exemplos que merecem atenção

- **Produto por contexto:** descubra se Estoque, Financeiro e Agenda usam o mesmo conceito ou só a mesma palavra; não force um “Produto universal” sem significado compartilhado.
- **Preço/custo histórico:** venda/movimentação tende a guardar um retrato do preço/custo aplicado; apontar apenas para o valor atual pode alterar a leitura do passado.
- **Quantidade e unidade:** “10” sem saber se são comprimidos, caixas, horas ou quilos não é quantidade suficiente para regra ou relatório.
- **Pessoa com mais de um papel:** mantenha identidade e atribuições distintas; pergunte quem pode fazer cada ação, não só qual tela vê.
- **Nova unidade:** verifique cardinalidade, escopo de estoque/dados, permissões e se valores globais passam a variar por unidade.

## Catálogo de remodelagens bruscas

Use como cenários de estresse no fechamento da etapa 5 de Construir ou como perguntas no H2 de Herdar. Cada item contém sintoma, pergunta preventiva, custo de prevenir e custo de corrigir depois.

| Remodelagem | Sintoma | Pergunta que previne | Custo de prevenir agora | Custo de corrigir depois |
|---|---|---|---|---|
| 1 que vira N | coluna `item` vira uma lista em telas/SQL | “Pode haver mais de um hoje ou no ano que vem?” | suportar coleção e validação, mesmo que pequena | migração, endpoints/telas/relatórios e dados existentes |
| Valor deveria ser copiado | venda histórica muda ao trocar cadastro | “Precisamos saber quanto/qual descrição valia naquele evento?” | guardar snapshot mínimo, sem duplicar cadastro inteiro | reconstruir passado sem fonte confiável ou ajustar histórico manualmente |
| Falta de histórico | só há valor atual, sem saber transições anteriores | “Quem precisa explicar o que ocorreu e quando?” | registrar eventos/histórico com retenção clara | não reconstruir causa/linha do tempo; auditoria pode ser impossível |
| Mesma palavra, dois sentidos | `Produto` reúne campos de estoque, finanças e agenda | “Este setor usa a palavra com a mesma regra?” | glossário e fronteira explícitos | separação de modelo, migrações e sincronização entre áreas |
| Status vira entidade | valores têm campos, permissões ou ciclo próprio | “Cada status tem configuração, dono ou dados próprios?” | definir ciclo e extensibilidade se houver necessidade real | trocar enum/strings em código, migração, contrato e relatórios |
| Booleanos se multiplicam | `isPaid`, `isCanceled`, `isPartial` produzem combinações impossíveis | “Quais estados são válidos e como se chega a cada um?” | explicitar transições e validar estado | normalizar dados incoerentes e revisar todos os consumidores |
| Campos opcionais demais | modelo cheio de nulos e condicionais | “Esse campo pertence a todos os tipos/contextos?” | separar variantes ou contexto quando semântica divergir | regras condicionais, validação confusa e compatibilidade difícil |
| Pessoa confundida com papel | cadastro duplicado para cliente e funcionária | “A mesma pessoa pode vestir mais de um papel?” | relação papel-permissão simples | deduplicar identidades, unir histórico e corrigir auditoria |
| Valor fixo vira cadastro | valor aparece em muitos `if`/selects | “Quem muda esse valor, com que frequência, e ele tem dados próprios?” | parametrizar apenas o que tem dono/ciclo de mudança | deploy para mudanças operacionais ou configuração duplicada |
| Número sem unidade | saldo/tempo parece válido, mas escala não é clara | “10 de quê? Quem converte e qual precisão vale?” | registrar unidade e regra de conversão | recalcular dados, corrigir relatórios e negociar significado com clientes |

Um cenário plausível não obriga a modelar o futuro agora. Compare custo e reversibilidade; solução simples pode ser correta se o gatilho de revisão estiver registrado.
