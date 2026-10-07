# Trade-offs e Cartão de Decisão

## Critérios para comparar opções

Use apenas os critérios que realmente importam na decisão:

- simplicidade agora;
- custo de mudar depois;
- consistência dos dados;
- segurança e privacidade;
- desempenho necessário (não hipotético);
- tempo até entregar;
- quem vai manter (considere time pequeno e nível iniciante);
- operação, observabilidade e recuperação quando forem relevantes.

Explique ganho e custo de cada opção em linguagem simples. A solução recomendada não é “a melhor em absoluto”; diga em qual contexto ela vence.

## Reversibilidade

- **Porta de duas vias**: barato desfazer. Prefira um padrão simples, decida rápido e siga; registre o que faria revisar a decisão.
- **Porta de uma via**: caro desfazer, por exemplo esquema de banco difícil de migrar, contrato público já consumido, dinheiro ou dados pessoais. Compare opções com calma, registre custos e confirme com o dono do produto.

Rótulo da porta não substitui análise: confirme impacto, alcance, migração e recuperação. Para cada decisão, use [`../templates/cartao-decisao.md`](../templates/cartao-decisao.md).

## Catálogo de decisões recorrentes

Cada linha é um ponto de partida, não prescrição universal.

### 1. Pastas por substantivo vs. por camada

- **Padrão:** por substantivo/domínio quando cada parte tem regras e muda por razões próprias.
- **Quando quebrar:** aplicação muito pequena, organização existente estável ou camadas técnicas realmente compartilhadas podem favorecer pastas por camada.
- **Custo de seguir o padrão:** referências entre módulos podem ser menos óbvias no início; precisa de limites claros.
- **Custo de não seguir:** mudança de negócio pode exigir caça em pastas genéricas por camada; risco de módulos misturarem responsabilidades.

### 2. DTO separado vs. reutilizar tipo interno

- **Padrão:** DTO separado na fronteira, sobretudo para entrada/saída pública, campos derivados, validação ou evolução independente.
- **Quando quebrar:** protótipo ou CRUD pequeno em que o tipo é exatamente o contrato, não expõe campos internos e não há risco de mudança independente.
- **Custo de seguir:** tipos e tradução adicionais.
- **Custo de não seguir:** contrato público acoplado ao armazenamento/modelo; campos internos podem vazar ou mudanças internas quebrar clientes.

### 3. Ação de negócio como sub-recurso (`POST .../atendimento`) vs. `PATCH` de status

- **Padrão:** POST representando a ação quando há validação, autorização, efeitos colaterais ou criação de fato.
- **Quando quebrar:** alteração simples de atributo editável, sem transição de negócio especial, pode ser PATCH.
- **Custo de seguir:** mais rotas e nomes para documentar.
- **Custo de não seguir:** mudança de estado pode pular regras, registrar incompletamente efeitos e dificultar idempotência/auditoria.

### 4. Regra na entidade vs. na ação

- **Padrão:** regra intrínseca ao ciclo de vida junto do modelo/entidade; coordenação de vários módulos na ação.
- **Quando quebrar:** regra de política transversal pode morar em serviço/política compartilhada; modelo anêmico ou persistência específica pode alterar a fronteira.
- **Custo de seguir:** objetos de domínio podem exigir mais modelagem.
- **Custo de não seguir:** regra espalhada, duplicada ou acoplada a uma rota/tela; cuidado para não concentrar orquestração em uma entidade.

### 5. Validar na tela vs. na porta de entrada (ou em ambos)

- **Padrão:** tela dá feedback cedo; porta de entrada valida de novo como fronteira autoritativa.
- **Quando quebrar:** validação exclusivamente visual só serve para formato/conveniência não relacionado à integridade; a porta continua protegendo regras e dados.
- **Custo de seguir em ambos:** regra de formato simples pode ser repetida; mantenha contrato/testes para reduzir divergência.
- **Custo de validar só na tela:** outras telas, scripts ou chamadas diretas podem gravar dados inválidos ou contornar permissão.

