Prompt: Especificación para Regresión Lineal (TMDB 5000)

**Contexto:**  
"Actúa como un Ingeniero de Machine Learning Senior. El objetivo es desarrollar un pipeline completo de regresión lineal para predecir el éxito financiero (`revenue`) de películas utilizando el dataset TMDB 5000. El desarrollo debe ser modular, creando funciones reutilizables que sirvan de plantilla para futuros proyectos de análisis de datos."

**Especificaciones Técnicas por Fases:**

1. **Ingesta y Limpieza:**
    - Implementar la descarga vía `kagglehub` y carga de `tmdb_5000_movies.csv` y `tmdb_5000_credits.csv` (p. 1).
    - Realizar un `merge` por ID y limpiar columnas duplicadas (p. 1).
    - Filtrar registros con `budget` o `revenue` igual a 0 y asegurar el uso de `.copy()` para evitar advertencias de memoria (p. 2).
2. **Procesamiento de Datos Complejos (JSON):**
    - Crear una función robusta `parse_json_col` con manejo de errores (`try-except`) para extraer información de las columnas `genres`, `cast` y `crew` (p. 2).
    - Extraer el género principal (`main_genre`) y el nombre del director (p. 2).
3. **Análisis y Transformación:**
    - Realizar una transformación logarítmica usando `np.log1p()` para normalizar las distribuciones de `budget` y `revenue` (p. 3).
    - Generar visualizaciones clave: histogramas de distribución, scatter plots de correlación y una matriz de calor (heatmap) (p. 3).
4. **Modelado Iterativo:**
    - **Modelo 1 (Baseline):** Regresión lineal simple (`log_budget` vs `log_revenue`) (p. 3).
    - **Modelo 2 (Múltiple):** Incluir variables adicionales y aplicar `StandardScaler` para normalizar escalas (p. 4).
    - **Modelo 3 (Enriquecido):** Aplicar ingeniería de características creando variables _Dummies_ para géneros y calculando el `director_avg_revenue` (p. 4).
5. **Evaluación y Diagnóstico:**
    - Para cada modelo, calcular y tabular  R^2 y RMSE (p. 4).
    - Realizar un diagnóstico de residuos: gráfico de residuos vs. predichos e histograma de errores para verificar la normalidad (p. 3).

**Formato de Salida:**  
"Proporciona el código en bloques lógicos comentados. Cada sección debe incluir una breve explicación del 'porqué' de ese procedimiento para facilitar su comprensión y uso como plantilla futura."

---