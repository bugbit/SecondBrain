# AGENTS.md

Este documento define las instrucciones de trabajo para agentes IA, asistentes de código y herramientas automáticas que colaboren en el proyecto **SecondBrain RAG**.

El objetivo es mantener una base de código limpia, documentada, testeable y coherente con la arquitectura del proyecto.

---

## Contexto del proyecto

**SecondBrain RAG** es una aplicación Blazor/.NET que permite construir un segundo cerebro personal usando IA local, Ollama, embeddings y RAG.

El sistema permite almacenar notas, documentos y páginas wiki, generar embeddings, realizar búsqueda semántica y responder preguntas usando contexto recuperado desde una base vectorial.

---

## Objetivo del agente

Cuando trabajes en este repositorio, debes ayudar a:

- Implementar funcionalidades de forma incremental.
- Mantener una arquitectura limpia.
- Evitar acoplamientos innecesarios.
- Crear código C# claro, mantenible y testeable.
- Documentar decisiones importantes.
- Proponer pruebas cuando añadas lógica relevante.
- Respetar las convenciones del proyecto.
- No introducir dependencias sin justificación.

---

## Stack técnico previsto

- .NET
- C#
- Blazor
- Ollama
- PostgreSQL
- pgvector
- Docker
- Markdown
- RAG
- Embeddings
- Clean Architecture

---

## Estructura esperada del repositorio

```text
src/
├── SecondBrain.Web/
├── SecondBrain.Application/
├── SecondBrain.Domain/
├── SecondBrain.Infrastructure/
└── SecondBrain.Shared/

tests/
├── SecondBrain.UnitTests/
└── SecondBrain.IntegrationTests/

docs/
├── architecture/
├── decisions/
├── rag/
└── use-cases/
```

---

## Reglas generales

1. Prioriza soluciones simples.
2. No añadas complejidad prematura.
3. No mezcles lógica de dominio con infraestructura.
4. No coloques acceso a base de datos en componentes Blazor.
5. No llames directamente a Ollama desde la capa Web.
6. No generes código duplicado.
7. No uses singletons globales para lógica de negocio.
8. No ocultes errores importantes.
9. No inventes APIs inexistentes.
10. No modifiques múltiples áreas del proyecto sin explicar el motivo.

---

## Estilo de código C#

Usa C# moderno, claro y expresivo.

### Reglas

- Usa nombres descriptivos.
- Prefiere clases pequeñas.
- Prefiere métodos cortos.
- Usa `async`/`await` correctamente.
- Usa `CancellationToken` en operaciones I/O.
- Evita lógica compleja dentro de controladores, páginas o componentes.
- Usa interfaces solo cuando aporten desacoplamiento real.
- Evita abstracciones artificiales.
- Usa comentarios XML `///` solo cuando aporten valor.

### Ejemplo recomendado

```csharp
public sealed class SearchKnowledgeQueryHandler
{
    private readonly IEmbeddingService _embeddingService;
    private readonly IKnowledgeVectorRepository _repository;

    public SearchKnowledgeQueryHandler(
        IEmbeddingService embeddingService,
        IKnowledgeVectorRepository repository)
    {
        _embeddingService = embeddingService;
        _repository = repository;
    }

    public async Task<IReadOnlyList<SearchResultDto>> HandleAsync(
        SearchKnowledgeQuery query,
        CancellationToken cancellationToken)
    {
        var embedding = await _embeddingService.GenerateEmbeddingAsync(
            query.Text,
            cancellationToken);

        return await _repository.SearchSimilarAsync(
            embedding,
            query.TopK,
            cancellationToken);
    }
}
```

---

## Arquitectura

El proyecto debe seguir una separación por capas.

### Domain

Contiene entidades, value objects y reglas de negocio.

No debe depender de:

- Entity Framework Core.
- Dapper.
- Ollama.
- Blazor.
- PostgreSQL.
- APIs externas.

