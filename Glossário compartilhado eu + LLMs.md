# Glossário compartilhado: eu + LLMs

Oct 28, 2026 · @processosdrenesse

## Como usar

Este é um **vocabulário de leitura de sistemas**: define o que perguntar e como a resposta deve vir. O vocabulário diz do que estamos falando; a evidência diz o que é verdade.

- **Cole o Núcleo em toda sessão.** As outras seções entram sob demanda, quando o assunto aparecer.
- **Termo + limite de tamanho.** Ex.: "Liste os **fluxos** do módulo Financeiro, um por linha, com o **entrypoint** de cada."
- **Corrija o termo na hora.** Se a LLM usar um termo fora da definição, corrija antes de seguir. Termo frouxo gera resposta frouxa.
- **Toda afirmação sobre o código traz evidência:** `arquivo:linha` e um grau de confiança (abaixo).
- **"Não sei" é resposta válida.** Melhor que uma lacuna preenchida com invenção elegante.
- **Termo novo só entra** se apareceu duas vezes em projeto real e evita uma ambiguidade concreta.

**Grau de confiança** (obrigatório em cada afirmação):

| Marca | Significa |
| --- | --- |
| **confirmado** | Visto no código, teste ou dado, com `arquivo:linha` ou consulta. |
| **declarado** | Dito em spec, ADR, comentário ou pela pessoa, mas não verificado no código. |
| **hipótese** | Inferência provável, ainda não testada. |

## 1. Núcleo (sempre colar)

Um **ator** quer um **caso de uso**; um **disparador** o inicia; o código o recebe num **entrypoint**; o **fluxo** é o caminho percorrido; os **efeitos** são o que muda no mundo; a **evidência** prova cada passo.

| Termo | Definição | Pergunta que destrava |
| --- | --- | --- |
| **Sistema** | Tudo o que é implantado e operado junto para um propósito de negócio (ex.: ERP DreneHub). | "Propósito em uma frase e quem usa?" |
| **Módulo** | Agrupamento em torno de um **assunto de negócio** (Financeiro, Estoque), com fronteira suficiente para ser estudado isoladamente, mesmo que declare dependências. Responde "sobre o quê?". **Não é** camada: módulo corta por assunto, camada por tipo de trabalho. | "Liste os módulos com uma frase de propósito cada." |
| **Componente** | Peça técnica dentro de um módulo: controller, service, repository, job, client. Responde "faz o quê tecnicamente?". **Não é** módulo: `UserService` é componente. | "Quais componentes o módulo X tem e o papel de cada?" |
| **Caso de uso** | O que um ator quer conseguir, em linguagem de negócio: verbo + objeto ("Emitir boleto"). **Não é** fluxo. | "Liste os casos de uso do módulo X." |
| **Ator** | Quem tem o objetivo: pessoa, perfil de acesso ou sistema externo. **Não é** disparador: cron não quer nada. | "Quais atores podem iniciar esse caso de uso, e por quais disparadores/entrypoints?" |
| **Disparador** | Evento técnico que inicia o fluxo: clique, requisição, cron, webhook, mensagem de fila. | "O que faz esse fluxo começar?" |
| **Entrypoint** | Onde o código recebe o disparo: rota HTTP, comando, handler de job, consumidor de fila, handler de evento na UI. | "Liste os entrypoints e o caso de uso de cada." |
| **Fluxo** | Caminho concreto do código para realizar **um** caso de uso, do entrypoint ao último efeito, incluindo continuação assíncrona quando o caso de uso só termina depois. | "Trace o fluxo de X, um passo por linha: componente → ação, com `arquivo:linha`." |
| **Efeito observável** | O que o negócio ou o usuário percebe: resposta, tela, e-mail, arquivo, evento publicado. | "Qual efeito o negócio percebe?" |
| **Efeito irreversível** | O que não dá para desfazer sem custo: dinheiro, estoque, exclusão, permissão, envio externo. | "Algum efeito é irreversível? Qual o custo de reverter?" |
| **Efeito técnico** | Log, métrica, cache. Importa para operação, mas **não é** risco de negócio por si só. | Só pergunte se o problema for operacional. |
| **Evidência** | Trecho, `arquivo:linha`, teste ou dado que sustenta uma afirmação. Vem com grau: confirmado, declarado ou hipótese. | "Onde está a prova?" |

