# 🗳️ Atribución de Autoría en Tweets de opinologos y personalidades políticas

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-orange.svg)](https://pytorch.org/)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-yellow.svg)](https://huggingface.co/)
[![License](https://img.shields.io/badge/License-Academic%20Use-green.svg)]()

# 📋 Contexto del Problema

En un contexto global marcado por la desinformación y la polarización política (Foro Económico Mundial, 2024), las redes sociales han dado lugar a la figura del opinólogo: aquel que "habla sin saber, cuyas intervenciones causan estragos en el avance de proyectos" (Tandem, 2021).

Este proyecto busca contribuir a mitigar este fenómeno mediante una herramienta de Ciencia de Datos que permita:
- Clasificar la autoría de un tweet basándose en rasgos estilísticos
- Verificar el origen de una opinión en el ecosistema digital
- Contextualizar el discurso de figuras públicas en México y Latinoamérica

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

## 🧠 Pipeline del Proyecto


<img src="https://raw.githubusercontent.com/sullivansantana/atribucion-autoria-tweets-esp/main/imagenes/pipeline.jpg" alt="Pipeline del proyecto" width="600">

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

### Modelos Evaluados

| Modelo | Origen | Especialización | Tamaño |
|--------|--------|-----------------|--------|
| **MexSmall** | INFOTEC | Tweets mexicanos | 140M tweets |
| **RoBERTuito** | pysentimiento | Tweets en español | 500M tweets |
| **SentenceTransformer** | HuggingFace | Multilingüe | General |

---

## 📂 Estructura del Repositorio

```txt
atribucion-autoria-tweets-esp/
├── notebooks/
│   ├── 01_preprocesamiento.ipynb
│   ├── 02_representaciones_vectoriales.ipynb
│   ├── 03_clasificacion_red_neuronal.ipynb
│   ├── 04_clasificacion_svm.ipynb
│   └── 05_comparativa_resultados.ipynb
├── data/
│   ├── Raw/                         # Datos crudos (CSVs en Drive)
│   │   └── Link.txt                 # Acceso a datos completos
│   └── embeddings/
│       ├── SentenceTransformer/
│       │   └── Link.txt             # Embeddings en Drive
│       └── mex_state/
│           └── Link.txt             # Embeddings en Drive
├── imagenes/                        # Imágenes para documentación
├── resultados/                      # Figuras y visualizaciones
├── .gitignore
└── README.md
```
---

## 🔬 Análisis de Resultados

### 📊 Resultados Clave

| Modelo | Accuracy | Macro F1 | Mejor Clase | Peor Clase |
|--------|----------|----------|-------------|------------|
| **MexSmall (MLP)** | **64.13%** | **0.61** | 0.90 (Clase 34) | 0.24 (Clase 16) |
| RoBERTuito (MLP) | 59.19% | 0.56 | 0.89 (Clase 34) | 0.17 (Clase 46) |
| SentenceTransformer (MLP) | ~58% | ~0.55 | - | - |
| SVM (Línea Base) | Pendiente | Pendiente | - | - |

**📌 El modelo MexSmall (especializado en tweets mexicanos) supera a RoBERTuito y SentenceTransformer en todos los aspectos, demostrando la importancia de la especialización lingüística y regional.**

### ¿Porque funciona mejor este modelo de embeddings ?

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

### Conclusion

El modelo **MexSmall** (especializado en tweets mexicanos) combinado con una **red neuronal MLP** logra la mejor precisión (64.13%), demostrando que la especialización lingüística y regional es clave para este tipo de tareas.





