# Da Folha ao Código — Full Cycle

Skill em português para construir, herdar e operar sistemas. Ajuda a transformar domínio em decisões, descobrir objetos e responsabilidades pela metáfora da pasta sanfonada, visualizar interações com Mermaid, entender o que já existe, proteger mudanças em produção e criticar propostas de IA. O método mantém a metáfora do papel e as sete etapas originais dentro do fluxo de Construir.

## Instalação, atualização e remoção

Requisito: Node.js com `npx`. Os comandos são iguais em Linux, macOS e Windows (PowerShell ou Prompt de Comando). Só muda a citação de `*`: `'*'` no PowerShell, `"*"` no Prompt.

| Escopo | Vale para | Como |
|---|---|---|
| **Global** | todos os projetos do seu usuário | acrescente `-g` |
| **Projeto** | só o projeto atual | rode na raiz do projeto, sem `-g` |

### Instalar

Global:

```bash
npx skills add yurilimadev/skill-guia-oo-contratos-api -g -s folha-ao-codigo
```

No projeto:

```bash
npx skills add yurilimadev/skill-guia-oo-contratos-api -s folha-ao-codigo
```

Variações úteis: `-y` não pergunta nada; `--all` instala todas as skills do repositório para todos os agentes; `-a '*'` escolhe todos os agentes; `--copy` copia em vez de criar link simbólico.

### Atualizar

```bash
npx skills update folha-ao-codigo -g -y   # global
npx skills update folha-ao-codigo -p -y   # projeto
npx skills update -g -y                   # todas as globais
```

### Remover

```bash
npx skills remove folha-ao-codigo -g -y   # global
npx skills remove folha-ao-codigo -y      # projeto
```

Para um agente específico, acrescente `-a <agente>`. Veja os nomes com `npx skills list -g`.

### Conferir

```bash
npx skills list -g   # globais
npx skills list      # do projeto
```

### Testar um clone ou branch

Para usar uma versão ainda não publicada, instale a partir do clone local:

```bash
npx skills add /caminho/do/clone -g -s folha-ao-codigo --copy -y
```

O escopo passa a apontar para o caminho local. Depois do teste, reinstale a partir do GitHub para voltar à versão publicada.

### Onde fica

- Global: `~/.agents/skills/folha-ao-codigo` (Windows: `%USERPROFILE%\.agents\skills\folha-ao-codigo`).
- No projeto: `.agents/skills/` ou `.opencode/skills/`, conforme o agente. `npx skills list` mostra o caminho real.

## As três entradas

- **Construir** — modelar funcionalidade nova com etapas 0–10, lente de domínio, teste de estresse e compromisso de retorno em produção.
- **Herdar** — fazer Raio-X de sistema que já existe, mesmo que tenha sido feito por você; só leitura por padrão, evidências e refatoração incremental.
- **Operar** — medir saúde, correção, segurança, backup e mudança segura sem executar comandos em produção sem autorização.

`--learn` (aprender fazendo) e `--job` (entregar com raciocínio) funcionam nas três entradas. A lente de crítica, os arreios e a documentação viva acompanham qualquer caminho.

## Como começar

Informe entrada, modo, linguagem do projeto (alvo) e a linguagem que domina melhor (referência). Se o projeto estiver aberto, a skill sugere o que detectou e pede confirmação. A referência pode ser trocada durante a conversa.

```text
Entrada: Construir · --learn
Linguagem alvo: TypeScript do projeto
Linguagem de referência: Python
Quero modelar a divergência no recebimento de estoque. Hoje a equipe confere itens
e anota diferenças no papel. Quero descobrir o que deve ser registrado antes de
escrever código. Ajude-me a separar o ator humano dos objetos e responsabilidades
do software. Se ajudar, desenhe um diagrama Mermaid do fluxo; não transforme cada
substantivo em classe. Conduza uma etapa por vez.
```

Para pedir um desenho direto, acrescente, por exemplo: “Gere um `sequenceDiagram` Mermaid do fluxo de recebimento; mostre o estoquista como ator separado das responsabilidades do software e marque o que ainda é hipótese.” Em `--learn`, a skill pode ajudar você a escolher atores, objetos e setas antes de completar o desenho.

