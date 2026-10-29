# Revisão do “Glossário compartilhado: eu + LLMs”

## Para o Claude

Você propôs o documento `Glossário compartilhado eu + LLMs.md`. Eu o li e concordo com a direção: ele é um **vocabulário de leitura de sistemas**, não um glossário de domínio nem um currículo de TypeScript.

Revise o documento inteiro aplicando as observações abaixo. Preserve o que está forte, elimine ambiguidades, acrescente os termos que faltam e mantenha o texto curto o suficiente para ser colado no início de uma sessão.

Não transforme o glossário em aula de programação. O objetivo é reduzir volume de leitura e melhorar a qualidade das perguntas, não ensinar a linguagem.

---

## O que deve ser preservado

1. **Módulo × componente.** Módulo responde “sobre o quê?”; componente responde “faz o quê tecnicamente?”. Essa distinção está boa.
2. **Caso de uso × fluxo.** Caso de uso é o que o ator quer; fluxo é o caminho concreto no código. Essa é a distinção mais útil do documento.
3. **Ator × entrypoint.** Quem quer e onde o código recebe o disparo são coisas diferentes. Manter.
4. **Estado × efeito colateral.** O que persiste e o que muda no mundo precisam continuar separados.
5. **Decisão arquitetural × ADR × trade-off.** A ligação entre decisão, alternativa, consequência e reversibilidade está boa.
6. **Zona de risco e dívida de compreensão.** São termos que mudam onde você gasta atenção de leitura. Manter.
7. **A sequência de perguntas no fim.** Ela é o melhor parte do documento: transforma o glossário em protocolo.
8. **“Termo + limite de tamanho” e “diga não sei”.** São regras anti-alucinação. Não remova.
9. **“Corrija o termo na hora.”** Termo frouxo gera resposta frouxa. Manter.

---

## Ajustes necessários

### 1. “Definição fechada” ainda não está fechada

Alguns pares se sobrepõem: módulo × camada, fronteira × integração, estado × status, premissa × suposição. Para cada termo ambíguo, acrescente uma linha **“não é”** ou **“quando os dois aparecerem, use esta distinção”**.

Exemplo:

> **Estado persistido** não é o mesmo que **estado de ciclo de vida**. O primeiro é dado que sobrevive à execução; o segundo é a situação de um compromisso (“Em trânsito”, “Recebido”).

### 2. Ator × disparador

O documento coloca “relógio (cron)” como ator. Cron não quer nada; ele **dispara**.

Separe:

- **Ator:** quem tem o objetivo — pessoa, perfil, sistema externo.
- **Disparador:** o que inicia tecnicamente — clique, rota, cron, webhook, fila.
- **Entrypoint:** onde o código recebe o disparo.

A pergunta “quais atores podem disparar esse fluxo?” deve virar “quais atores podem iniciar esse caso de uso, e por quais disparadores/entrypoints?”.

### 3. “Estado” tem dois sentidos

Crie dois termos:

- **Estado persistido:** banco, cache, arquivo, sessão; sobrevive à execução.
- **Estado de ciclo de vida:** situação de um compromisso ou documento; “Em trânsito”, “Recebido”, “Cancelado”.

No ERP, essa diferença é central: a linha persistida pode continuar “Em trânsito” mesmo depois de outro evento “Recebido”. O glossário precisa impedir que a LLM trate os dois como a mesma coisa.

### 4. “Efeito colateral” está amplo demais

Log e métrica são efeitos, mas raramente são o risco principal. Separe:

- **Efeito observável:** resposta, e-mail, evento, tela, arquivo.
- **Efeito irreversível:** dinheiro, estoque, exclusão, permissão, envio externo.
- **Efeito técnico:** log, métrica, cache; importa para operação, mas não é risco de negócio por si só.

A pergunta “algum é irreversível?” está boa. Acrescente “qual efeito o negócio percebe?”.

### 5. Responsabilidade: “sem usar e” é pista, não lei

“Guarda saldo e impede saldo negativo” pode ser uma responsabilidade só. O critério mais forte é: **um motivo para mudar**. O “e” é um cheiro, não uma proibição.

Sugestão:

> **Responsabilidade:** a única razão pela qual aquele pedaço muda. Teste: “se X mudar, quantos arquivos eu mexo?”. Se a frase precisar de “e”, verifique se são dois motivos de mudança; não divida automaticamente.

### 6. Contrato precisa de mais campos

Entrada, saída e erros são o mínimo. Para API, inclua também:

- pré-condições e pós-condições;
- autorização (quem pode chamar);
- idempotência e repetição;
- efeitos colaterais;
- versionamento e compatibilidade.

“Quem pode chamar” é parte do contrato, mesmo quando a resposta é 403.

### 7. Premissa × suposição

São coisas diferentes:

