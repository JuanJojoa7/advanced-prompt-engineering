# Prompt Engineering Avanzado

Guía-taller participativa para aprender, practicar y evidenciar el impacto de diseñar prompts estructurados frente a preguntas simples en modelos de lenguaje (LLMs) como GPT-4/5 (futuro), Claude, Gemini, Llama, Mistral, etc.

---
## 1. Objetivos
Al finalizar podrás:
- Comprender qué ocurre “por detrás” cuando llamas a un LLM.
- Diferenciar tipos de prompts y roles (system, user, assistant, herramientas).
- Identificar fuentes de ruido y pérdidas de contexto.
- Diseñar prompts estructurados reutilizables (plantillas, frameworks).
- Iterar, evaluar y comparar calidad de respuestas (evidencia empírica).
- Aplicar técnicas avanzadas (Chain-of-Thought, ReAct, Self-Consistency, Tree-of-Thought, JSON enforced, Guardrails).
- Documentar y versionar prompts como activos de ingeniería.

---
## 2. Índice Rápido
1. Objetivos
2. Fundamentos del ciclo de inferencia
3. Tipos y roles de prompts
4. Anatomía de un prompt efectivo
5. Fuentes de ruido y degradación
6. Frameworks / Plantillas reutilizables
7. Técnicas avanzadas (con ejercicios)
8. Laboratorios comparativos (naive vs estructurado)
9. Evaluación y métricas de calidad
10. Gestión de memoria y contexto (olvidos aparentes)
11. Prompt Libraries y asistencia (GPT, Claude, otros)
12. Buenas prácticas de ingeniería
13. Checklist rápido de refinamiento
14. Referencias bibliográficas

---
## 3. Fundamentos: ¿Qué pasa cuando llamas a un LLM?
Flujo simplificado:
1. Tu texto (system + historial + user) se tokeniza (fragmentos subpalabra).
2. Se trunca o ajusta al límite del contexto (context window).
3. El modelo genera tokens secuenciales condicionados a todos los anteriores (atención). Cada token nuevo se realimenta.
4. Se aplican capas de filtrado / políticas (seguridad, moderación, formatos) según proveedor.
5. Se retorna la secuencia. Algunas APIs opcionalmente transmiten en streaming.

Razón de “se olvidó”:
- El contexto excede ventana → se descartan partes iniciales.
- Cambiaste la instrucción sin reforzar restricciones previas.
- Ambigüedad / colisión de reglas entre system vs user.
- Sobrecarga de ejemplos irrelevantes (ruido semántico).

---
## 4. Roles de Mensaje
- System: Marco, identidad, políticas invariables (define personalidad y límites).
- Developer (algunas APIs): Instrucciones internas de la app.
- User: Intención puntual / tarea específica.
- Assistant: Respuestas generadas; también actúa como memoria conversacional.
- Tool / Function Calls: Canal estructurado para delegar a herramientas externas.

Recomendación: Mantén el system breve, estable y testeado; versiona variantes.

---
## 5. Tipos de Prompt (Taxonomía práctica)
- Directo simple: “Explícame X”.
- Contextualizado: Añade objetivo, audiencia, tono.
- Ejemplificado (Few-Shot): Provee 1–N ejemplos con formato esperado.
- Estructurado / Plantilla: Secciones claras (Rol, Objetivo, Datos, Formato, Criterios, Restricciones, Pasos, Validación).
- Cadena de razonamiento (Chain-of-Thought): Pedir que explique pasos intermedios (no siempre mostrar al usuario final).
- Programático / Parametrizado: Placeholder variables ({{variable}}) inyectadas en runtime.
- Multi-turn plan & refine: Primero plan, luego ejecución.
- ReAct: Alterna razonamiento y acciones (llamadas herramientas).
- Evaluador / Crítico: Prompt meta para auditar otra respuesta.
- Auto-reflexivo / Self-Improve: “Evalúa tu salida frente a criterios y reescribe”.
- JSON / Output Constrained: Forzar formato parseable.
- Guardrails / Políticas: Rechazar tareas fuera de alcance.

---
## 6. Anatomía de un Prompt Estructurado
Secciones recomendadas:
1. Rol / Contexto.
2. Objetivo claro (1 frase).
3. Entrada explícita (datos, supuestos). Si algo falta: pedirlo.
4. Formato de salida (ej. JSON con schema + validación).
5. Criterios de calidad (ex.: exactitud factual, concisión <300 palabras, tono neutral).
6. Proceso sugerido (pasos numerados). (Fomenta razonamiento).
7. Restricciones (idioma, evitar jergas, citar fuentes).
8. Ejemplos positivos / negativos.
9. Mecanismo de verificación / auto-revisión.

---
## 7. Fuentes de Ruido
- Ambigüedad léxica (“resuélvelo” sin definir métrica de éxito).
- Mezcla de idiomas sin aclarar preferencia.
- Instrucciones contradictorias en mensajes distintos.
- Ejemplos irrelevantes o demasiado largos.
- Temperatura alta para tareas de precisión.
- Falta de formato → dificulta parseo y automatización.

