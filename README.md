# 📝 Evaluación de Prompts - Docente JavaScript Junior
##  Julian Santamaria
## 📖 Descripción del Proyecto

Este proyecto utiliza **Promptfoo** para realizar pruebas unitarias automatizadas (**Prompt Unit Testing**) sobre **30 preguntas clave de JavaScript Vanilla**. El objetivo es asegurar que el modelo de lenguaje (Gemini) mantenga un **rol pedagógico consistente** (Docente para Junior de 17-24 años) y que las respuestas cumplan con un **formato de salida estructurado** (Explicación Teórica + Caso de Estudio Codificable).

---

## 🎯 Objetivos del Proyecto

- ✅ **Validar consistencia pedagógica**: Verificar que el modelo mantenga un tono didáctico apropiado para estudiantes junior
- ✅ **Asegurar formato estructurado**: Garantizar que cada respuesta incluya teoría + ejemplo práctico codificable
- ✅ **Cobertura conceptual completa**: Evaluar 30 conceptos fundamentales de JavaScript Vanilla
- ✅ **Detección de errores comunes**: Identificar respuestas que no cumplan con los criterios pedagógicos establecidos

---

## 🛠️ Configuración y Ejecución

### Requisitos Previos

- **Node.js** (v16 o superior)
- **Promptfoo** instalado globalmente:
  ```bash
  npm install -g promptfoo
  ```
- **Clave de API de Gemini** válida

### Configuración del Proveedor

El proveedor y la clave de API están configurados directamente dentro del archivo `promptfooconfig.yaml` para simplificar la ejecución:

| Parámetro | Valor |
|-----------|-------|
| **Modelo Utilizado** | `google:gemini-2.5-flash` |
| **API Key** | Configurada en `providers:config:apiKey` |
| **Max Tokens** | 2048 (ajustable según necesidad) |
| **Temperature** | 0.7 (balance entre creatividad y precisión) |

---

## 🚀 Comandos de Ejecución

### Ejecución Completa (Recomendada)

Para ejecutar la evaluación completa y evitar errores de tasa límite (`Rate Limit 429`), utilice el siguiente comando que **limita la concurrencia** y añade un **retraso entre llamadas**:

```bash
# Ejecución segura con control de velocidad
promptfoo eval --max-concurrency 1 --delay 2000 --no-cache
```

| Parámetro | Propósito |
|-----------|-----------|
| `--max-concurrency 1` | Ejecuta las pruebas **una a una** para evitar saturación de la API |
| `--delay 2000` | Espera **2 segundos** entre cada llamada (2000ms) |
| `--no-cache` | Ignora resultados anteriores y fuerza nuevas llamadas (esencial tras corregir errores) |

### Ejecución Rápida (Solo Desarrollo)

Si ya validaste la configuración y solo necesitas probar cambios menores:

```bash
promptfoo eval --max-concurrency 3 --delay 1000
```

### Visualización de Resultados

Una vez finalizada la ejecución, abra la **interfaz web interactiva** para analizar resultados detallados:

```bash
promptfoo view --yes
```

**Funcionalidades de la interfaz:**
- 📊 Ver tasas de aprobación por test
- 🔍 Inspeccionar respuestas completas del modelo
- ⚠️ Identificar fallos de aserciones específicas
- 📈 Comparar rendimiento entre ejecuciones

---

## 📋 Estructura del Proyecto

