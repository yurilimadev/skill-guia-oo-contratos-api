# Entrada Operar — sistema em produção

Use para saber se o sistema está vivo, correto e protegido, e como mudar sem quebrar. **Não execute comandos em produção, não altere DNS, servidor nem banco sem aprovação explícita.** Comece coletando evidências e só depois indique o próximo passo menor. O exemplo de referência é fictício: `references/exemplos/raio-x-erp.md`.

## Evidência antes de maturidade

Antes de atribuir nível, classifique cada item observado:

- **não existe** — procurado/confirmado no escopo indicado;
- **existe, não verificado** — declarado ou configurado, mas resultado não foi testado (ex.: backup sem restauração testada);
- **existe e verificado** — resultado e evidência observáveis (data da restauração, alerta recebido, log de teste, link do monitor).

“Existe” sem verificação não conta como capacidade comprovada. Separe observação, declaração do usuário, hipótese e pergunta. Não procure credenciais nem exponha segredos; peça evidência redigida/mascarada.

## Régua de maturidade

| Nível | Situação comprovada |
|---|---|
| 0 | Só produção; ninguém sabe que caiu até alguém reclamar |
| 1 | Sabe quando cai (monitor externo) e guarda erros |
| 2 | Backup com restauração testada; deploy repetível com volta atrás |
| 3 | Homologação separada; alertas úteis; checagens de consistência dos dados |
| 4 | Métricas de sucesso do negócio acompanhadas; incidentes viram aprendizado |

Não promedie evidências para inflar nível: declare o maior nível cujos requisitos estejam **todos verificados**, indique lacunas e sugira o **próximo passo menor**. Um ERP pode ter homologação e backup declarados; sem comprovação, marque “existe, não verificado” e não os trate como nível atingido.

## O1–O6

| Passo | Nome | Pergunta | Produz | Chapéus usuais e por quê |
|---|---|---|---|---|
| O1 | Mapa de produção | Onde roda? Servidor, DNS, certificado, banco, segredos, acesso e deploy? | Mapa de produção sem segredos | Infra e Segurança: mapear dependências e limites de acesso |
| O2 | Saber se está vivo | Há monitor de fora, health check e erros do servidor e navegador? | Visibilidade mínima | Infra e Suporte: detectar quedas e relacionar reclamações a sinais |
| O3 | Saber se está certo | Critérios de sucesso e invariantes aparecem nos dados reais? | Checagens de negócio | Dono do processo, Dados e QA: definir “certo” e como conferir |
| O4 | Não perder dados | Backup é automático? A restauração foi testada? Qual retenção? | Plano de backup/restore | Dados e Infra: validar recuperação sem sobrescrever produção |
| O5 | Mudar com segurança | Há homologação, deploy repetível, rollback e migrações reversíveis? | Caminho de mudança | Infra, Dados e QA: reduzir risco de deploy/migração |
| O6 | Quando der errado | Como responder, registrar e aprender sem culpados? | Roteiro e registro de incidente | Suporte, Infra e Dono do processo: coordenar recuperação e aprendizado |

### O1 — mapa

Registre categorias e relações necessárias; não copie secrets, senhas, tokens, IPs sensíveis ou dados pessoais para a documentação. Se a pessoa não souber algum item, marque como pergunta.

### O2 — sinais fracos e erros

“Deu erro e resolveu com F5” ou uma requisição falhou uma vez são dados, não ruído nem prova de defeito persistente. Pergunte quando ocorreu, tela/ação, mensagem/código, navegador/servidor e se houve repetição; sem coleta, marque hipótese. Proponha uma forma proporcional de registrar erros com contexto no servidor **e** navegador e disponibilidade externa. Considere categorias como monitor externo de uptime (por exemplo, UptimeRobot), coletor de erros (por exemplo, Sentry) e alertas; compare custo, esforço, privacidade e vendor lock-in, sem impor ferramenta.

### O3 — saúde do negócio

Retome critérios de sucesso da etapa 0 (quando houver Construir) e invariantes do domínio. Checagens podem encontrar saldo negativo ou registro sem responsável, mas precisam de escopo, privacidade, custo de consulta e resposta segura. Não execute consulta real de produção sem aprovação.

### O4 — backup/restauração

“Job de backup verde” prova que uma rotina terminou, não que dados restauram. Plano recomendado: cópia isolada/redigida, restauração fora da produção, verificação mínima e registro da evidência/data; não sobrescreva instâncias reais.

### O5 — mudança

Verifique separação entre produção/homologação por evidência não sensível, deploy repetível, rollback e migração. Não faça deploy nem teste exploratório em produção. Se não existir ambiente seguro, planeje primeiro a rede de proteção com Herdar.

### O6 — incidente e ciclo de retorno

Registre impacto, linha do tempo, sinais disponíveis, resposta, recuperação e aprendizado sem culpabilizar. O achado cria próximo passo em Herdar e lição para Construir. Não exponha dados pessoais ao registrar o incidente.

## Modos, entrega e documentação

- `--learn`: comece perguntando, por exemplo “Se o servidor cair às 3h da manhã, quem fica sabendo e quando?”. Ensine evidência, não suponha a causa.
- `--job`: apresente o inventário/evidências, o nível sustentado por elas e o próximo passo menor, com trade-off e Cartão.
- Preencha [`../../templates/ficha-operacao.md`](../../templates/ficha-operacao.md) e mapa de envolvidos. Cada item O1–O6 tem status de evidência, evidência e próximo passo.
- Registre critério de retorno de Construir; termine dizendo o que atualizar em `docs/operacao.md`, `docs/diario.md` e eventual Registro de Decisão. Só escreva nesses arquivos mediante permissão.
