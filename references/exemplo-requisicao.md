# Exemplo — requisição de produtos

Este exemplo preserva a referência original e mostra como uma sessão pode registrar os motivos das escolhas. O mesmo modelo pode ser traduzido para outra linguagem; confirme a linguagem/stack antes de escolher a sintaxe.

## História e modelagem

A gestora solicita produtos para uma unidade. A requisição registra a unidade, os itens e quem a abriu. Depois, uma pessoa autorizada atende tudo ou parte do pedido; o atendimento verifica estoque e registra uma movimentação. A requisição também pode ser cancelada conforme as regras do negócio.

| Substantivo | Tipo | Vida própria? | Módulo sugerido |
|---|---|---|---|
| Gestora/usuário | Quem | Sim | identidade/permissões |
| Unidade | Quem/lugar | Sim | unidade |
| Produto | O quê | Sim | produto/estoque |
| Requisição | Compromisso | Sim | requisicao |
| Item da requisição | Pedaço da requisição | Não, no exemplo | dentro de `requisicao` |
| Movimentação | Fato | Sim | movimentacao |

### Folha da requisição

| Campo | Classificação | Observação |
|---|---|---|
| `id` | Identificador gerado | Servidor/persistência |
| `unidadeId` / `unidade_id` | Referência | Aponta para a unidade |
| `itens` | Pedaços | Cada item referencia produto e quantidade pedida |
| `criadaEm` / `criada_em` | Dado de auditoria | Gerado pelo servidor, não recebido como autoridade do cliente |
| `status` | Estado | Inicialmente `aberta` |
| `criadaPor` / `criada_por` | Assinatura | Identidade autenticada da sessão |

Estados de exemplo: `aberta → atendida | parcial | cancelada`. Cada seta é uma ação com permissão, regra, registro e resposta apropriada. A movimentação é um fato novo, não um estado da requisição.

## Contrato e critérios observáveis

| Ação | Verbo e endereço sugeridos | Entrada | Saída |
|---|---|---|---|
| Criar requisição | `POST /requisicoes` | `unidadeId`, `itens[]` | id, estado inicial, data, assinatura e resumo legível |
| Atender requisição | `POST /requisicoes/{id}/atendimento` | quantidades atendidas conforme contrato decidido | novo estado, itens atendidos e movimentações relacionadas |
| Cancelar requisição | `POST /requisicoes/{id}/cancelamento` | motivo se o negócio exigir | estado `cancelada` e confirmação |

Critérios de aceite iniciais:

```text
Dado uma pessoa autorizada e uma requisição com unidade e itens válidos
Quando ela cria a requisição
Então vê a confirmação com o identificador e o estado “aberta” (a identidade e a data são preenchidas pelo servidor)

Dado uma requisição aberta e saldo suficiente
Quando uma pessoa autorizada a atende
Então a requisição mostra os itens atendidos e a movimentação correspondente pode ser consultada

Dado uma requisição aberta
Quando uma pessoa sem permissão tenta atendê-la
Então a tela informa que a ação não foi autorizada (a API responde 403)
```

Cada regra adicional da etapa 5 deve ganhar um critério. Se não houver estoque suficiente, o que deve ocorrer? Essa é uma pergunta de negócio, não um palpite técnico.

## Cartões de decisão de exemplo

### Decisão: atender como ação ou editar status

- **Opções:** `POST /requisicoes/{id}/atendimento` ou `PATCH` com status `atendida`.
- **Escolha provisória:** sub-recurso POST, porque atendimento verifica estoque e cria movimentação.
- **Ganho:** a rota nomeia o que aconteceu e permite validar a transição e registrar efeitos.
- **Custo:** uma rota e um DTO de ação a mais.
- **Reversível?** Duas vias enquanto o contrato não estiver publicado; uma via após clientes dependerem dele.
- **Quando revisitar:** se atendimento deixar de ter efeitos além de editar um campo.
- **Modo:** job (exemplo didático).

### Decisão: DTO separado ou tipo interno

- **Opções:** aceitar a própria entidade ou receber DTO específico.
- **Escolha provisória:** DTO separado; id, estado, data e autor são dados sob controle do servidor.
- **Ganho:** o cliente só envia os campos permitidos.
- **Custo:** traduzir entrada/saída.
- **Reversível?** Duas vias antes de publicar o contrato.
- **Quando revisitar:** se for protótipo estritamente interno, sem exposição nem metadado de servidor.
- **Modo:** job (exemplo didático).

### Decisão: item com módulo próprio ou parte da requisição