## 2. Leitura de um fluxo (colar ao analisar um fluxo)

Estes termos dizem o que o fluxo lê, o que ele protege e onde vale ler linha a linha.

| Termo | Definição | Pergunta que destrava |
| --- | --- | --- |
| **Estado persistido** | Dado que sobrevive à execução: banco, cache, arquivo, sessão. **Não é** estado de ciclo de vida. | "O que esse fluxo lê e grava?" |
| **Estado de ciclo de vida** | Situação de um compromisso ou documento ("Em trânsito", "Recebido", "Cancelado"), com transições permitidas. Pode divergir do registro persistido: a linha pode seguir "Em trânsito" depois de um evento "Recebido". | "Quais estados são válidos, como se transita e onde cada transição é gravada?" |
| **Regra de negócio** | Decisão que vem do negócio, não da técnica. Ou é política, ou é invariante. | "Quais regras esse fluxo aplica e onde cada uma está?" |
| **Política** | Regra que o negócio pode ajustar ("desconto máximo de 10% sem aprovação"). | "Isso pode mudar por decisão do negócio?" |
| **Invariante** | Regra que precisa ser verdadeira sempre, em qualquer fluxo ("saldo nunca negativo"). **Não é** política: não se negocia. | "Algum fluxo consegue quebrar isso?" |
| **Regra declarada / implementada / verificada** | O que a spec diz / o que o código faz / o que teste ou uso comprova. | "As três versões coincidem? Onde divergem?" |
| **Fonte da verdade** | Onde a regra ou o dado vive de fato: spec, código, banco ou produção (se autorizado). | "Onde eu confirmo esta regra?" |
| **Zona de risco** | Trecho onde erro custa caro: permissão, dinheiro/fiscal, escrita em dados, concorrência, integrações, efeitos irreversíveis. **Só aqui se lê linha a linha.** | "Quais trechos desse diff tocam zonas de risco?" |

## 3. Estrutura e design (sob demanda)

