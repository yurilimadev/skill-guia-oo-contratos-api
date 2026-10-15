# Ponte entre linguagens

Em cada sessão confirme **linguagem alvo** (do projeto) e **linguagem de referência** (a que a pessoa domina melhor). A preferência conhecida do autor é Python; ofereça-o como referência inicial, sem assumir. A pessoa pode trocar o idioma de referência entre sessões ou no meio de uma (“agora usa Go como referência”); mantenha o alvo e anuncie a troca na linha de estado.

Na primeira vez que um conceito da linguagem alvo aparecer, explique em uma linha e dê equivalente aproximado na referência. A tabela permite qualquer par; os exemplos não são identidade exata. Se não houver equivalente direto, diga isso e explique a diferença sem forçar analogia. Confirme versão e convenções no projeto antes de recomendar.

| Conceito | TypeScript | Python | Java | C# | Go | PHP |
|---|---|---|---|---|---|---|
| Molde/estrutura de dados | `type`, `interface`, `class` | `TypedDict`, `dataclass`, classe/Pydantic | `record`, classe | `record`, classe | `struct` | classe |
| Objeto/instância | valor/objeto runtime; instância se criado por `class` | valores são objetos na linguagem; instância de classe é um caso | instância de `class`/`record` | instância de `class`/`record` | valor de um tipo (por exemplo, `struct`); não precisa ser classe | instância de `class` |
| Objeto/instância | valor/objeto runtime; instância se criado por `class` | valores são objetos na linguagem; instância de classe é um caso | instância de `class`/`record` | instância de `class`/`record` | valor de um tipo (por exemplo, `struct`); não precisa ser classe | instância de `class` |
| Estados/valores fechados | união discriminada, `enum` | `Enum` / `StrEnum` | `enum` | `enum` | tipo definido + constantes; validação explícita | `enum` (8.1+) ou constantes |
| Módulo/pasta | diretório e módulo | pacote (`__init__`) | `package` | namespace | `package` | `namespace` |
| Importar/visibilidade | `import` / `export` | `import`; visibilidade por convenção/API | `import`; modificadores de acesso | `using`; modificadores de acesso | `import`; exporta nome iniciado em maiúscula | `use`; visibilidade do membro |
| Assíncrono | `async` / `await` conforme runtime | `async` / `await` quando biblioteca/runtime suportam | APIs assíncronas do runtime/framework | `async` / `await` | goroutines, channels, `context`; sem `async/await` nativo | varia por runtime/biblioteca |
| Validação de contrato | tipos estáticos + validação runtime (ex.: Zod) | validação explícita, `TypedDict` não valida runtime; Pydantic é uma opção | Jakarta Bean Validation ou opção do projeto | DataAnnotations ou opção do projeto | validação explícita/opção adotada | validação explícita/framework |
| Testes | runner existente (ex.: Jest/Vitest) | `unittest` / pytest | JUnit | xUnit/NUnit | pacote padrão `testing` | PHPUnit |
| Erros | exceções ou tipo resultado, conforme API | exceções; retorno explícito se domínio pedir | exceções ou retorno tipado | exceções ou retorno tipado | `error` retornado explicitamente | exceções ou retorno tipado |
| Organização típica (não obrigatória) | módulos por domínio ou convenção existente | pacotes por domínio | packages | namespaces | packages | namespaces/estrutura do framework |

## Exemplos de ponte em qualquer direção

- **TypeScript ↔ Python:** `interface`/`type` descreve a forma estática; aproxima-se de `TypedDict` (forma de dicionário tipada), mas não garante validação runtime. `dataclass` cria classe com campos e pode carregar comportamento.
- **Go ↔ Python:** `struct` agrupa campos como uma `dataclass`, mas Go usa composição e métodos próprios; não há herança de classes equivalente.
- **Java ↔ TypeScript:** `record` imutável/de dados pode se aproximar de um objeto de tipo; `interface` de TS pode representar forma sem gerar objeto runtime. Não são intercambiáveis automaticamente.
- **PHP ↔ Python:** classe/enum e namespace lembram classes/enum e pacotes, mas carregamento, visibilidade e recursos dependem da versão/framework.

Use linhas da tabela para outros pares (por exemplo Java ↔ Go, C# ↔ PHP), explicando o conceito em comum e a diferença importante. Não traduza código palavra por palavra quando os modelos de erro, concorrência, runtime ou validação divergirem.

## Nomes e frameworks

Use nomes simples no idioma do projeto, sem pontos extras (`request.dto.ts`) a menos que essa seja a convenção existente. Não assuma Next.js, Django, Spring, ASP.NET, Gin ou Laravel; explique qualquer framework citado e confirme a versão antes de usar uma convenção específica.

## Como explicar

Uma linha, conectada à folha: “`struct` em Go é um molde com campos, parecido com uma `dataclass` Python; Go não tem o mesmo mecanismo de herança de classes.” Faça isso ao primeiro conceito alvo que aparecer, não recite a tabela inteira em cada resposta.
