# 🗳️ Atribución de Autoría en Tweets de opinologos y personalidades políticas

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-orange.svg)](https://pytorch.org/)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-yellow.svg)](https://huggingface.co/)
[![License](https://img.shields.io/badge/License-Academic%20Use-green.svg)]()

> **Clasificación automática de autores en tweets mexicanos usando embeddings contextuales y redes neuronales.**

Este repositorio contiene la implementación de un pipeline de **Procesamiento de Lenguaje Natural (PLN)** para la **clasificación de autoría de tweets**. El proyecto explora y compara diferentes modelos de **embeddings contextuales** y clasificadores (MLP y SVM) para identificar al autor de un tweet analizando exclusivamente su estilo de escritura.

Desarrollado como parte de una tesis de maestría en **Ciencia de Datos (INFOTEC)**.

---

## 📊 Resultados Clave

| Modelo | Accuracy | Macro F1 | Mejor Clase | Peor Clase |
|--------|----------|----------|-------------|------------|
| **MexSmall (MLP)** | **64.13%** | **0.61** | 0.90 (Clase 34) | 0.24 (Clase 16) |
| RoBERTuito (MLP) | 59.19% | 0.56 | 0.89 (Clase 34) | 0.17 (Clase 46) |
| SentenceTransformer (MLP) | ~58% | ~0.55 | - | - |
| SVM (Línea Base) | Pendiente | Pendiente | - | - |

**📌 El modelo MexSmall (especializado en tweets mexicanos) supera a RoBERTuito y SentenceTransformer en todos los aspectos, demostrando la importancia de la especialización lingüística y regional.**

---

## 🧠 Pipeline del Proyecto




---

## 📋 Descripción del Proyecto

El pipeline sigue un flujo de trabajo modular que abarca todo el ciclo de vida del proceso de clasificación:

1. **Preprocesamiento** de tweets (limpieza, normalización)
2. **Generación de embeddings contextuales** con tres modelos:
   - **SentenceTransformer**: Modelo general multilingüe
   - **MexSmall**: Modelo INFOTEC especializado en tweets mexicanos (140M tweets)
   - **RoBERTuito**: Modelo pysentimiento para español
3. **Clasificación** con dos enfoques:
   - **Red Neuronal (MLP)**: 4 capas ocultas con regularización
   - **SVM**: Con búsqueda de hiperparámetros
4. **Evaluación y comparativa** de resultados

---

## 📓 Descripción de los Notebooks

| Notebook | Descripción | Contenido |
|----------|-------------|-----------|
| **01_preprocesamiento** | Limpieza y preparación de tweets | Normalización, eliminación de ruido, etiquetado |
| **02_representaciones_vectoriales** | Generación de embeddings contextuales | SentenceTransformer, MexSmall (RoBERTuito en script externo) |
| **03_clasificacion_red_neuronal** | Clasificación con MLP en PyTorch | MLP 768-512-1024-512-256-47, BatchNorm, Dropout |
| **04_clasificacion_svm** | Clasificación con SVM | GridSearch, kernels, escalado |
| **05_comparativa_resultados** | Análisis comparativo | Tablas, gráficas, conclusiones |

---

## 🔬 Análisis de Resultados

### ¿Qué modelo de embeddings funciona mejor?

| Modelo | Ventajas | Desventajas | Accuracy |
|--------|----------|-------------|----------|
| **MexSmall** | Especializado en tweets mexicanos, captura jerga regional | Menos conocido, solo español de México | **64.13%** |
| **RoBERTuito** | Open-source, bien documentado, 500M tweets | No especializado en México, más lento | 59.19% |
| **SentenceTransformer** | Rápido, multilingüe, fácil de usar | Menos preciso, embeddings generales | ~58% |

### ¿Por qué MexSmall es el ganador?

1. **Especialización geográfica**: Entrenado en 140M tweets de México (2015-2023)
2. **Tokens regionales**: Captura variaciones dialectales y modismos
3. **Preprocesamiento específico**: Mantiene emojis, jerga, lenguaje coloquial
4. **Contexto cultural**: Entiende referencias locales mexicanas

### ¿Qué autores son más fáciles de clasificar?

Los autores con estilos de escritura más distintivos y consistentes:
- **Clase 34**: F1 de 0.89-0.90 en todos los modelos
- **Clase 12**: F1 de ~0.85

### ¿Qué autores son más difíciles?

- **Clase 46**: F1 de 0.17-0.39 (dependiendo del modelo)
- **Clase 16**: F1 de 0.24-0.26

### ¿Por qué MLP > SVM?

| Factor | MLP | SVM |
|--------|-----|-----|
| **Alta dimensionalidad (768 dims)** | ✅ La maneja naturalmente | ❌ Sufre la "maldición de la dimensionalidad" |
| **No linealidades** | ✅ Capas ocultas capturan relaciones complejas | ⚠️ Kernel ayuda, pero limitado |
| **Clases minoritarias** | ✅ Con class weights mejora significativamente | ❌ Tiende a aplastarlas |
| **Escalabilidad** | ✅ O(n) - eficiente | ❌ O(n²) - lento con 930K datos |

---

## 🛠️ Tecnologías Utilizadas

| Herramienta | Uso |
|-------------|-----|
| **SentenceTransformer** | Embeddings contextuales generales |
| **MexSmall** | Embeddings especializados en tweets mexicanos |
| **RoBERTuito** | Embeddings para español |
| **PyTorch** | Red Neuronal Multicapa (MLP) |
| **Scikit-learn** | SVM, métricas, GridSearch |
| **Pandas / NumPy** | Manipulación de datos |
| **Matplotlib / Seaborn** | Visualizaciones estáticas |
| **TQDM** | Barras de progreso |

---

## 📚 Contexto de Investigación

Este repositorio contiene la implementación experimental desarrollada para una tesis de maestría en **Ciencia de Datos e Información** en **INFOTEC** (Instituto de Formación e Investigación en Tecnologías de la Información).

### Modelos Evaluados

| Modelo | Origen | Especialización | Tamaño |
|--------|--------|-----------------|--------|
| **MexSmall** | INFOTEC | Tweets mexicanos | 140M tweets |
| **RoBERTuito** | pysentimiento | Tweets en español | 500M tweets |
| **SentenceTransformer** | HuggingFace | Multilingüe | General |

### Objetivo de la tesis

Comparar el desempeño de diferentes modelos de **embeddings contextuales** y clasificadores (MLP y SVM) en la tarea de clasificación de autoría de tweets en el contexto mexicano.

### Resultado principal

El modelo **MexSmall** (especializado en tweets mexicanos) combinado con una **red neuronal MLP** logra la mejor precisión (64.13%), demostrando que la especialización lingüística y regional es clave para este tipo de tareas.

---

## 📂 Estructura del Repositorio


---

## 📋 Descripción del Proyecto

El pipeline sigue un flujo de trabajo modular que abarca todo el ciclo de vida del proceso de clasificación:

1. **Preprocesamiento** de tweets (limpieza, normalización)
2. **Generación de embeddings contextuales** con tres modelos:
   - **SentenceTransformer**: Modelo general multilingüe
   - **MexSmall**: Modelo INFOTEC especializado en tweets mexicanos (140M tweets)
   - **RoBERTuito**: Modelo pysentimiento para español
3. **Clasificación** con dos enfoques:
   - **Red Neuronal (MLP)**: 4 capas ocultas con regularización
   - **SVM**: Con búsqueda de hiperparámetros
4. **Evaluación y comparativa** de resultados

---

## 📓 Descripción de los Notebooks

| Notebook | Descripción | Contenido |
|----------|-------------|-----------|
| **01_preprocesamiento** | Limpieza y preparación de tweets | Normalización, eliminación de ruido, etiquetado |
| **02_representaciones_vectoriales** | Generación de embeddings contextuales | SentenceTransformer, MexSmall (RoBERTuito en script externo) |
| **03_clasificacion_red_neuronal** | Clasificación con MLP en PyTorch | MLP 768-512-1024-512-256-47, BatchNorm, Dropout |
| **04_clasificacion_svm** | Clasificación con SVM | GridSearch, kernels, escalado |
| **05_comparativa_resultados** | Análisis comparativo | Tablas, gráficas, conclusiones |

---

## 🔬 Análisis de Resultados

### ¿Qué modelo de embeddings funciona mejor?

| Modelo | Ventajas | Desventajas | Accuracy |
|--------|----------|-------------|----------|
| **MexSmall** | Especializado en tweets mexicanos, captura jerga regional | Menos conocido, solo español de México | **64.13%** |
| **RoBERTuito** | Open-source, bien documentado, 500M tweets | No especializado en México, más lento | 59.19% |
| **SentenceTransformer** | Rápido, multilingüe, fácil de usar | Menos preciso, embeddings generales | ~58% |

### ¿Por qué MexSmall es el ganador?

1. **Especialización geográfica**: Entrenado en 140M tweets de México (2015-2023)
2. **Tokens regionales**: Captura variaciones dialectales y modismos
3. **Preprocesamiento específico**: Mantiene emojis, jerga, lenguaje coloquial
4. **Contexto cultural**: Entiende referencias locales mexicanas

### ¿Qué autores son más fáciles de clasificar?

Los autores con estilos de escritura más distintivos y consistentes:
- **Clase 34**: F1 de 0.89-0.90 en todos los modelos
- **Clase 12**: F1 de ~0.85

### ¿Qué autores son más difíciles?

- **Clase 46**: F1 de 0.17-0.39 (dependiendo del modelo)
- **Clase 16**: F1 de 0.24-0.26

### ¿Por qué MLP > SVM?

| Factor | MLP | SVM |
|--------|-----|-----|
| **Alta dimensionalidad (768 dims)** | ✅ La maneja naturalmente | ❌ Sufre la "maldición de la dimensionalidad" |
| **No linealidades** | ✅ Capas ocultas capturan relaciones complejas | ⚠️ Kernel ayuda, pero limitado |
| **Clases minoritarias** | ✅ Con class weights mejora significativamente | ❌ Tiende a aplastarlas |
| **Escalabilidad** | ✅ O(n) - eficiente | ❌ O(n²) - lento con 930K datos |

---

## 🛠️ Tecnologías Utilizadas

| Herramienta | Uso |
|-------------|-----|
| **SentenceTransformer** | Embeddings contextuales generales |
| **MexSmall** | Embeddings especializados en tweets mexicanos |
| **RoBERTuito** | Embeddings para español |
| **PyTorch** | Red Neuronal Multicapa (MLP) |
| **Scikit-learn** | SVM, métricas, GridSearch |
| **Pandas / NumPy** | Manipulación de datos |
| **Matplotlib / Seaborn** | Visualizaciones estáticas |
| **TQDM** | Barras de progreso |

---

## 📚 Contexto de Investigación

Este repositorio contiene la implementación experimental desarrollada para una tesis de maestría en **Ciencia de Datos e Información** en **INFOTEC** (Instituto de Formación e Investigación en Tecnologías de la Información).

### Modelos Evaluados

| Modelo | Origen | Especialización | Tamaño |
|--------|--------|-----------------|--------|
| **MexSmall** | INFOTEC | Tweets mexicanos | 140M tweets |
| **RoBERTuito** | pysentimiento | Tweets en español | 500M tweets |
| **SentenceTransformer** | HuggingFace | Multilingüe | General |

### Objetivo de la tesis

Comparar el desempeño de diferentes modelos de **embeddings contextuales** y clasificadores (MLP y SVM) en la tarea de clasificación de autoría de tweets en el contexto mexicano.

### Resultado principal

El modelo **MexSmall** (especializado en tweets mexicanos) combinado con una **red neuronal MLP** logra la mejor precisión (64.13%), demostrando que la especialización lingüística y regional es clave para este tipo de tareas.

---

## 📂 Estructura del Repositorio