Cole quando a pergunta for sobre como as peças se ligam ou dividem o trabalho. Responsabilidade e contrato são aprofundados no [guia OO e contratos de API](https://github.com/yurilimadev/skill-guia-oo-contratos-api); aqui fica só a definição de trabalho.

| Termo | Definição | Pergunta que destrava |
| --- | --- | --- |
| **Camada** | Lente: faixa com o mesmo tipo de trabalho em todos os módulos (API, aplicação, domínio, infraestrutura). Quando o framework misturar camadas, registre a exceção em vez de forçar o modelo. | "Alguma camada chama outra que não deveria?" |
| **Dependência** | A precisa de B para funcionar (importa, chama, lê dados). Tem direção: A → B. | "Desenhe as dependências entre módulos. Existe ciclo?" |
| **Fronteira** | Ponto onde o sistema toca algo fora do seu núcleo: usuário, fila, arquivo, outro sistema ou a persistência (banco). Banco controlado é fronteira de persistência; não é automaticamente integração. | "Liste as fronteiras e o que entra/sai por cada uma." |
| **Integração** | Fronteira com **outro sistema** (NF-e, n8n, gateway, banco externo/gerenciado). Toda integração é fronteira; nem toda fronteira é integração. Banco interno não vira integração só por existir. | "O que acontece se essa integração cair ou responder duas vezes?" |
| **Responsabilidade** | A razão pela qual aquele pedaço muda. Teste: "se X mudar, quantos arquivos eu mexo?". Um "e" na frase é pista, não lei: verifique se são dois motivos de mudança antes de dividir. | "Por que motivo esse componente mudaria? Há mais de um?" |
| **Contrato** | Acordo público de uma API ou componente: entrada, saída, erros, pré e pós-condições, autorização (quem pode chamar, inclusive o 403), idempotência, efeitos e versionamento. | "Mostre o contrato em tabela, com `arquivo:linha` de cada item. O que está implícito?" |
| **Acoplamento / coesão** | Acoplamento: quanto uma peça precisa saber de outra. Coesão: quanto o que está dentro muda pelo mesmo motivo. | "Se eu mudar X, o que mais quebra?" |

## 4. Decisão e incerteza (colar ao escrever ADR)

Se a pergunta "por que foi feito assim?" vai aparecer daqui a 6 meses, é decisão arquitetural e merece ADR.

| Termo | Definição | Pergunta que destrava |
| --- | --- | --- |
| **Decisão arquitetural** | Escolha cara de reverter, que afeta vários módulos ou a operação (banco, sync vs. fila, monolito vs. serviços). Nome de variável não é. | "Liste as decisões implícitas neste diff, com o custo de reverter cada uma." |
| **ADR** | Registro de **uma** decisão: Contexto, Decisão, Alternativas, Consequências. Uma página. | "Rascunhe a ADR. Deixe em branco o que você não sabe." |
| **Alternativa** | Opção considerada e descartada. Sem ela, a ADR não explica o porquê. | "Que outras 2 opções existiam e por que perderam?" |
| **Trade-off** | O que se ganha e o que se paga. Se não aparecer, falta análise. | "O que fica pior com essa decisão?" |
| **Premissa** | Algo assumido como verdade para a decisão valer ("o volume será X"). Se mudar, revisite a ADR. **Não é** suposição. | "Que premissas sustentam essa decisão?" |
| **Suposição** | Lacuna preenchida para poder continuar a análise. Sempre com status: `aberta`, `confirmada` ou `invalidada`. | "Isso foi confirmado ou assumido?" |
| **Hipótese** | Explicação provável de um comportamento, ainda não testada. | "O que provaria que está errada?" |
| **Pergunta em aberto** | Falta de informação que só o dono do negócio pode resolver. Não se responde lendo código. | "Quem precisa responder isso?" |
| **Dívida de compreensão** | Parte que funciona mas ninguém do time consegue explicar. Cresce quando se aceita código de IA sem entender. | "O que neste diff eu não conseguiria explicar a outra pessoa?" |

## 5. Perguntas-modelo, na ordem de uso

Protocolo para entender um sistema novo ou um diff grande. Em todos os passos vale a regra: `arquivo:linha` + confirmado/declarado/hipótese, ou "não sei".

1. **Visão geral:** "Em até 10 linhas: propósito do sistema, módulos (uma frase cada) e integrações."
2. **Casos de uso:** "Tabela do módulo X: caso de uso · ator · disparador · entrypoint (`arquivo:linha`)."
3. **Um fluxo:** "Trace o fluxo de \[caso de uso\], um passo por linha: componente → ação → `arquivo:linha`. Marque efeitos observáveis com ○ e irreversíveis com ⚠."
4. **Estado e regras:** "Que estado persistido o fluxo lê e grava? Que estados de ciclo de vida ele muda? Quais políticas e invariantes aplica? Separe declarado de implementado."
5. **Risco:** "Quais trechos tocam zonas de risco? Só esses eu leio linha a linha."
6. **Teste de compreensão:** "Vou explicar o fluxo com minhas palavras. Aponte só o que está errado ou faltando, com evidência."
7. **Decisões:** "Liste as decisões arquiteturais implícitas, com alternativa, trade-off e premissas. Suposições com status. Depois rascunhe ADR só das que eu aprovar."

**Frase de abertura para colar no prompt:**

> Use este glossário com estas definições exatas: \[colar Núcleo + seções necessárias\]. Responda curto, no formato pedido. Toda afirmação sobre o código vem com `arquivo:linha` e grau (confirmado, declarado, hipótese). Se faltar informação, diga "não sei". Se algo não se encaixar nas definições, avise em vez de inventar um termo.

## O que este glossário não é

- **Não é glossário de domínio.** Termos do ERP (pedido, NF, romaneio) ficam no glossário do projeto.
- **Não é currículo de programação.** Não ensina TypeScript nem padrões; para OO e contratos, use o guia.
- **Não é prova de que o código está certo.** Ele melhora as perguntas; quem prova é teste, leitura das zonas de risco e banco de homologação.
