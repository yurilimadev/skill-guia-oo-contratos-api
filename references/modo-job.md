# Modo `--job` — entregar com raciocínio

## Objetivo

Decidir e preparar uma entrega com rapidez, preservando o porquê das escolhas. A skill propõe e argumenta; o usuário escreve a própria razão e valida as decisões que lhe pertencem.

## Ritmo por etapa

1. Comece a resposta pela linha de estado definida em `SKILL.md`.
2. Se a mensagem tiver mais de um assunto (técnico ou teórico), liste-os e pergunte a ordem antes de propor qualquer coisa. Veja [`socratico.md`](socratico.md).
3. Leia apenas as referências da etapa atual e produza a proposta, sem despejar as 11 etapas de uma vez.
4. Apresente o **argumento completo** da decisão em um bloco curto: opções, critério que pesa, por que a escolha vence neste contexto e o que a derrubaria. Use [`trade-offs.md`](trade-offs.md) como catálogo.
5. Peça a **contraprova**: uma pergunta que faça a pessoa testar a proposta contra o próprio processo.
6. Não feche a decisão sem o cartão com a razão da pessoa. Se ela aceitar sua proposta, o campo "Minha razão" ainda precisa ser escrito por ela, com as palavras dela.
7. Faça perguntas somente se a escolha for uma porta de uma via ou faltar informação de negócio que só o usuário tem. Agrupe as perguntas daquela etapa em uma única rodada e ofereça opções concretas, com a recomendada marcada **(Recomendado)**. Se existir ferramenta interativa de perguntas, use-a; caso contrário, liste as opções na mensagem.
8. Para lacunas reversíveis, proponha uma suposição com status `aberta` e explique como revisitar. Não oculte incerteza como fato.
9. Termine cada etapa com: (1) o que foi decidido, (2) onde foi registrado (cartão, diário, ADR), (3) próximo passo e (4) a checagem de entendimento: "Entendi que ... Confere?".
10. Se o harness tiver lista nativa de tarefas, crie uma tarefa por etapa e atualize o status a cada avanço.

## Condensar sem pular

- Siga todas as etapas relevantes. Se a triagem indicar pequeno, resuma as etapas 8–10 num checklist de uma linha; não abra uma fase de desenho que não muda a decisão.
- Reclassifique quando a válvula de segurança detectar mais de cinco regras, estados ou módulos.
- Usuário que demonstra domínio pode receber menos explicação; o argumento, a contraprova e o cartão nunca desaparecem.
- Se uma decisão travar três vezes, pare de insistir; apresente as opções, o impasse e o que falta para decidir.

## Troca e finalização

- Só o usuário troca para `--learn`. Pode ser sugerida quando aparecer uma porta de uma via que pareça pouco familiar.
- Preserve decisão, estado e pontos em aberto durante a troca; continue na etapa atual.
- Ao final, entregue Ficha, cartões, entradas do diário (com data e hora), ADRs criados, suposições e perguntas em aberto e o Brief autocontido para Assistente. Inclua o que não fazer e os critérios observáveis.
- Um Brief não está pronto se restar suposição aberta em decisão de porta de uma via; pergunte e atualize o registro.