```text
Entrada: Herdar · --job
Alvo: TypeScript/Next.js · Referência: Python
Herdei o módulo de estoque. Faça um inventário e trace o fluxo de registrar uma
entrada. Só leitura; cite arquivo:linha e separe fato de hipótese.
```

```text
Entrada: Operar · --learn
Alvo: [linguagem/stack] · Referência: Python
Às vezes uma requisição falha e volta a funcionar com F5. Ajude-me a descobrir
que evidência falta e qual pergunta fazer primeiro; não altere produção.
```

Se ainda não souber qual entrada escolher, descreva o problema: a skill sugere a entrada e pede confirmação. Durante a sessão, diga “muda para Herdar” ou use a flag correspondente para trocar modo; decisões e evidências continuam registradas.

## Documentação e segurança

O ponto de partida pode ser qualquer etapa e qualquer estado do projeto. Se `docs/` existir, a skill lê o diário, operação e decisões relevantes antes de perguntar; documentos vivos são atualizados, não recriados. A skill oferece escrever no projeto, mas só faz isso com permissão.

Herdar é somente leitura por padrão. Operar não executa comandos nem muda DNS, servidor ou banco sem aprovação explícita. Toda evidência distingue “não existe”, “existe, não verificado” e “existe e verificado”.

## Mudanças — versão 3.3.0

- Perguntas norteadoras (`references/perguntas-norteadoras.md`) ligam consulta, hierarquia e modelo, sem numeração. Em `--learn`, consulta antes do desenho; em `--job`, desenho com hipóteses marcadas e teste depois.
- Hipóteses no desenho: arestas tracejadas e tabela na Ficha, com status `hipótese`, `testada` ou `refutada`.
- Glossário: molde (folha em branco), ocorrência (folha preenchida), documento (o que se consulta e muda de estado).
- Herdar: spec em PDF entra como declarada; código como implementado; uso ou teste como verificado.
- Acrescenta a metáfora da pasta sanfonada para armário, pasta, divisória, documento, folhas, campos, grampos, referências e verso.
- Diferencia ator humano, responsabilidade do software, objetos/classes, persistência e objetos transitórios.

## Mudanças — versão 3.2.0

- Razão escrita pela pessoa antes da crítica da skill, no cartão e nos modos.
- Novo `references/socratico.md`: perguntas graduais, checagem de entendimento e separação de assuntos técnicos e teóricos.
- Novo `references/decisoes.md`: mapa de decisões de modelagem e organização, com dependências de ciclo de vida, direção de importações e razões de mudança.
- Data e hora nos registros: diário, decisões e cartões.

## Mudanças — versão 3.1.0

- Explica objetos, classes/instâncias, atores humanos e responsabilidades do software; diferencia objetos persistidos de controles transitórios.
- Exercita a descoberta de objetos em `--learn` sem transformar cada substantivo em classe.
- Oferece diagramas Mermaid em `--learn` e `--job` quando forem pedidos ou ajudarem a entender interações.

## Mudanças — versão 3.0.0

- Adiciona entradas Construir, Herdar e Operar; preserva as etapas 0–10 e a revisão anterior.
- Acrescenta lente de domínio, Raio-X, evidências de operação, arreios e crítica obrigatória.
- Cria documentação viva, registros retroativos, chapéus e Mapa de envolvidos.
- Amplia a ponte de linguagens para TypeScript, Python, Java, C#, Go e PHP.

## Créditos

Algumas práticas de linguagem simples, dimensionamento, registro de suposições/perguntas, critérios observáveis e carregamento progressivo foram adaptadas com crédito à skill [`onp-spec-driven`](https://github.com/onovoprogramador/onp-spec-driven), de Vitor Manoel — O Novo Programador, disponibilizada sob MIT. O texto, as entradas e o método de modelagem foram escritos para Da Folha ao Código; nenhum script ou motor da referência foi copiado.