### Application

Contiene casos de uso, DTOs, contratos e interfaces.

Puede definir interfaces como:

- `IKnowledgeRepository`
- `IEmbeddingService`
- `IChatCompletionService`
- `IVectorSearchService`
- `IDocumentChunker`

No debe contener detalles concretos de infraestructura.

### Infrastructure

Contiene implementaciones técnicas:

- Repositorios.
- Cliente Ollama.
- Acceso a PostgreSQL.
- Implementación con pgvector.
- Lectores de documentos.
- Servicios externos.

### Web

Contiene la interfaz Blazor.

Debe llamar a casos de uso o servicios de aplicación, no a infraestructura directamente.

---

## Convenciones para RAG

Cuando implementes funcionalidades RAG, respeta este flujo:

```text
Entrada del usuario
    ↓
Normalización de texto
    ↓
Generación de embedding
    ↓
Búsqueda vectorial Top-K
    ↓
Construcción de contexto
    ↓
Prompt final
    ↓
Respuesta del LLM
    ↓
Referencias a fuentes usadas
```

---

## Reglas para embeddings

- No generes embeddings dentro de componentes Blazor.
- Centraliza la generación en un servicio de aplicación.
- Guarda metadatos del contenido original.
- Cada chunk debe mantener referencia a su documento o nota origen.
- El tamaño de chunk debe ser configurable.
- El número de resultados Top-K debe ser configurable.
- Evita reindexar contenido si no ha cambiado.

---

## Reglas para prompts

Los prompts deben estar versionados o localizados en una zona clara del proyecto.

Ubicaciones recomendadas:

```text
src/SecondBrain.Application/Prompts/
```

o

```text
docs/prompts/
```

Todo prompt importante debe indicar:

- Rol del asistente.
- Restricciones.
- Formato de salida esperado.
- Cómo actuar si falta contexto.
- Contexto recuperado.
- Pregunta del usuario.

### Regla crítica

El asistente no debe inventar información cuando el contexto recuperado no sea suficiente.

Debe responder algo equivalente a:

```text
No tengo información suficiente en tu base de conocimiento para responder con seguridad.
```

---

## Reglas para Blazor

- Mantén los componentes pequeños.
- Extrae lógica a servicios o casos de uso.
- Evita lógica de negocio en `.razor`.
- Usa view models cuando la pantalla empiece a crecer.
- Evita llamadas directas a base de datos.
- Evita llamadas directas a Ollama.
- Mantén una separación clara entre UI y aplicación.

---

## Reglas para base de datos

- Las migraciones deben ser explícitas.
- Los nombres de tablas deben ser claros.
- Los campos vectoriales deben estar documentados.
- Los índices vectoriales deben justificarse.
- No borres datos sin confirmación explícita.
- No cambies el esquema sin actualizar documentación si afecta al diseño.

---

## Reglas para Docker

- Mantén `docker-compose.yml` simple.
- Usa volúmenes persistentes para PostgreSQL.
- No incluyas secretos reales en el repositorio.
- Usa `.env` para configuración sensible.
- Documenta los comandos básicos para levantar servicios.

---

## Pruebas

Añade pruebas cuando implementes:

- Chunking.
- Generación de prompts.
- Normalización de texto.
- Ranking de resultados.
- Casos de uso de aplicación.
- Reglas de dominio.
- Transformaciones de datos.

### Estructura recomendada

```text
tests/
├── SecondBrain.UnitTests/
└── SecondBrain.IntegrationTests/
```

### Convenciones

- Las pruebas unitarias no deben depender de Ollama.
- Las pruebas unitarias no deben depender de PostgreSQL.
- Usa dobles de prueba para servicios externos.
- Las pruebas de integración pueden usar Docker si está justificado.

---

## Documentación

Toda decisión relevante debe documentarse en `/docs`.

Ejemplos:

