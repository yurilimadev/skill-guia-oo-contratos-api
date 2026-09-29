---
name: "folha-ao-codigo"
description: "Guia para modelar uma funcionalidade nova antes de implementar: conduz as 7 etapas Da Folha ao Código e gera a Ficha de Modelagem com objetos, estados, regras, contrato de API e pastas."
---

# Da Folha ao Código

Método para sair de um processo real (da clínica, do ERP, de qualquer sistema) e chegar em: tipos/objetos, estados, regras, contrato de API, ações e pastas. A ideia central: **todo sistema é um arquivo de pastas digitalizado**. Para organizar qualquer coisa, pergunte: *se isso fosse papel, qual seria a folha?*

## Quando usar

- O usuário quer implementar uma funcionalidade nova e precisa decidir o que criar e onde.
- Depois de um discovery (história, personagens, problema já entendidos) e antes de definir estrutura de projeto ou escrever código.
- Para revisar código existente e apontar onde as coisas estão fora do lugar (modo revisão, no fim).

Não usar para correções pontuais de UI, bugs isolados ou dúvidas de sintaxe.

## Como conduzir

- Tudo em português do Brasil.
- **Uma etapa por vez.** Faça as perguntas da etapa, espere a resposta, mostre o resultado da etapa e só então avance. Não despeje as 7 etapas de uma vez.
- Se o discovery já trouxe a história e os personagens, confirme em uma frase e pule para a etapa 3.
- O usuário está aprendendo a pensar assim: explique com a metáfora do papel, use TypeScript simples, sem convenções de framework (Next etc.) sem explicar. Nomes de arquivo simples em português (`tipos.ts`, `regras.ts`), sem pontos extras no nome.
- Quando o usuário errar uma classificação, não só corrija: diga qual pergunta de bolso resolve o caso.
- Respostas diretas e curtas. Ao final, entregue a Ficha de Modelagem pronta para uso.

## Vocabulário

**Critérios para pensar**
- **Responsabilidade**: o único motivo para um pedaço de código mudar. Teste: "se X mudar, quantos arquivos eu mexo?"
- **Caneta × papel**: quem segura a caneta (usuário que age) vira **permissão**. O que está escrito no papel vira **dado**.
- **Assunto**: o que está escrito na folha e tem ficha própria em outra pasta. A folha só aponta para ele (`unidadeId`).
- **Vida própria × pedaço**: se existe sozinho, ganha módulo; se só existe dentro de outro, fica dentro dele (item do pedido).

**Os 4 tipos de substantivo**
| Tipo | O que é | Exemplos |
|---|---|---|
| Quem | Pessoa, papel, lugar | Fornecedor, Unidade |
| O quê | Coisa física | Produto |
| Compromisso | Prometido para o futuro; tem estados | Conta a pagar, Requisição |
| Fato | Já aconteceu; não muda, só se registra | Pagamento, Movimentação |

**Lado de fora (contrato)**: Recurso (substantivo na URL) · Verbo HTTP (GET lê, POST cria, PATCH edita, DELETE apaga) · Endpoint (verbo + endereço) · DTO (o formulário que atravessa a porta; um de entrada e um de saída).

**Lado de dentro (implementação)**: Tipo (molde da folha) / objeto (folha preenchida) · Estado (situações do compromisso) · Regra (proibido/obrigatório) · Método (verbo de um só substantivo) · Ação/caso de uso (verbo que coordena vários substantivos) · Banco (só busca e salva, não decide).

## As 7 etapas

| # | Etapa | Pergunta ao usuário | Produz |
|---|---|---|---|
| 1 | História em papel | Como isso funcionaria sem sistema nenhum? Quem preenche o quê, e o que acontece depois? | Entendimento |
| 2 | Caneta × papel | Quem escreve? O que ou quem está escrito? | Lista de permissões + assuntos |
| 3 | Campos da folha | Quais campos tem a folha? Cada um é referência, dado simples ou assinatura? O que tem vida própria? | `type` do objeto |
| 4 | Estados | Por quais situações o compromisso passa? O que faz mudar de uma para outra? | `type` do status + lista de ações (as setas) |
| 5 | Regras | O que é proibido ou obrigatório? Se a regra mudar, qual folha muda? | Funções de regra e seus donos |
| 6 | Contrato | O que a tela envia e o que precisa ver de volta? | Endpoints + DTOs de entrada e saída |
| 7 | Ação | Qual sequência de passos cumpre o pedido? Mexe em um ou vários substantivos? | Função de ação e o módulo onde mora |

Detalhes por etapa:
- **Etapa 3**: classifique cada campo como **referência** (aponta para outra ficha → `…Id`), **dado simples** (só existe nesta folha), **assinatura** (quem segurou a caneta → `criadaPor`, vem da sessão) ou **estado**.
- **Etapa 4**: só compromissos têm estados. Fatos não mudam. Cada seta entre estados é uma ação.
- **Etapa 6**: ação de negócio vira **substantivo criado com POST** (`POST /requisicoes/123/atendimento`), nunca `PATCH` trocando status. O DTO de entrada não tem `id`, `status`, datas nem autor. O DTO de saída traz o que a tela precisa ver (ex.: `unidadeNome`) sem sujar a folha.
- **Etapa 7**: a ação mora no módulo do substantivo que ela mais altera e chama os especialistas dos outros módulos. Lida em voz alta, deve soar como a história da etapa 1.