```
proyecto-evaluacion-js/
│
├── promptfooconfig.yaml      # Configuración principal de Promptfoo
├── README.md                  # Este archivo

---

## 📋 Contenido de la Evaluación

### 1. Instrucción Base (Rol y Formato)

Todos los **30 casos de prueba** utilizan la **misma instrucción base** para mantener consistencia:

| Aspecto | Definición |
|---------|------------|
| **Rol (Actuar como)** | Docente de programación para estudiantes de 17-24 años con enfoque en JavaScript |
| **Contexto** | Estudiante inicial de Vanilla JavaScript con dificultades en conceptos fundamentales |
| **Formato de Salida** | Explicación pedagógica + Caso de estudio + Mezcla de teoría, ejemplos codificables y texto argumentativo |
| **Tono** | Amigable, paciente, didáctico, sin tecnicismos innecesarios |

### 2. Casos de Prueba (30 Tests Unitarios)

El archivo `promptfooconfig.yaml` contiene **30 tests unitarios**, cada uno enfocado en un concepto fundamental de JavaScript. A continuación, un resumen de los temas evaluados:

| # | Tema | Pregunta Específica (`{{question}}`) | Aserción Requerida (`icontains`) |
|---|------|--------------------------------------|----------------------------------|
| **1** | Hoisting | Explícame, paso a paso, cómo funciona el `hoisting` en JavaScript... | `eleva` Y `var` |
| **2** | `var`, `let`, `const` | Ayúdame a entender la diferencia entre `var`, `let` y `const`. | `scope de bloque` |
| **3** | TDZ/Error `let` | ¿Por qué el siguiente código da error y cómo podría corregirse? | `Dead Zone` Y `antes de inicializar` |
| **4** | Closures | ¿Qué es un closure en JavaScript y para qué sirve? | `función interna` Y `accede` |
| **5** | `this` en funciones | ¿Por qué `this` no funciona como esperaba en mi código? | `contexto` Y `flecha` |
| **9** | `return` implícito | Tengo una función que no devuelve el valor esperado, ¿por qué? | **llm-rubric** (evalúa problema de return y punto y coma) |
| **12** | Arrow Functions | ¿Cuál es la diferencia entre funciones tradicionales y arrow functions? | `this` Y `léxico` |
| **17** | Interpretado/Single-threaded | ¿Qué significa que JavaScript sea interpretado y de un solo hilo? | `Event Loop` |
| **22** | Event Bubbling | ¿Qué es el event bubbling y cómo puedo controlarlo? | `stopPropagation` |
| **30** | Validación Formulario | Enséñame a validar un formulario simple (nombre y correo). | `e.preventDefault` |

### 3. Tipos de Aserciones Utilizadas

El proyecto utiliza **múltiples tipos de aserciones** para validar diferentes aspectos:

| Tipo | Uso | Ejemplo |
|------|-----|---------|
| `icontains` | Verifica que la respuesta contenga términos clave específicos | `"eleva" AND "var"` |
| `llm-rubric` | Evaluación semántica compleja usando otro LLM como juez | Evalúa si explica correctamente el problema de `return` implícito |
| `not-icontains` | Asegura que NO se usen términos inapropiados | Evitar jerga excesivamente técnica |
| `javascript` | Valida que el código generado sea sintácticamente correcto | Verifica ejemplos ejecutables |

---

## 📊 Interpretación de Resultados

### Métricas Clave

Al ejecutar `promptfoo view`, verás las siguientes métricas:

| Métrica | Descripción | Meta |
|---------|-------------|------|
| **Pass Rate** | % de tests aprobados | ≥ 70% |
| **Assertion Failures** | Cantidad de aserciones fallidas por test | 0 por test |
| **Avg Response Length** | Longitud promedio de respuestas | 800-1500 caracteres |
| **Token Usage** | Tokens consumidos por evaluación | Monitorear costos |

### Estados de Tests

- ✅ **PASS**: Todas las aserciones aprobadas
- ❌ **FAIL**: Una o más aserciones fallaron
- ⚠️ **WARN**: El test pasó pero con advertencias
- ⏭️ **SKIP**: Test omitido (configuración específica)

---

## 🔧 Solución de Problemas Comunes

### Error 429 (Rate Limit Exceeded)

**Síntoma**: Errores de tasa límite al ejecutar múltiples tests
**Solución**:
```bash
promptfoo eval --max-concurrency 1 --delay 3000 --no-cache
```

### Respuestas Inconsistentes

**Síntoma**: El modelo no mantiene el rol pedagógico
**Solución**: Revisar y fortalecer la instrucción base en `promptfooconfig.yaml`

### Aserciones Fallidas Sistemáticas

**Síntoma**: Un test falla consistentemente
**Solución**: 
1. Revisar si la aserción es demasiado estricta
2. Verificar que el modelo tenga suficiente contexto
3. Ajustar el `temperature` si las respuestas son muy variables

---

## 🎓 Próximos Pasos

1. **Ampliar cobertura**: Añadir tests para ES6+, Async/Await, Modules
2. **Comparación de modelos**: Evaluar Gemini vs Claude vs GPT-4
3. **Métricas pedagógicas**: Añadir aserciones sobre claridad, ejemplos prácticos
4. **CI/CD**: Integrar con GitHub 