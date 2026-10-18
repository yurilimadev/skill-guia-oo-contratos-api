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

- **Decisão:** atendimento como sub-recurso `POST /requisicoes/{id}/atendimento`, não `PATCH` de status.
- **Minha razão (exemplo — a pessoa escreve a própria):** atender verifica estoque e cria movimentação; um `PATCH` de status deixaria esses efeitos sem lugar claro.
- **Alternativa descartada e por quê:** `PATCH` com status `atendida`, porque pularia a verificação de saldo e a movimentação.
- **O que me faria mudar de ideia:** se atendimento deixar de ter efeitos além de editar um campo.
- **Crítica da skill:** a razão depende de "atendimento tem efeitos". Isso vale só se nenhum outro caminho alterar o estoque sem passar por esta ação.
- **Reversível?** Duas vias enquanto o contrato não estiver publicado; uma via após clientes dependerem dele.
- **Modo:** job (exemplo didático).

### Decisão: DTO separado ou tipo interno

- **Decisão:** DTO específico para entrada e saída, em vez de aceitar a entidade inteira.
- **Minha razão (exemplo):** id, estado, data e autor são controlados pelo servidor; o cliente não deve conseguir enviá-los.
- **Alternativa descartada e por quê:** reutilizar a entidade, porque ela expõe campos que o cliente não deve escrever.
- **O que me faria mudar de ideia:** se for um protótipo interno, sem exposição nem campos de servidor.
- **Crítica da skill:** a razão é segurança, mas o DTO também custa tradução em duas direções. Quanto isso pesa no seu caso?
- **Reversível?** Duas vias antes de publicar o contrato.
- **Modo:** job (exemplo didático).

### Decisão: item com módulo próprio ou parte da requisição

- **Decisão:** o item fica dentro da requisição, sem módulo próprio.
- **Minha razão (exemplo):** o item só existe dentro desta requisição; sem ela, não tem sentido.
- **Alternativa descartada e por quê:** módulo independente de itens, porque não há uso fora do pedido que justifique a separação.
- **O que me faria mudar de ideia:** se item passar a ter ciclo de vida próprio ou for consultado fora do pedido.
- **Crítica da skill:** você sabe se algum relatório vai consultar itens sem passar pela requisição? Se sim, o argumento enfraquece.
- **Reversível?** Duas vias enquanto não houver uso independente; migração mais cara depois de persistir referências externas.
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