### 6. Função vs. classe

- **Padrão:** função para transformação ou ação simples, explícita e sem estado duradouro.
- **Quando quebrar:** classe pode ajudar quando há estado com ciclo de vida, várias implementações intercambiáveis ou dependências que precisam ser encapsuladas.
- **Custo de seguir:** funções podem receber vários parâmetros ou repetir contexto.
- **Custo de não seguir:** classes e hierarquias podem adicionar indireção, estado implícito e cerimônia sem benefício.

### 7. Banco relacional vs. documento

- **Padrão:** relacional quando relações, integridade referencial, transações e consultas cruzadas são centrais.
- **Quando quebrar:** documento pode se ajustar melhor quando agregado é lido/escrito como unidade e esquema flexível é vantagem real.
- **Custo de relacional:** migrações e modelagem explícita de relações.
- **Custo de documento:** relações e consistência entre documentos podem exigir duplicação, reconciliação e lógica adicional.

### 8. Síncrono vs. fila/job em segundo plano

- **Padrão:** síncrono para trabalho curto que precisa de resultado imediato.
- **Quando quebrar:** fila/job quando trabalho é demorado, precisa de retentativa, tolera resposta posterior ou há pico de volume.
- **Custo de síncrono:** latência e dependência da disponibilidade de tudo na chamada.
- **Custo de fila:** operação, monitoramento, estado eventual, idempotência e experiência de usuário assíncrona.

### 9. Criar abstração agora vs. esperar a terceira repetição (YAGNI)

- **Padrão:** mantenha explícito até a repetição revelar uma forma estável; a terceira ocorrência é um sinal útil, não uma lei.
- **Quando quebrar:** segurança, contrato irreversível, requisito comum comprovado ou custo alto de migração pode justificar uma abstração antes.
- **Custo de esperar:** duplicação temporária e refatoração depois.
- **Custo de abstrair cedo:** abstração baseada em suposição, API difícil de desfazer e custo de aprendizagem.

### 10. Biblioteca pronta vs. código próprio

- **Padrão:** biblioteca mantida para problema complexo e comum, se licença, segurança, manutenção e compatibilidade servirem.
- **Quando quebrar:** código próprio pequeno e estável pode ser melhor quando dependência traz superfície maior do que o problema.
- **Custo da biblioteca:** atualizações, dependências transitivas, curva de uso e risco de abandono.
- **Custo de código próprio:** manutenção, casos de borda e segurança ficam por conta do time.

## Transformar afirmações absolutas em padrões

Ao revisar o método, procure “sempre”, “nunca” e “só”. Para escolhas técnicas, converta-os em: **padrão recomendado → quando quebrar → custo de seguir → custo de não seguir**. Preserve como regra de segurança apenas o que realmente for inegociável no contexto e diga por que; não chame uma preferência de lei.

## Diário

Registre opções, escolha, ganho, custo, reversibilidade, gatilho para revisitar e modo em que foi decidida. Em `--learn`, espere a reflexão do usuário antes de apresentar a recomendação; em `--job`, proponha uma opção e o trade-off em 1–2 frases.

## Extensões para domínio, legado e operação

Os padrões 1–10 continuam acima. Use as extensões abaixo quando a entrada e a evidência trouxerem a decisão; não modele riscos hipotéticos só para cobrir o catálogo.

