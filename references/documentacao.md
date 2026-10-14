# Documentação viva do projeto

O objetivo é preservar por que algo foi decidido, não criar cerimônia. No final de cada bloco de trabalho, diga **o que documentar e onde** e ofereça escrever. Só escreva no projeto depois de permissão explícita; sem permissão, entregue o texto pronto para colar.

## Árvore sugerida (ajustável)

```text
docs/
  decisoes/
    0001-ambientes-separados.md
    0002-backups.md
  dominio/
    glossario.md
    mapa.md
  raio-x.md
  operacao.md
  equipe/
    chapeus.md
  diario.md
```

Não crie toda a árvore para um pedido pequeno. Use a organização que já existe no projeto; combine antes de impor convenção.

## Registro de Decisão

Use [`../templates/registro-decisao.md`](../templates/registro-decisao.md): número/título/data/status (proposta, aceita, substituída por N, descartada), contexto, evidência, opções, decisão, consequências, pontos fracos e como verificar.

- Decisões não se apagam: a nova indica “substitui 0003” e o registro antigo passa a “substituída por 0007”.
- Escolhas anteriores sem documento viram **retroativas**, com motivo conhecido ou “motivo não registrado”; diferencie o que foi confirmado depois.
- Não transforme uma suposição em decisão aceita sem validação de quem tem autoridade.

## Diário de bordo

Use [`../templates/diario-sessao.md`](../templates/diario-sessao.md): data, entrada/modo, o que foi feito, decisões (links), evidências, suposições/perguntas e próximo passo. Máximo de 10 linhas por sessão; concisão é intencional.

## Documentos vivos e começo de sessão

- Raio-X, operação, glossário e mapa são **atualizados**, não substituídos sem comparar a versão atual.
- Se `docs/` existir, leia o `diario.md` mais recente, `operacao.md` e os Registros de Decisão relevantes antes de perguntas. Consulte `raio-x.md`, `dominio/` ou `equipe/chapeus.md` quando forem relevantes.
- Se o projeto não documenta esses artefatos, isso é uma lacuna a confirmar, não permissão para criar arquivos.
- Registre evidência com links/paths e data; nunca coloque tokens, segredos, IPs sensíveis ou conteúdo pessoal desnecessário.

## Proporcionalidade

Pedido pequeno: uma linha de Diário e, se houver escolha relevante, um Cartão/Registro. Herdar pode atualizar um único achado no Raio-X; Operar pode atualizar só o status evidenciado. Ao encerrar, diga arquivo e seção e qual é o próximo passo.
