# Vocabulário e perguntas de bolso

## Metáforas e critérios

- **Responsabilidade**: o único motivo para um pedaço de código mudar. Teste: “se X mudar, quantos arquivos eu mexo?”.
- **Caneta × papel**: quem segura a caneta (usuário que age) representa ator/permissão; o que está escrito no papel vira dado.
- **Assunto**: o que está na folha e tem uma ficha própria em outra pasta; a folha aponta para ele (por exemplo, `unidadeId`).
- **Vida própria × pedaço**: se algo pode existir sozinho, um módulo próprio é um padrão razoável; se só vive dentro de outro registro, costuma ficar embutido (como item do pedido). Compare custos antes de dividir ou juntar.

## Quatro tipos de substantivo

| Tipo | O que é | Exemplos |
|---|---|---|
| Quem | Pessoa, papel, lugar | Fornecedor, gestora, unidade |
| O quê | Coisa física | Produto |
| Compromisso | Promessa para o futuro, pode ter estados | Conta a pagar, requisição |
| Fato | Algo que já aconteceu; registra-se, não se desfaz como se não tivesse ocorrido | Pagamento, movimentação |

São lentes de modelagem, não uma taxonomia universal. Se um substantivo tiver características de mais de um tipo, explique a decisão e siga a linguagem do domínio.

## Lado de fora e lado de dentro

**Lado de fora (contrato):** recurso (substantivo na URL), verbo HTTP (GET lê; POST costuma criar; PATCH altera parcialmente; DELETE remove), endpoint e DTO (formulário que atravessa a porta: um de entrada e um de saída são um padrão que evita acoplamento interno).

**Lado de dentro (implementação):** tipo/modelo (molde da folha), objeto (folha preenchida), estado (situação de um compromisso), regra (condição permitida/proibida), método (verbo de uma responsabilidade), ação/caso de uso (verbo que coordena partes) e persistência (busca e grava; decisões de negócio ficam em componentes do domínio por padrão).

## Viagem do dado

1. A tela monta o DTO de entrada.
2. O pedido vira JSON.
3. A porta valida o contrato e obtém a pessoa autenticada da sessão.
4. A ação completa os campos controlados pelo servidor e executa a regra.
5. A persistência busca e grava.
6. Um tradutor (`paraSaida`, ou equivalente) monta o DTO de saída.
7. O resultado vira JSON e a tela mostra os dados legíveis.

Fluxo é um mapa para explicar responsabilidades; a arquitetura concreta pode variar conforme a linguagem e o framework.

## Dependências: direção de referência

| Quem | Pode depender de | Evite como padrão |
|---|---|---|
| Tela | contrato de apresentação (`import type` quando existir) | tipos internos, regras e banco |
| Porta de entrada | contrato e ações/casos de uso | detalhes do banco |
| Ação | tipos, contrato necessário, regras e persistência | detalhes da tela |
| Persistência | tipos/modelos que grava | contrato de tela, regras ou ações |

Direção de referência do método: `tela → porta → ações → regras → tipos` e `ações → persistência → tipos`. Regras normalmente não dependem de ações; modelos simples não precisam importar camadas. Um projeto existente pode adotar outra direção válida: registre a fronteira e o motivo em vez de forçar uma forma.

## Pastas de referência

```text
<substantivo>/
  tipos.<extensao>       # etapas 3 e 4
  contrato.<extensao>    # etapa 6 (DTOs e tradução para saída)
  regras.<extensao>      # etapa 5
  banco.<extensao>       # busca e grava
  acoes.<extensao>       # etapa 7
integracoes/<sistema-externo>/  # fronteira que muda com o parceiro
infra/                          # banco, sessão, logs compartilhados
```

Esta árvore é um padrão didático, não uma obrigação de framework. Use a extensão e a convenção existentes no projeto; nomes simples no idioma do projeto, sem pontos extras como `request.dto.ts` enquanto isso não for convenção confirmada.

## Perguntas de bolso

1. De qual substantivo é? → considere a pasta do módulo.
2. É contrato, regra, ação ou persistência? → escolha o arquivo responsável.
3. Serve a vários módulos e não é regra de negócio? → considere `infra/`.
4. Quem pode segurar a caneta aqui? → ator e permissão.
5. Se essa regra mudar, qual folha muda? → dono da regra.
6. O verbo mexe em um substantivo ou em vários? → método/regra local ou ação que coordena.

Cada resposta é um critério inicial para comparar opções; se o contexto apontar para outra solução, registre a razão e o custo.

## Sinais de alerta herdados

Leia [`revisao.md`](revisao.md) para o catálogo completo, o motivo de cada alerta e quando uma alternativa pode ser aceitável.
