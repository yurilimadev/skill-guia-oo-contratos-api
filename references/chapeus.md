# Chapéus — quem precisa participar

Um dev Full Cycle atravessa vários tipos de decisão. Em equipe pequena, a mesma pessoa (muitas vezes quem pede a mudança) veste vários chapéus. São **papéis**, não cargos: o papel existe mesmo sem alguém formalmente nomeado. Não invente pessoas; pergunte quem veste cada chapéu. “Envolver” pode ser só preparar uma pergunta concreta, não convocar reunião.

## Catálogo

| Chapéu | Pelo que responde | O que entrega / pergunta típica | Sinal de que foi esquecido |
|---|---|---|---|
| Dono do processo (negócio) | Por que existe, regras e sucesso | decide regra e “como é sucesso para você?” | regra foi inventada pelo dev |
| Usuário-chave | Como o trabalho ocorre de verdade | exceções e “o que acontece quando falta item?” | só o caminho feliz foi modelado |
| Produto | Prioridade, escopo e o que fica fora | ordem e limite “o que fica para depois?” | uma pequena entrega vira sistema inteiro |
| Domínio/arquitetura | Modelo, fronteiras e decisões caras | contexto, trade-offs e mudança de negócio | nomes iguais ganham significados diferentes |
| Backend | Regra no servidor, API e integração | contrato, validação e falhas | rota aceita campo ou estado sem regra |
| Frontend/UX | Tela, fluxo e mensagens | protótipo/estados vazios e “o que vê ao falhar?” | regra correta, mas pessoa não sabe como agir |
| Dados | Esquema, migração, consistência e relatório | integridade e “qual valor precisa ser histórico?” | mudança funcional quebra relatório/dados |
| QA/Qualidade | Aceite, bordas e regressão | casos observáveis e caracterização | só caminho feliz foi testado |
| Infra/Operação | Ambientes, deploy, monitoramento e backup | evidência de deploy/restore/alerta | “funciona na minha máquina”; produção sem sinal |
| Segurança/LGPD | Acesso, privacidade, auditoria | ameaça, acesso e retenção | exposição indevida ou coleta sem motivo |
| Suporte | Reclamações e ajuda ao usuário | contexto de tickets e padrões de erro | o mesmo problema reaparece sem aprendizado |

## Quem envolver por ponto do fluxo

| Trabalho | Chapéus típicos | Por quê |
|---|---|---|
| Construir 0–2 (problema, história, caneta) | Dono do processo, Usuário-chave, Produto | alinhar problema, exceções reais e escopo |
| Construir 3–5 e teste de estresse | Domínio, Dono do processo, Dados | validar significado, identidade, regra e histórico |
| Construir 6–7 (contrato/ação) | Backend, Frontend/UX | entrada/saída e sequência que a pessoa vê |
| Construir 8–10 | Dados, QA, Infra, Segurança conforme risco | consistência, prova, publicação e recuperação |
| Herdar H1–H3 | quem conhece o sistema hoje, Usuário-chave, Suporte | confrontar inventário/código com uso real |
| Herdar H4–H6 | Domínio, QA, Infra | priorizar dívida e construir rede segura |
| Operar O1–O6 | Infra, Segurança, Suporte e Dono do processo (O3) | saúde técnica, acesso, relatos e sucesso de negócio |

## Sinais que acrescentam chapéus

- **Dinheiro:** Dono do processo financeiro e, se existir, contabilidade.
- **Dados pessoais:** Segurança/LGPD.
- **Integração externa:** responsável pelo outro lado da integração.
- **Tela/fluxo novo:** Usuário-chave e Frontend/UX.
- **Migração/esquema:** Dados e Infra.
- **Várias unidades/setores:** ao menos uma voz de cada contexto afetado.
- **Incidente recorrente:** Suporte e Operação, além do dono da regra.

Pedido pequeno costuma envolver 1–2 chapéus. Use julgamento; não transforme esta tabela em cerimônia obrigatória.

## Mapa de envolvidos

Para cada etapa, monte `templates/mapa-envolvidos.md`: chapéu, por que agora, pergunta concreta a levar, quem veste e status (a perguntar/perguntado/respondido). A **pergunta a levar** é mais valiosa que só listar títulos; a resposta atualiza suposição, pergunta aberta ou decisão.

Se a própria pessoa veste o chapéu, diga, por exemplo: “Agora você está com o chapéu de QA: o que QA perguntaria aqui?”. Em `--learn`, peça que tente antes de revelar perguntas típicas; em `--job`, aplique-as e marque **chapéu vestido por você (sem segunda opinião)** como ponto fraco da proposta.

Registre em `docs/equipe/chapeus.md` apenas os papéis e nomes que a pessoa fornecer. Não invente equipe, avaliações, dados pessoais ou opinião sobre indivíduos; guarde somente nome e papel necessários.