## A viagem do dado

O mesmo pedido muda de forma três vezes:

1. Tela monta o **DTO de entrada** → 2. vira texto (JSON) → 3. porta de entrada confere e pega o usuário da sessão → 4. ação completa a **folha** (`id`, data, status inicial, assinatura) → 5. banco guarda → 6. tradutor (`paraSaida`) monta o **DTO de saída** → 7. vira texto → 8. tela mostra.

Quem pode importar o quê:
| Quem | Pode importar | Nunca importa |
|---|---|---|
| Tela | só `contrato.ts` (com `import type`) | tipos, regras, banco, acoes |
| Porta de entrada | contrato, acoes | banco |
| Ação | tipos, contrato, regras, banco | nada da tela |
| Banco | só tipos | contrato, regras, acoes |

Direção das setas: `tela → porta → acoes → regras → tipos` e `acoes → banco → tipos`. Regras nunca importam ações. Tipos não importam nada.

## Pastas

```
<substantivo>/
  tipos.ts      ← etapas 3 e 4
  contrato.ts   ← etapa 6 (DTOs + paraSaida)
  regras.ts     ← etapa 5
  banco.ts      ← busca e salva
  acoes.ts      ← etapa 7
integracoes/<sistema-externo>/   ← muda quando o sistema externo muda
infra/                           ← banco, sessão, logs: serve a todos
```

## Perguntas de bolso

1. De qual substantivo é? → pasta do módulo.
2. É contrato, regra, ação ou banco? → arquivo dentro do módulo.
3. Serve a vários módulos e não é regra de negócio? → `infra/`.
4. Quem pode segurar a caneta aqui? → permissão.
5. Se essa regra mudar, qual folha muda? → dono da regra.
6. O verbo mexe em um substantivo ou em vários? → um: método/regra; vários: ação.

## Sinais de alerta

- Arquivo genérico que só cresce (`types.ts`, `utils.ts`, `domain.ts`).
- Regra de negócio na rota ou na tela.
- `PATCH` trocando status (pula as regras).
- Campo na entidade só porque a tela precisa (é trabalho do DTO de saída).
- Tela importando regras, banco ou tipos internos.
- `criadaPor`, `status` ou `id` vindo do corpo do pedido.
- `regras.ts` importando `acoes.ts`.

## Entrega: Ficha de Modelagem

Ao final das etapas, entregue esta ficha preenchida. Ela é a entrada para a estrutura de projeto e para a implementação.

```markdown
# Ficha de Modelagem — <nome da funcionalidade>

## 1. História em papel
<3 a 5 linhas>

## 2. Caneta × papel
| Quem | Segura a caneta? | Vira |

## 3. Substantivos
| Substantivo | Tipo (quem/o quê/compromisso/fato) | Vida própria? | Módulo |

### Campos da folha principal
| Campo | Classificação (referência/simples/assinatura/estado) |

## 4. Estados
ESTADO_A → (ação) → ESTADO_B ...

## 5. Regras
| Regra | Dono | Arquivo |

## 6. Contrato
| Ação | Verbo | Endereço | DTO entrada | DTO saída |

## 7. Ações
| Ação | Mora em | Passos (em português) | Chama quais módulos |

## Pastas e arquivos a criar
<árvore>

## Pontos em aberto
<dúvidas para decidir antes de implementar>
```

Se o código for pedido junto, gere apenas os esqueletos (`tipos.ts`, `contrato.ts`, `regras.ts`, `acoes.ts`) com comentários em português explicando cada campo, e explique `async/await` e `import/export` na primeira vez que aparecerem.

## Modo revisão (código existente)

Quando o usuário pedir para revisar um módulo:
1. Reconstrua a folha a partir do código (etapa 3) e os estados (etapa 4).
2. Liste o que está fora do lugar usando os sinais de alerta, com arquivo e o motivo.
3. Proponha a movimentação em forma de tabela: `de → para → por quê (qual pergunta de bolso)`.
4. Não mova nada sem o usuário aprovar.

## Exemplo de referência (resumido)

Requisição de produtos: a gestora (caneta) pede produtos para a unidade (assunto). Folha: `unidadeId` (referência), `itens[]` (pedaço, sem módulo próprio), `criadaEm` (simples), `status` (estado), `criadaPor` (assinatura, vem da sessão). Estados: aberta → atendida / parcial / cancelada. Contrato: `POST /requisicoes`, `POST /requisicoes/{id}/atendimento`, `POST /requisicoes/{id}/cancelamento`. Ação `atenderRequisicao` mora em `requisicao/acoes.ts` e chama `estoque/regras` (saldo) e `movimentacao/acoes` (cria o fato).
