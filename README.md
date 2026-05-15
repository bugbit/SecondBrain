# SecondBrain RAG

**SecondBrain RAG** es una aplicación web desarrollada con **Blazor**, **.NET**, **Ollama** y técnicas de **RAG** (*Retrieval-Augmented Generation*) cuyo objetivo es funcionar como un segundo cerebro personal: un sistema para capturar, organizar, consultar y convertir conocimiento propio en una wiki viva asistida por IA.

El proyecto nace como Trabajo de Fin de Máster en desarrollo con Inteligencia Artificial y combina una interfaz web moderna, modelos locales de lenguaje, embeddings, búsqueda semántica y organización estructurada del conocimiento.

---

## Objetivo del proyecto

El objetivo principal es construir una aplicación que permita al usuario:

- Guardar notas, ideas, documentos, fragmentos técnicos y conocimiento personal.
- Organizar la información en categorías, etiquetas y páginas tipo wiki.
- Generar embeddings de los contenidos almacenados.
- Realizar búsquedas semánticas sobre el conocimiento propio.
- Consultar un asistente conversacional local basado en Ollama.
- Obtener respuestas fundamentadas en los documentos recuperados mediante RAG.
- Convertir información desordenada en una base de conocimiento estructurada.

---

## Idea principal

El sistema actúa como una combinación de:

- **Gestor de conocimiento personal**
- **Wiki inteligente**
- **Chatbot local con memoria documental**
- **Buscador semántico**
- **Asistente para aprendizaje y documentación técnica**

La aplicación no pretende ser solo un chat con IA, sino una herramienta para transformar información dispersa en conocimiento reutilizable.

---

## Funcionalidades principales

### Gestión de conocimiento

- Crear, editar y eliminar notas.
- Clasificar contenido por categorías.
- Añadir etiquetas.
- Crear páginas tipo wiki.
- Relacionar conceptos entre sí.
- Mantener una estructura de conocimiento navegable.

### Sistema RAG

- Fragmentación de documentos en chunks.
- Generación de embeddings.
- Almacenamiento vectorial.
- Búsqueda semántica Top-K.
- Recuperación de contexto relevante.
- Generación de respuestas usando un LLM local.

### Integración con Ollama

- Uso de modelos locales para generación de texto.
- Uso de modelos locales para embeddings.
- Independencia de servicios cloud externos.
- Mayor privacidad sobre los datos del usuario.

### Wiki asistida por IA

- Generación de borradores de páginas wiki.
- Resúmenes automáticos.
- Reorganización de notas.
- Extracción de conceptos clave.
- Propuesta de categorías y etiquetas.
- Conversión de conversaciones o notas sueltas en documentación estructurada.

---

## Arquitectura propuesta

El proyecto puede organizarse siguiendo una arquitectura limpia y modular:

```text
src/
├── SecondBrain.Web/              # Aplicación Blazor
├── SecondBrain.Application/      # Casos de uso
├── SecondBrain.Domain/           # Entidades y reglas de dominio
├── SecondBrain.Infrastructure/   # Persistencia, Ollama, vector store
└── SecondBrain.Shared/           # DTOs, contratos compartidos

tests/
├── SecondBrain.UnitTests/
└── SecondBrain.IntegrationTests/

docs/
├── architecture/
├── rag/
├── decisions/
└── use-cases/
```

---

## Componentes principales

### Blazor Web App

Interfaz principal del sistema. Permite gestionar notas, navegar por la wiki, consultar el asistente y visualizar resultados de búsqueda.

### Application Layer

Contiene los casos de uso de la aplicación:

- Crear nota.
- Actualizar nota.
- Indexar contenido.
- Buscar información.
- Preguntar al asistente.
- Generar página wiki.
- Reorganizar conocimiento.

### Domain Layer

Define el núcleo del sistema:

- Notas.
- Documentos.
- Categorías.
- Etiquetas.
- Chunks.
- Embeddings.
- Páginas wiki.
- Conversaciones.

### Infrastructure Layer

Implementa detalles técnicos:

- Base de datos.
- Repositorios.
- Cliente Ollama.
- Servicio de embeddings.
- Vector store.
- Lectura de documentos.
- Persistencia de conversaciones.

---

## Flujo RAG

El flujo básico del sistema es el siguiente:

```text
Usuario pregunta
      ↓
Se genera embedding de la pregunta
      ↓
Se buscan chunks similares en la base vectorial
      ↓
Se seleccionan los resultados Top-K
      ↓
Se construye un prompt con el contexto recuperado
      ↓
Ollama genera una respuesta
      ↓
La respuesta se muestra al usuario con referencias al contexto usado
```

---

## Tecnologías previstas

- **.NET**
- **Blazor**
- **C#**
- **Ollama**
- **PostgreSQL**
- **pgvector**
- **Docker**
- **Markdown**
- **RAG**
- **Embeddings**
- **Clean Architecture**

---

## Posible stack local

```text
Blazor Web App
      ↓
Application Services
      ↓
Ollama API
      ↓
Embedding Model
      ↓
PostgreSQL + pgvector
      ↓
Vector Search
```

---

## Ejemplo de casos de uso

### 1. Guardar una nota técnica

El usuario crea una nota sobre un tema aprendido, por ejemplo:

> Cómo usar pgvector con PostgreSQL en una aplicación .NET.

La aplicación guarda la nota, genera embeddings y la deja disponible para búsquedas posteriores.

### 2. Preguntar al asistente

El usuario pregunta:

> ¿Cómo configuré pgvector en Docker?

El sistema busca contenido relacionado en las notas guardadas y genera una respuesta basada en el conocimiento real almacenado.

### 3. Crear una página wiki

A partir de varias notas sueltas sobre un tema, la IA propone una página wiki organizada con:

- Introducción.
- Conceptos clave.
- Ejemplos.
- Enlaces internos.
- Resumen final.

---

## Estructura de datos inicial

Entidades candidatas:

- `KnowledgeItem`
- `Note`
- `WikiPage`
- `Category`
- `Tag`
- `DocumentChunk`
- `Embedding`
- `Conversation`
- `Message`
- `SourceReference`

---

## Objetivos académicos

Este proyecto permite demostrar competencias en:

- Desarrollo web moderno con Blazor.
- Integración de modelos de lenguaje locales.
- Diseño de sistemas RAG.
- Uso de bases de datos vectoriales.
- Arquitectura limpia.
- Procesamiento de lenguaje natural.
- Organización automática del conocimiento.
- Diseño de software mantenible.
- Documentación técnica de un sistema con IA.

---

## Roadmap inicial

### Fase 1: Base del proyecto

- Crear solución .NET.
- Crear aplicación Blazor.
- Configurar PostgreSQL con pgvector.
- Configurar Docker Compose.
- Crear entidades principales.
- Crear sistema básico de notas.

### Fase 2: Indexación

- Dividir documentos en chunks.
- Generar embeddings con Ollama.
- Guardar vectores en PostgreSQL.
- Implementar búsqueda semántica.

### Fase 3: Chat RAG

- Crear pantalla de chat.
- Recuperar contexto relevante.
- Construir prompts.
- Generar respuestas con Ollama.
- Mostrar fuentes utilizadas.

### Fase 4: Wiki inteligente

- Crear páginas wiki.
- Generar estructura automáticamente.
- Relacionar notas y conceptos.
- Proponer categorías y etiquetas.

### Fase 5: Pulido final

- Añadir pruebas.
- Mejorar experiencia de usuario.
- Crear documentación técnica.
- Preparar demo del TFM.
- Redactar memoria del proyecto.

---

## Ejemplo de configuración con Docker

```yaml
services:
  postgres:
    image: pgvector/pgvector:pg17
    container_name: secondbrain-postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: secondbrain
    ports:
      - "5432:5432"
    volumes:
      - secondbrain_pgdata:/var/lib/postgresql/data

volumes:
  secondbrain_pgdata:
```

---

## Ejemplo de prompt RAG

```text
Eres un asistente especializado en ayudar al usuario a consultar su segundo cerebro.

Responde únicamente usando el contexto proporcionado.
Si el contexto no contiene información suficiente, indícalo claramente.
No inventes datos.

Pregunta del usuario:
{{question}}

Contexto recuperado:
{{context}}
```

---

## Principios del proyecto

- Privacidad primero.
- IA local siempre que sea posible.
- Código simple y mantenible.
- Separación clara de responsabilidades.
- Documentación como parte del producto.
- Respuestas fundamentadas en fuentes.
- El conocimiento debe ser reutilizable, navegable y ampliable.

---

## Estado del proyecto

Proyecto en fase inicial de diseño y prototipado.

---

## Licencia

Pendiente de definir.