- **Opções:** módulo independente ou item embutido.
- **Escolha provisória:** item permanece dentro da requisição porque só existe nesse pedido neste exemplo.
- **Ganho:** modelo direto, sem abstração adicional.
- **Custo:** consultas e reuso independente de itens ficam limitados.
- **Reversível?** Duas vias enquanto não houver uso independente; migração mais cara depois de persistir referências externas.
- **Quando revisitar:** se item de requisição passar a ter ciclo de vida próprio ou for referenciado fora do pedido.
- **Modo:** learn (exemplo didático).

## Consistência, falhas e pontos em aberto

- **Consistência:** se estoque e requisição estiverem no mesmo banco, considerar uma transação para reservar/baixar saldo e gravar atendimento/movimentação. Se houver sistemas separados, registrar a decisão sobre consistência eventual e compensação.
- **Concorrência:** duas pessoas podem tentar atender o mesmo saldo; verificar o saldo e a transição de forma atômica ou adotar mecanismo apropriado ao banco.
- **Repetição:** repetição do pedido de atendimento pode baixar estoque duas vezes. Perguntar se o contrato precisa de chave idempotente antes de fechar a API.
- **Falhas:** saldo insuficiente, requisição já atendida/cancelada, produto inativo, usuário sem permissão, falha ao persistir movimentação.

### Suposições (exemplo)

| ID | Suposição | Status |
|---|---|---|
| ASM-001 | A pessoa autenticada tem identidade de unidade e papel disponíveis na sessão | aberta — confirmar com o projeto |
| ASM-002 | Requisição e estoque compartilham o mesmo banco | aberta — porta de uma via para consistência |

### Perguntas em aberto (exemplo)

| ID | Pergunta | Status |
|---|---|---|
| Q-001 | O que fazer quando só parte da quantidade tem saldo? | aberta |
| Q-002 | Como evitar atendimento repetido em caso de retry? | aberta |

Com ASM-002 ainda aberta, o Brief é ilustrativo, não pronto para implementação. Confirme a suposição de consistência antes de fechar o plano.

## Trechos de tipos no idioma confirmado

O mesmo modelo, escrito em **TypeScript**:

```ts
type StatusRequisicao = "aberta" | "atendida" | "parcial" | "cancelada";

interface ItemRequisicao {
  produtoId: string;
  quantidade: number;
}

interface Requisicao {
  id: string;
  unidadeId: string;
  itens: ItemRequisicao[];
  criadaEm: string;
  status: StatusRequisicao;
  criadaPor: string;
}
```

O mesmo modelo, escrito em **Python**:

```python
from dataclasses import dataclass
from datetime import datetime
from enum import StrEnum


class StatusRequisicao(StrEnum):
    ABERTA = "aberta"
    ATENDIDA = "atendida"
    PARCIAL = "parcial"
    CANCELADA = "cancelada"


@dataclass(frozen=True)
class ItemRequisicao:
    produto_id: str
    quantidade: int


@dataclass(frozen=True)
class Requisicao:
    id: str
    unidade_id: str
    itens: tuple[ItemRequisicao, ...]
    criada_em: datetime
    status: StatusRequisicao
    criada_por: str
```

São apenas os tipos da folha para ilustrar a tradução, não implementação completa nem padrão obrigatório. Em Python anterior à versão 3.11, confirme a versão e escolha a representação de enum compatível. A tela continua dependendo do contrato de saída, não necessariamente da entidade.

## Brief ilustrativo

> **Objetivo:** permitir que uma pessoa autorizada solicite produtos para uma unidade e que outra pessoa autorizada registre atendimento parcial ou total.
>
> **Módulos:** `requisicao`, `estoque/produto`, `movimentacao`, identidade/permissões.
>
> **Contrato provisório:** criar via `POST /requisicoes`; atender via `POST /requisicoes/{id}/atendimento`; cancelar via `POST /requisicoes/{id}/cancelamento`. DTOs separados por padrão; usuário, id, estado inicial e data são controlados pelo servidor.
>
> **Regras:** só atender requisição aberta; conferir saldo; registrar movimentação junto ao atendimento; obter autor da sessão. Falta definir comportamento de saldo parcial.
>
> **Critérios:** usar os exemplos Dado/Quando/Então acima e adicionar um critério por regra confirmada.
>
> **Não fazer:** não aceitar assinatura ou estado inicial enviados pelo cliente; não descontar estoque duas vezes em retry; não editar arquivos fora dos módulos combinados.
>
> **Bloqueios:** confirmar ASM-002 (fronteira transacional) e responder Q-001/Q-002 antes de marcar o brief pronto.