```text
docs/architecture/clean-architecture.md
docs/rag/rag-pipeline.md
docs/rag/chunking-strategy.md
docs/rag/vector-search.md
docs/decisions/0001-use-ollama.md
docs/decisions/0002-use-postgresql-pgvector.md
```

---

## Formato para decisiones técnicas

Usa un formato tipo ADR:

```markdown
# ADR-0001: Usar Ollama para modelos locales

## Estado

Aceptada

## Contexto

...

## Decisión

...

## Consecuencias

...
```

---

## Seguridad y privacidad

Este proyecto puede almacenar conocimiento personal del usuario.

Por tanto:

- No registres contenido sensible en logs.
- No envíes datos personales a servicios externos sin indicarlo.
- Prioriza modelos locales.
- Evita telemetría innecesaria.
- No incluyas claves API en el repositorio.
- No uses datos reales en pruebas si pueden contener información privada.

---

## Configuración

Usa configuración externa para:

- URL de Ollama.
- Modelo de chat.
- Modelo de embeddings.
- Cadena de conexión.
- Tamaño de chunk.
- Overlap de chunk.
- Valor Top-K.
- Temperatura del modelo.

Ejemplo:

```json
{
  "Ollama": {
    "BaseUrl": "http://localhost:11434",
    "ChatModel": "qwen3",
    "EmbeddingModel": "mxbai-embed-large"
  },
  "Rag": {
    "ChunkSize": 800,
    "ChunkOverlap": 120,
    "TopK": 5,
    "Temperature": 0.2
  }
}
```

---

## Commits

Usa mensajes de commit claros.

Ejemplos:

```text
feat: add knowledge note entity
feat: implement semantic search use case
fix: avoid reindexing unchanged notes
docs: add RAG pipeline documentation
test: add chunking service tests
```

---

## Pull Requests

Cada PR debe incluir:

- Qué cambia.
- Por qué cambia.
- Cómo se ha probado.
- Capturas si afecta a UI.
- Documentación actualizada si aplica.

---

## Tareas recomendadas para agentes

Cuando recibas una tarea grande:

1. Resume el objetivo.
2. Identifica archivos afectados.
3. Propón un plan breve.
4. Implementa cambios pequeños.
5. Añade pruebas si procede.
6. Actualiza documentación si procede.
7. Explica los cambios realizados.

---

## Restricciones importantes

No hagas lo siguiente salvo petición explícita:

- Reescribir toda la arquitectura.
- Cambiar de base de datos.
- Sustituir Ollama por un proveedor cloud.
- Introducir frameworks pesados.
- Añadir autenticación compleja antes de tiempo.
- Añadir colas, microservicios o CQRS completo sin necesidad real.
- Mezclar código experimental con código principal.

---

## Prioridades del proyecto

Orden de prioridad:

1. Correctitud.
2. Simplicidad.
3. Mantenibilidad.
4. Testabilidad.
5. Privacidad.
6. Rendimiento.
7. Experiencia de usuario.

---

## Definición de terminado

Una tarea se considera terminada cuando:

- El código compila.
- La funcionalidad cumple el objetivo.
- La lógica importante tiene pruebas.
- No se han introducido dependencias innecesarias.
- La documentación se ha actualizado si aplica.
- La solución respeta las capas del proyecto.
- No se han dejado secretos ni datos sensibles.

---

## Notas para asistentes IA

Antes de modificar código:

- Revisa la estructura existente.
- Respeta nombres y convenciones ya presentes.
- No asumas que una carpeta existe si no la has comprobado.
- No elimines código sin explicar por qué.
- No ocultes limitaciones.
- Si una decisión tiene varias opciones razonables, explica el criterio elegido.

---

## Resumen

Trabaja como un desarrollador senior de C#/.NET especializado en arquitectura limpia, Blazor, RAG y sistemas locales de IA.

El resultado debe ser software mantenible, bien estructurado y útil para un Trabajo de Fin de Máster.