Mitigación: Limpieza, normalización, chunking, delimitar con triple backticks ``` para separar bloques.

---
## 8. Frameworks / Plantillas
Plantilla genérica:
```
SYSTEM: Eres un <rol> experto en <dominio>. Sigue estrictamente el formato indicado.

USER:
Objetivo: <en una frase>
Datos de entrada:
```
<bloque_datos>
```
Formato de salida (JSON):
{
  "resumen": string,
  "pasos": string[],
  "riesgos": string[]
}
Restricciones: Español neutral, máximo 250 palabras en resumen.
Proceso: 1) Analiza 2) Enumera riesgos 3) Sintetiza.
Validación: Si faltan datos críticos, pide aclaración antes de responder.
```

Mini-frameworks conocidos:
- RTF (Role, Task, Format)
- CRISPE (Capacity, Role, Insight, Structure, Persona, Examples)
- IDEA (Intent, Data, Examples, Answer format)
- PASSPORT (Persona, Audience, Style, Scope, Process, Output, Restrictions, Testing)

---
## 9. Técnicas Avanzadas (con Ejercicios)
1. Chain-of-Thought (CoT)
   - Ejercicio: Resolver un problema lógico con y sin “muestra tu razonamiento paso a paso”. Compara exactitud.
2. Self-Consistency
   - Generar múltiples cadenas y escoger mayoría. (Simular con 3 ejecuciones).
3. ReAct
   - Alternar: Pensamiento → Acción (buscar) → Observación → Pensamiento → Respuesta.
4. Tree-of-Thought / Branching
   - Pedir 3 enfoques, evaluar y seleccionar mejor.
5. Auto-Reflexión
   - Prompt secundario: “Evalúa la respuesta según criterios X y reescribe”.
6. Output Guardrails
   - Forzar schema JSON y post-validar; re-prompt si no cumple.
7. Tool Augmentation
   - Simular: Modelo planifica → pide ejecución de función (ej. calculadora) → integra resultado.

---
## 10. Laboratorios Comparativos
### Laboratorio A: Resumen Técnico
Problema: “Resume un artículo sobre Transformers para estudiantes de pregrado”.

Prompt Naive:
> Resume los transformers.

Prompt Estructurado:
```
Rol: Eres un profesor de IA.
Objetivo: Producir un resumen pedagógico del paper ‘Attention Is All You Need’. 
Audiencia: Estudiantes de 6º semestre (base en redes neuronales, no en lingüística avanzada).
Extensión: 180-220 palabras.
Debe incluir: 1) Problema previo, 2) Idea central de atención, 3) Ventajas sobre RNN, 4) Limitaciones.
Formato: JSON {"resumen": string, "bullet_points": string[]}
Proceso: 1) Identifica secciones clave, 2) Sintetiza, 3) Valida límite de palabras.
Validación: Si faltan datos del paper, dilo.
Idioma: Español neutral.
```
Actividad: Ejecuta ambos y evalúa con criterios: cobertura, estructura, parseabilidad.

### Laboratorio B: Refactor de Código
Prompt Naive: “Mejorar este código Python”. (Código de ejemplo cualquiera.)

Prompt Estructurado incorpora: objetivo (rendimiento), métricas (complejidad ciclomática), constraints (mantener API), formato (diff unificado), pasos (analizar → proponer → diffs → explicación breve). 

### Laboratorio C: Razonamiento Matemático
Compara exactitud sin CoT vs con CoT.

### Registro de Evidencia
Crea una tabla (manual) con columnas: Tarea | Prompt versión | Métrica | Resultado | Observaciones.

---
## 11. Evaluación y Métricas
- Exactitud factual (fuentes confiables cruzadas).
- Cobertura (porcentaje de requisitos satisfechos).
- Estructura / Formato (parse JSON sin errores).
- Brevedad / Longitud (tokens vs límite).
- Consistencia (varianza entre ejecuciones).
- Latencia (tiempo de respuesta) vs complejidad del prompt.
- Costo (tokens prompt + completion).

Automatizable: scripts que puntúan parseabilidad, longitud, presencia de campos.

---
## 12. Gestión de Memoria y Olvidos
Estrategias:
- Resumen incremental del historial (rolling summary) para liberar tokens.
- Anclar reglas críticas repitiéndolas condensadas cada N turnos.
- Externalizar conocimiento largo en vector store y solo inyectar fragmentos relevantes (retrieval-augmented generation, RAG).
- Limitar ejemplos a los mínimos discriminativos.

---
## 13. Modelos y Sugerencias (Prompt Libraries)
- GPT-4 / GPT-4o / (futuro GPT-5): Soporte amplio de herramientas, JSON modes.
- Claude (Anthropic): Ventanas grandes (contexto extenso) → ideal para documentos largos; acepta instancias de “Constitutional AI” (explícito en políticas).
- Gemini (Google): Integración multimodal profunda.
- Llama / Mistral (open weights): Personalización y fine-tuning local.
- Herramientas: PromptHub, OpenAI Prompt Examples, Anthropic Guides, Awesome Prompting repos.

Buenas prácticas: Centralizar plantillas y versionarlas (Git) + incluir métricas de evaluación.

---
## 14. Buenas Prácticas de Ingeniería
- Versionar prompts (nombres semánticos, changelog).
- Añadir tests automatizados (snapshots + verificaciones de campos).
- Medir costo antes/después al refinar.
- Reutilizar placeholders parametrizados.
- Añadir validación post-respuesta (parser + re-prompt si falla).
- Incluir disclaimers de límites del modelo (no autoridad legal, etc.).

---
## 15. Checklist de Refinamiento Rápido
1. ¿Rol explícito?  
2. ¿Objetivo 1-línea?  
3. ¿Formato de salida definido?  
4. ¿Criterios de calidad medibles?  
5. ¿Datos relevantes aislados y delimitados?  
6. ¿Ejemplos minimalistas?  
7. ¿Restricciones (idioma, longitud) claras?  
8. ¿Paso de verificación / auto-chequeo?  
9. ¿Ambigüedades resueltas?  
10. ¿Se evitó ruido irrelevante?

---
## 16. Ejemplo Comparativo Resumido
Tarea: Generar plan de pruebas para API.

Prompt Naive: “Dame un plan de pruebas para una API de usuarios.”

Prompt Avanzado:
```
Rol: Ingeniero QA senior.
Objetivo: Diseñar plan de pruebas exhaustivo para API REST de usuarios.
Entrada:
Endpoints:
- POST /users {name,email,password}
- GET /users/{id}
- PATCH /users/{id}
- DELETE /users/{id}
Requisitos no funcionales: 200 req/s pico, 99.5% disponibilidad, RGPD.
Formato JSON:
{
  "casos_funcionales": [ {"id": string, "descripcion": string, "tipo": "positivo"|"negativo" } ],
  "pruebas_seguridad": string[],
  "pruebas_rendimiento": string[],
  "riesgos": string[]
}
Proceso: 1) Categoriza 2) Genera casos mínimos y límite 3) Añade riesgos.
Validación: Si faltan roles de autenticación, solicítalos.
Idioma: Español.
```
Evaluación: Mejor cobertura (negativos, seguridad), formato estructurado parseable.

---
## 17. Ejercicios Prácticos Adicionales
1. Redacta 3 versiones de un prompt para la misma tarea; mide tokens y calidad. 
2. Implementa script que calcule coste (prompt_tokens + completion_tokens * precio_modelo).
3. Diseña prompt evaluador que puntúe factualidad de una respuesta sobre un texto fuente.
4. Crea pipeline: Recuperar fragmentos (RAG) → Inyectar → Prompt estructurado → Validar JSON.
5. Aplica ReAct simulando herramienta “calc(x)”.

---
## 18. Limitaciones y Ética
- Sesgos heredados del entrenamiento.
- Alucinaciones: Necesario verificación externa.
- Privacidad: Minimizar datos sensibles en prompts (enmascarar / tokenizar).
- Transparencia: Registrar versión de prompt usada para cada decisión automatizada.

---
## 19. Referencias Bibliográficas y Recursos
Papers / Artículos Clave:
1. Vaswani et al. (2017). Attention Is All You Need.
2. Brown et al. (2020). Language Models are Few-Shot Learners (GPT-3).
3. Wei et al. (2022). Chain-of-Thought Prompting Elicits Reasoning.
4. Yao et al. (2022). ReAct: Synergizing Reasoning and Acting.
5. Kojima et al. (2022). Large Language Models are Zero-Shot Reasoners.
6. Wang et al. (2022). Self-Consistency Improves Chain of Thought.
7. Shinn et al. (2023). Reflexion: Language Agents with Verbal Reinforcement Learning.
8. Besta et al. (2024). Graph of Thoughts: Solving Complex Problems with LLMs.
9. Anthropic (2023). Constitutional AI: Harmlessness from AI Feedback.
10. OpenAI (2024). GPT-4 Technical Report.

Recursos Prácticos:
- OpenAI Cookbook (GitHub).
- Anthropic Prompt Engineering Guide.
- Google Prompting (PaLM/Gemini) Guides.
- Awesome Prompt Engineering (GitHub lists).
- PapersWithCode (búsqueda de técnicas de prompting).

---
## 20. Próximos Pasos
- Añadir scripts de evaluación automática.
- Construir carpeta /prompts con versiones y tests.
- Integrar RAG y validación de esquemas.

---
## 21. Licencia y Uso
Contenido educativo. Verificar términos de uso de cada API antes de producción.

---
## 22. Resumen Final
Un prompt avanzado funciona como especificación técnica: reduce ambigüedad, aumenta control, mejora reproducibilidad y permite evaluar. Tratar los prompts como código (versionar, testear, medir) eleva la calidad y confiabilidad de sistemas basados en LLM.
