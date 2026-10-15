# Objetos, classes e responsabilidades

Use esta referência para ajudar a pessoa a imaginar abstrações de software sem transformar cada substantivo em classe. A ideia pedagógica é **qualquer coisa pode ser representada como objeto se houver uma necessidade clara**; isso não significa que toda coisa precise de objeto, classe ou linha no banco. Primeiro descubra a necessidade e a responsabilidade; depois escolha a forma da linguagem.

## Três sentidos que não devem se misturar

- **Coisa do domínio:** algo de que as pessoas falam, como produto, saldo, recebimento, pessoa ou política.
- **Abstração/objeto do software:** uma unidade que representa dados, comportamento ou coordenação para cumprir uma necessidade. Pode ser algo tangível ou uma ideia (regra, cálculo, ação, relatório).
- **Objeto em sentido técnico:** valor/instância que existe durante a execução de uma linguagem. Em algumas linguagens aparece como instância de uma classe; em outras a forma principal pode ser um `struct`, registro, função, módulo ou valor. Consulte `linguagens.md` e não prometa que todos os idiomas usam classes da mesma maneira.

**Classe** é um molde/tipo que descreve uma forma ou comportamento; **objeto/instância** é um exemplar concreto desse molde em execução. Essa é uma distinção didática, não uma regra universal da linguagem: em Python, por exemplo, a própria classe também é um objeto em runtime. Em conversa de modelagem, “objeto” também pode significar a abstração de software — diga qual sentido está usando. Uma tabela no banco não é automaticamente uma classe, e uma classe não precisa ter uma linha correspondente.

## Como decidir se uma abstração precisa existir

Pergunte, uma decisão por vez:

1. **Qual necessidade essa abstração atende?** O que fica mais claro, seguro ou fácil de mudar?
2. **Que responsabilidade teria?** Qual é o motivo para ela mudar?
3. **Que dados e regras mudam juntos?** Há identidade, ciclo de vida, estado ou histórico próprios?
4. **Ela coordena outras partes ou representa uma delas?** Um caso de uso/controlador pode coordenar; uma entidade/valor protege regra local.
5. **Precisa ser persistida?** É fato/dado que precisa sobreviver à execução, ou ferramenta temporária para calcular/validar/transportar?
6. **Qual é o menor passo útil?** Uma função ou módulo simples pode bastar; não crie classe por cada substantivo nem por antecipação.

O vocabulário de “quem/o quê/compromisso/fato” gera candidatos, não uma lista de classes obrigatórias. Um candidato pode virar objeto de domínio, dado simples, enum, função, módulo, ator externo, registro de banco ou nenhum artefato separado. Registre a escolha e o trade-off em `trade-offs.md`/Cartão.

## Pessoa real, ator e responsabilidade do software

As folhas de papel não interagem sozinhas: **na vida real, pessoas executam ações e fazem as folhas passarem de mão em mão**. No software, relações entre dados não provocam uma ação por si só. Uma requisição, evento, rotina ou pessoa aciona código; a aplicação então chama as responsabilidades necessárias.

Por isso, o humano costuma aparecer primeiro como **ator** no fluxo (“estoquista confere o recebimento”), não como uma classe automaticamente. Só represente pessoa/usuário/papel como dado ou objeto quando a aplicação precisa guardar identidade, autorização, histórico ou relação de negócio. Um papel pode ser relação/atributo/política, ou um objeto separado se tiver dados, regras ou ciclo próprios.

O trabalho que um humano faz na realidade ajuda a revelar **qual responsabilidade o sistema precisa executar**, mas não determina que exista uma classe com o cargo daquela pessoa. Pergunta de bolso: **“quem faz isso na vida real?” ajuda a descobrir a regra; “qual objeto deve ser responsável no software?” depende dos dados, invariantes, limites e mudanças.**

## Exemplo — estoquista e estoque

Na clínica/ERP, o **estoquista** pode contar produtos, conferir divergências e autorizar uma correção. Ele é o ator do processo. O software não precisa criar uma classe `Estoquista` só porque uma pessoa realiza o trabalho; pode precisar de identidade/papel para saber quem está autenticado e autorizado.

`Estoque` é um candidato a abstração se representar o saldo de produtos de uma unidade e proteger regras como “o saldo não fica negativo”. Pode manter/consultar os saldos dos produtos que pertencem àquela fronteira. Mas “somar saldo de todos os produtos da rede” pode ser uma consulta/read model, especialmente se atravessa muitas unidades. A responsabilidade pode pertencer a um módulo, função de consulta ou objeto de serviço — não escolha antes de conhecer o volume, a consistência e o dono da regra.

| Na vida real | Papel possível no software | Pergunta de bolso |
|---|---|---|
| Estoquista conta e confere | Ator que inicia `RegistrarDivergencia` | Quem inicia; o sistema registra quem? |
| Estoque guarda saldo por unidade/produto | Objeto/entidade/módulo `Estoque`, se possui esse dado e a invariante | O que precisa estar certo junto e quem protege isso? |
| Ação recebe quantidades e coordena validação e gravação | Objeto/caso de uso transitório, por exemplo `RegistrarDivergencia` | Coordena mais de uma responsabilidade? |
| Total geral para um painel | Consulta/read model ou função, se for projeção | É uma regra do agregado ou uma leitura para relatório? |

Este é um mapa possível, não uma decisão para todo ERP. Em `--learn`, peça à pessoa para apontar o que é ator, dado e responsabilidade antes de sugerir os nomes.

## Banco de dados: objetos persistentes e transitórios

| Candidato | Precisa ter linha própria? | Heurística |
|---|---|---|
| Entidade/fato com identidade, ciclo de vida ou histórico (recebimento, movimentação) | Frequentemente é persistido, talvez em uma ou várias tabelas | Pergunte qual identidade e história precisam sobreviver; defina o dado que a pessoa precisa consultar |
| Valor/estrutura embutida (unidade de medida aplicada, endereço do snapshot) | Pode ser coluna, valor embutido ou parte de outra linha | Preserve o significado; não crie tabela/classe separada sem identidade/ciclo próprios |
| Ação/caso de uso, política, validador ou calculador | Em geral não precisa de linha própria; vive durante a execução | Persiste o resultado/fato, não necessariamente o mecanismo que o produziu |
| DTO, comando ou mensagem | Pode ser transitório, logado ou persistido conforme necessidade | “Existe no código” não implica “tem tabela”; retenção e privacidade importam |

**Heurística, não equivalência:** se há uma linha persistida, provavelmente há um dado/registro importante do domínio, mas a relação não é 1:1. Uma classe pode usar várias tabelas; uma tabela pode ser só associação, índice ou projeção; e um objeto pode não ser salvo. Antes de remodelar, confira as migrations, fluxos, relatórios e contratos reais.

## Exercício recorrente para `--learn`

Quando houver oportunidade sem interromper o fluxo, escolha uma ação real e use estas perguntas ao longo da conversa (respeite o limite do modo learn: no máximo duas por mensagem):

1. “Quem faz isso na vida real?”
2. “Que responsabilidade precisa existir no sistema e que dado ela protege?”
3. “O que você chamaria de objeto, classe, ator ou informação transitória — e por quê?”

Espere a tentativa. Depois mostre uma alternativa e explique o trade-off. Às vezes a resposta correta é “não precisa de uma classe nova”: o pensamento que se quer praticar é fazer surgir abstrações **da necessidade**, não decorar que todo substantivo vira classe.
