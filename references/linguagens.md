# Linguagens e tradução dos conceitos

O método não pertence a uma linguagem. Antes de sugerir nomes, modelos ou código, confirme a linguagem/stack. Se manifests ou fontes estiverem visíveis, diga o que detectou e peça confirmação; um projeto pode ser poliglota ou estar migrando. Na primeira ocorrência de conceito específico, explique em uma linha com analogia simples.

| Conceito | TypeScript | Python | Java | C# | Go | PHP |
|---|---|---|---|---|---|---|
| Molde da folha | `type` ou `interface` | `dataclass` ou modelo Pydantic | `record` ou `class` | `record` ou `class` | `struct` | `class` |
| Estados | união discriminada ou `enum` | `Enum` | `enum` | `enum` | tipo definido + constantes | `enum` (PHP 8.1+) ou constantes tipadas |
| Pasta/módulo | pasta e módulo | pacote/pasta | `package` | namespace/pasta | `package` | namespace/pasta |
| Importar/exportar | `import` / `export` | `import` (módulo público) | `import` (visibilidade por acesso) | `using` (namespace); visibilidade por modificador | `import`; exporta identificador iniciado em maiúscula | `use`; visibilidade pública explícita |
| Assincronismo | `async` / `await`, conforme runtime | `async` / `await` quando biblioteca suporta | `CompletableFuture` ou APIs do framework | `async` / `await` | goroutines, channels e `context`; não há `async/await` nativo | APIs assíncronas dependem do runtime/framework |
| Validação de contrato | validação estática + biblioteca de runtime (ex.: Zod) | validação explícita ou Pydantic | Bean Validation (ex.: Jakarta) ou biblioteca do projeto | validação explícita / DataAnnotations / biblioteca do projeto | validação explícita ou biblioteca adotada | validação explícita ou biblioteca/framework do projeto |
| Organização comum (variável) | por módulo/domínio, se coerente com o app | por pacote/domínio | por pacote/domínio | por namespace/domínio | por pacote/domínio | namespace/domínio conforme framework |

As opções da tabela são equivalências aproximadas, não receitas. Verifique versão, runtime e convenções existentes; framework não é pressuposto.

## Exemplos de nome

Prefira nomes simples e consistentes com o projeto. Se o projeto está em português, `tipos.ts`, `regras.py`, `Requisicao.java`, `Requisicao.cs`, `requisicao.go` ou `Requisicao.php` podem ser mais didáticos que nomes com sufixos desconhecidos. Se o projeto adota `request.dto.ts`, siga a convenção explicando seu papel; não imponha uma convenção nova.

## O que explicar

- **Tipo/classe**: molde que diz quais campos uma folha preenchida pode ter.
- **Enum/união de estados**: lista de valores válidos do ciclo de vida.
- **Import/export ou package/namespace**: caminhos para um módulo usar o que outro oferece.
- **Assíncrono**: trabalho que pode aguardar rede/disco sem bloquear a continuação; mecanismos diferem por linguagem.
- **DTO/validador**: formato conferido que atravessa a porta da aplicação.

Explique o conceito quando aparecer pela primeira vez, não recite o glossário inteiro em uma resposta.
