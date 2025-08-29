# Laboratorio Práctico: De Prompt Simple a Prompt Avanzado

Este archivo contiene una serie de ejercicios diseñados para demostrar empíricamente la diferencia entre un prompt simple (ingenuo) y un prompt estructurado (avanzado). La meta es que puedas ejecutar ambos, comparar los resultados y validar la teoría aprendida en la guía principal.

---

## Instrucciones

Para cada ejercicio:
1.  **Copia y pega el "Prompt Simple"** en tu LLM de preferencia (ChatGPT, Claude, Gemini, etc.).
2.  **Guarda la respuesta** generada.
3.  **Copia y pega el "Prompt Avanzado"** en el mismo LLM.
4.  **Guarda la respuesta** generada.
5.  **Compara ambas respuestas** usando los "Criterios de Evaluación" sugeridos. Anota tus observaciones.

---

### Ejercicio 1: Generación de una Función de Código

**Objetivo:** Crear una función en Python que valide si una contraseña cumple con ciertos criterios de seguridad.

#### 1.A: Prompt Simple

```
Haz una función en Python para validar una contraseña.
```

#### 1.B: Prompt Avanzado

```
**Rol:** Eres un desarrollador de software senior especializado en ciberseguridad.

**Tarea:** Genera una función en Python llamada `validar_contrasena`.

**Contexto:** La función recibirá una cadena de texto (la contraseña) y debe retornar `True` si cumple con TODAS las siguientes reglas, o `False` si falla al menos una.

**Reglas de Validación (Criterios):**
1.  Debe tener una longitud mínima de 12 caracteres.
2.  Debe contener al menos una letra mayúscula (A-Z).
3.  Debe contener al menos una letra minúscula (a-z).
4.  Debe contener al menos un número (0-9).
5.  Debe contener al menos un carácter especial (ej: @, #, $, %, &, !).

**Formato de Salida:**
-   El bloque de código de la función en Python.
-   Debe incluir docstrings que expliquen qué hace la función, sus parámetros y qué retorna.
-   Añade 3 ejemplos de uso: uno con una contraseña válida y dos con contraseñas inválidas que demuestren diferentes fallos.

**Restricciones:**
-   No uses librerías externas, solo el core de Python.
-   El código debe ser legible y seguir las convenciones de estilo de PEP 8.
```

#### 1.C: Evaluación Comparativa

-   **Completitud:** ¿La función del prompt simple incluyó todas las reglas?
-   **Claridad:** ¿El código está bien documentado? ¿Es fácil de entender?
-   **Utilidad:** ¿El prompt avanzado proporcionó ejemplos de uso que facilitan la prueba y la integración?
-   **Robustez:** ¿La función maneja casos borde que el prompt simple podría haber ignorado?

---

### Ejercicio 2: Extracción de Información a Formato JSON

**Objetivo:** Extraer datos específicos de un texto no estructurado y presentarlos en formato JSON.

#### 2.A: Prompt Simple

```
Saca los datos importantes de este texto en JSON:

"La reunión de lanzamiento del Proyecto Fénix será el 15 de octubre de 2024 a las 10:30 AM en la Sala Júpiter. Los asistentes confirmados son Ana García (Líder de Proyecto), Carlos Rodríguez (Desarrollador Principal) y Elena Fernández (Diseñadora UX). El presupuesto asignado es de $25,000 USD."
```

#### 2.B: Prompt Avanzado

```
**Rol:** Eres un asistente de procesamiento de datos altamente preciso.

**Tarea:** Extrae la información clave del siguiente texto y formatéala estrictamente como un objeto JSON.

**Texto de Entrada:**
```
La reunión de lanzamiento del Proyecto Fénix será el 15 de octubre de 2024 a las 10:30 AM en la Sala Júpiter. Los asistentes confirmados son Ana García (Líder de Proyecto), Carlos Rodríguez (Desarrollador Principal) y Elena Fernández (Diseñadora UX). El presupuesto asignado es de $25,000 USD.
```

**Esquema JSON de Salida (Schema):**
Debes seguir esta estructura exacta. Si un campo no se encuentra en el texto, déjalo como `null`.

```json
{
  "proyecto": {
    "nombre": "string",
    "presupuesto": {
      "monto": "number",
      "moneda": "string"
    }
  },
  "reunion": {
    "fecha": "string (formato YYYY-MM-DD)",
    "hora": "string (formato HH:MM)",
    "ubicacion": "string"
  },
  "asistentes": [
    {
      "nombre": "string",
      "rol": "string"
    }
  ]
}
```

**Restricciones:**
-   El `monto` del presupuesto debe ser un número, no un string.
-   La `fecha` y `hora` deben ser normalizadas a los formatos especificados.
-   No incluyas campos que no estén en el esquema.
```

#### 2.C: Evaluación Comparativa

-   **Estructura:** ¿El JSON del prompt simple tiene una estructura lógica y predecible?
-   **Precisión:** ¿Los tipos de datos son correctos (números vs. strings)?
-   **Parseabilidad:** ¿El JSON generado por el prompt avanzado es directamente utilizable por una aplicación sin necesidad de limpieza o conversión?
-   **Manejo de Nulos:** ¿Cómo manejaría cada uno un texto donde, por ejemplo, el presupuesto no se menciona?

---

### Ejercicio 3: Escritura Creativa con Tono y Estilo

**Objetivo:** Escribir un párrafo corto sobre un tema complejo, adaptado a una audiencia específica.

#### 3.A: Prompt Simple

```
Escribe sobre los agujeros negros.
```

#### 3.B: Prompt Avanzado

```
**Rol:** Eres un divulgador científico como Carl Sagan, capaz de explicar conceptos complejos con asombro y claridad.

**Tarea:** Escribe un párrafo corto (aproximadamente 100 palabras) sobre los agujeros negros.

**Audiencia:** Niños de 10 a 12 años.

**Tono y Estilo:**
-   Usa un lenguaje sencillo y evocador.
-   Evita la jerga técnica y las fórmulas matemáticas.
-   Utiliza analogías o metáforas fáciles de entender (ej: "un aspirador cósmico del que ni la luz puede escapar").
-   El tono debe ser de misterio y fascinación, no de miedo.

**Formato de Salida:**
-   Un único párrafo de texto.

**Restricción:**
-   No menciones la "singularidad" o la "relatividad general" directamente. Enfócate en el fenómeno observable.
```

#### 3.C: Evaluación Comparativa

-   **Adecuación a la Audiencia:** ¿Qué respuesta es más apropiada y comprensible para un niño?
-   **Tono:** ¿Logró el prompt simple capturar el tono de asombro solicitado?
-   **Creatividad:** ¿Qué respuesta es más original y memorable?
-   **Cumplimiento de Restricciones:** ¿El prompt avanzado logró evitar la jerga técnica como se le pidió?

---

### Conclusión de los Ejercicios

Al realizar estos laboratorios, deberías notar que los prompts avanzados no solo dan respuestas "mejores", sino que producen resultados:
-   **Más predecibles y consistentes.**
-   **Más fáciles de integrar en flujos de trabajo automatizados.**
-   **Alineados con objetivos de negocio o de producto muy específicos.**
-   **Que requieren menos post-procesamiento manual.**

¡La ingeniería de prompts es el arte de la especificación precisa para obtener resultados de alta calidad!