| Decisão | Padrão | Quando quebrar | Custo de seguir | Custo de não seguir |
|---|---|---|---|---|
| 11. Entidade × valor | Dar identidade persistente a algo reconhecido que muda mantendo continuidade; usar valor para descrição sem identidade | Quando o domínio não distingue continuidade ou o registro é rascunho descartável | Mais IDs, relações e decisões de ciclo de vida | Duplicação, histórico sem dono ou edição alterando objetos sem querer |
| 12. Copiar × apontar | Copiar o retrato que precisa permanecer fiel (preço, custo, descrição contratada); apontar para cadastro atual quando o dado deve acompanhar a fonte | Dado é sempre atual e histórico não é necessário; ou cópia cria risco maior | Mais storage e reconciliação entre fonte e snapshot | Relatório passado muda quando o cadastro atual muda |
| 13. Agregado grande × pequeno | Menor fronteira que garante invariantes que precisam mudar juntas | Requisito comprovado exige consistência maior e o custo de concorrência é aceitável | Mais fronteiras, coordenação e testes | Grande causa contenção; pequeno demais permite estados inconsistentes |
| 14. Modelo único × contextos separados | Compartilhar quando termos, regras e ciclo mudam juntos; separar quando a palavra muda de sentido por setor | Contextos têm a mesma semântica e dono de mudança | Traduções e integração entre contextos | Modelo universal ganha campos condicionais e regras contraditórias |
| 15. Modelar futuro agora × esperar | Registrar gatilho provável; modelar agora quando a porta é cara de reabrir | Cenário especulativo ou fácil de migrar; esperar preserva velocidade | Campos/abstrações talvez nunca usados | Migração mais cara se mudança provável vier sem espaço de evolução |
| 16. Estado atual × histórico/eventos | Guardar estado atual; acrescentar histórico/evento quando alguém precisa saber o que ocorreu e quando | Sem necessidade de auditoria, consulta temporal ou reação a eventos | Armazenamento, retenção e consistência dos eventos | Não conseguir explicar mudança, reconstituir saldo ou identificar causa |
| 17. Refatorar aos poucos × reescrever | Refatorar em fatias protegidas por testes de caracterização e rollback | Reescrita tem custo total menor provado, escopo claro e migração/convivência seguras | Manter por um tempo código antigo e novo | Big-bang perde comportamento implícito, alonga prazo e dificulta reversão |
| 18. Teste de caracterização antes × depois | Capturar comportamento atual antes de mexer no legado | Sistema descartável ou cobertura equivalente já verificada | Tempo para registrar comportamento, inclusive bugs conhecidos | Mudança acidental se confunde com correção; usuário pode perder comportamento dependido |
| 19. Corrigir sintoma × causa | Conter risco imediato e investigar causa com evidência em passo distinguível | Causa desconhecida e mitigação segura imediata é necessária | Duas etapas e trabalho de investigação | Patch superficial esconde recorrência; buscar causa antes de conter prolonga dano |
| 20. Serviço gerenciado × hospedar | Serviço gerenciado se reduz operação sem violar custo, privacidade ou controle | Requisito de controle, localização, custo sustentado ou conectividade impede serviço | Dependência do fornecedor, custo recorrente e limites de configuração | Patch, backup e monitoramento ficam sob responsabilidade local |
| 21. Homologação × teste cauteloso em produção | Ambiente isolado e representativo; produção só para comportamento normal com plano aprovado | Não há homologação e prova exige tráfego real; planeje proteções e aprovação explícita | Manter sincronização e detectar diferenças entre ambientes | Testar com dados/usuários reais pode causar dano e dificultar rollback |
| 22. Coletor de erros × logs próprios | Ferramenta mantida se agrupamento/contexto/alerta couberem em privacidade e preço | Volume pequeno, logs existentes bastam ou vendor lock-in é inaceitável | Dependência, custo e configuração de retenção | Equipe mantém parsing, deduplicação, busca, contexto e alertas |
| 23. Alertar tudo × alertas mínimos úteis | Alertar sintomas acionáveis, com responsável e instrução; registrar sinais menos urgentes sem paginar | Um sinal raro tem impacto severo e precisa acionar imediatamente | Falsos positivos geram fadiga e escondem incidentes | Sem alerta crítico, descoberta depende de reclamação ou perda de negócio |

Para itens 17–23, consulte evidência e limites de segurança em Herdar/Operar. “Testar em produção com cuidado” não autoriza executar; exige aprovação explícita conforme `references/entradas/operar.md`.
