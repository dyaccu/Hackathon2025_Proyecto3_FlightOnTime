# ✈️ FlightOnTime: Predicción de Retrasos de Vuelos

> **Proyecto 3 - Hackathon ONE 2025** > **Área:** Ciencia de Datos e Inteligencia Artificial.

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Libraries](https://img.shields.io/badge/Library-Scikit--Learn-orange)
![Status](https://img.shields.io/badge/Status-MVP%20Entregado-green)

## 📄 Descripción del Proyecto

**FlightOnTime** es una solución predictiva diseñada para estimar la probabilidad de retraso de un vuelo comercial antes de su despegue. 

Este repositorio contiene el componente de **Ciencia de Datos**, encargado de:
1.  Ingestar y limpiar datos históricos de vuelos.
2.  Generar variables predictivas (Feature Engineering).
3.  Entrenar un modelo de **Machine Learning (Random Forest)**.
4.  Exportar el modelo serializado para su integración en una API REST.

---

## 📂 Estructura del Repositorio

* `dataset_vuelos.csv`: Dataset histórico utilizado para el entrenamiento (Fuente: GitHub Raw).
* `Proyecto3_FlightOnTime_DataScience_Entregable.ipynb`: Notebook principal con todo el ciclo de vida del modelo (EDA, Entrenamiento, Evaluación).
* `flight_model.pkl`: El modelo entrenado y serializado (listo para producción).
* `model_columns.pkl`: Lista de columnas necesarias para replicar el One-Hot Encoding en producción.

---

## 🧠 Metodología y Modelo

### 1. Ingeniería de Características
Las fechas crudas (`2025-11-10T14:30:00`) no son útiles por sí solas. Se transformaron en variables numéricas que capturan patrones de tráfico aéreo:
* **Mes:** Para detectar estacionalidad (temporada alta/baja).
* **Día de la Semana:** Para diferenciar tráfico laboral vs. fin de semana.
* **Hora:** Para identificar horas pico de congestión.

### 2. Algoritmo Seleccionado
Se utilizó **Random Forest Classifier** por su capacidad para manejar variables categóricas (Aerolíneas, Origen, Destino) y su robustez contra el sobreajuste (overfitting).

### 3. Métricas de Desempeño
* **Exactitud (Accuracy):** ~77% (Capacidad global de acierto).
* **Salida:** Probabilidad (0.0 a 1.0) de retraso para cada vuelo.

---

## 🔌 Contrato de Integración (Data Science ↔ Back-End)

El modelo espera recibir los datos del vuelo y devuelve una predicción estructurada.

### Input Esperado (Variables)
El sistema de Back-End debe extraer estas variables del JSON del vuelo:
* `aerolinea` (Ej: "AR", "LA", "IB")
* `origen` (Ej: "EZE", "MAD")
* `destino` (Ej: "MIA", "GRU")
* `fecha_partida` (De aquí se deben extraer: Mes, Día, Hora).

### Output del Modelo (JSON)
El modelo entrega dos valores clave: la decisión binaria y la certeza.

```json
{
  "prevision": "Puntual",
  "probabilidad": 0.13
}

```

Nota: Si la probabilidad es > 0.5, la previsión cambia a "Retrasado".

### 🚀 Cómo ejecutar este proyecto localmente
**Prerrequisitos**
Tener instalado Python y las siguientes librerías:
```
Bash

pip install pandas scikit-learn ppscore joblib
```
**Cargar el modelo (Ejemplo en Python)**
Si deseas probar el modelo guardado (.pkl) fuera del notebook:

```Python

import joblib
import pandas as pd

# 1. Cargar artefactos
modelo = joblib.load('flight_model.pkl')
col_model = joblib.load('model_columns.pkl')

# 2. Crear dato de prueba (debe tener el mismo formato procesado)
# ... código de procesamiento ...

# 3. Predecir
probabilidad = modelo.predict_proba(dato_procesado)[0][1]
print(f"Probabilidad de retraso: {probabilidad:.2f}")
```

👥 Equipo
Rol: Data Scientist

Evento: Hackathon ONE 2025

Este proyecto fue desarrollado con fines educativos y de demostración para el Hackathon ONE.