- **Premissa:** algo assumido como verdade para a decisão valer (“o volume será X”).
- **Suposição:** lacuna preenchida para continuar, com status `aberta`, `confirmada` ou `invalidada`.
- **Pergunta em aberto:** falta informação que só o dono do negócio tem.

Acrescente os três. O documento hoje só tem “premissa”.

### 8. “Módulo pode ser entendido sozinho” é forte demais

Em um monolito, um módulo pode ter fronteira clara e ainda depender de outros. Melhor:

> **Módulo:** agrupamento em torno de um assunto de negócio, com fronteira suficiente para ser estudado isoladamente, mesmo que declare dependências.

### 9. “Camada” é lente, não lei

Frameworks modernos misturam apresentação, API e infraestrutura. Mantenha a definição, mas acrescente: “quando o framework misturar camadas, registre a exceção em vez de forçar o modelo”.

### 10. Toda resposta precisa de evidência

A sequência de perguntas deve exigir:

- `arquivo:linha` para o que foi visto no código;
- **confirmado / declarado / hipótese** para o grau de confiança;
- “não sei” quando faltar informação.

Sem isso, a LLM pode preencher lacunas com invenção elegante. O glossário define o vocabulário; a evidência define o que é verdade.

---

## Termos que faltam

Acrescente estes termos ou aponte para onde eles vivem. Não precisa repetir o guia OO inteiro.

| Termo | Definição curta | Pergunta que destrava |
|---|---|---|
| **Disparador** | Evento técnico que inicia o fluxo: clique, rota, cron, webhook, fila. | “O que faz esse fluxo começar?” |
| **Estado persistido** | Dado que sobrevive à execução e muda o comportamento. | “O que esse fluxo lê e grava?” |
| **Estado de ciclo de vida** | Situação de um compromisso/documento (“Em trânsito”, “Recebido”). | “Quais estados são válidos e como se transita?” |
| **Política** | Regra que o negócio pode ajustar. | “Isso pode mudar por decisão do negócio?” |
| **Invariante** | Regra que sempre precisa ser verdadeira. | “Algum fluxo consegue quebrar isso?” |
| **Evidência** | Trecho, linha ou dado que sustenta uma afirmação. | “Onde está a prova?” |
| **Hipótese** | Explicação provável ainda não testada. | “O que provaria que está errado?” |
| **Suposição** | Lacuna preenchida para continuar, com status. | “Isso foi confirmado ou assumido?” |
| **Fonte da verdade** | Onde a regra vive de fato: spec, código, banco, produção. | “Onde eu confirmo esta regra?” |
| **Regra declarada / implementada / verificada** | O que a spec diz, o que o código faz e o que o uso/teste comprova. | “As três versões coincidem?” |
| **Efeito observável / irreversível** | O que o negócio percebe; o que não dá para desfazer. | “Qual efeito é caro de reverter?” |

Também vale incluir, se o projeto usar: **agregado**, **contexto**, **evento**, **DTO**, **idempotência** e **migração**. Se entrarem, cada um com “quando carregar” e link para a referência que aprofunda.

---

## Estrutura sugerida

Para o documento continuar curto e colável:

1. **Núcleo (sempre colar):** sistema, módulo, caso de uso, ator, disparador, entrypoint, fluxo, efeito, evidência.
2. **Leitura de um fluxo:** estado persistido, estado de ciclo de vida, regra, invariante, política, zona de risco.
3. **Decisão:** decisão arquitetural, ADR, alternativa, trade-off, premissa, suposição, dívida de compreensão.
4. **Perguntas-modelo:** a sequência de 7 passos, agora exigindo `arquivo:linha` e confiança.
5. **O que este glossário não é:** não é glossário de domínio, não é currículo, não é prova de que o código está certo.

---

## Critérios de aceite da revisão

O documento revisado deve:

- manter as distinções e a sequência de perguntas que já estão boas;
- ter uma linha “não é / não confunda” nos termos ambíguos;
- separar ator, disparador e entrypoint;
- separar estado persistido de estado de ciclo de vida;
- separar efeito observável de efeito irreversível;
- incluir evidência, hipótese e suposição com status;
- apontar para o guia OO/contratos em vez de duplicá-lo;
- continuar curto: núcleo colável em uma sessão, extensões sob demanda;
- não inventar termos novos sem uso real.

---

## O que não fazer

- Não transformar o glossário em tutorial de TypeScript.
- Não duplicar o glossário de domínio do ERP.
- Não criar um termo para cada palavra; só entre se ela evita ambiguidade real.
- Não remover “diga não sei” nem a exigência de evidência.
- Não vender o glossário como substituto de teste, leitura ou banco de homologação.

---

## Formato da sua resposta

Devolva:

1. o documento completo revisado;
2. um changelog curto, em 5 a 8 bullets, dizendo o que mudou e por quê;
3. uma lista de pontos que ainda dependem de decisão minha.

Se algo não se encaixar nas definições, avise em vez de inventar um termo novo.
