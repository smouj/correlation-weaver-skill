name: correlation-weaver
version: 2.1.0
description: Motor avanzado de descubrimiento de correlaciones estadísticas y análisis de patrones para conjuntos de datos complejos
author: OpenClaw Analytics Team
tags:
  - correlation
  - statistics
  - patterns
  - data-mining
  - time-series
  - feature-engineering
category: analysis
dependencies:
  - python>=3.9
  - numpy>=1.21.0
  - pandas>=1.3.0
  - scipy>=1.7.0
  - scikit-learn>=0.24.0
  - matplotlib>=3.4.0
  - seaborn>=0.11.0
  - networkx>=2.6.0
  - statsmodels>=0.13.0
  - dask[dataframe]>=2021.10.0
  - numba>=0.55.0
required_env_vars:
  - CORRELATION_WEAVER_MAX_MEMORY_GB (default: 4)
  - CORRELATION_WEAVER_N_JOBS (default: -1)
  - CORRELATION_WEAVER_SIGNIFICANCE_LEVEL (default: 0.05)
  - CORRELATION_WEAVER_CACHE_DIR (default: ~/.cache/corr-weaver)
capabilities:
  - pearson_correlation
  - spearman_correlation
  - kendall_tau
  - mutual_information
  - time_lagged_correlation
  - partial_correlation
  - cross_correlation
  - granger_causality
  - anova_analysis
  - chi_square_test
  - correlation_matrix_visualization
  - correlation_network_graph
  - bootstrap_confidence_intervals
  - hierarchical_clustering
  - feature_selection
compatible_workflows:
  - data-mining
  - predictive-modeling
  - anomaly-detection
  - feature-engineering
  - exploratory-data-analysis
timeout_seconds: 1800
```

# Correlation Weaver

Motor avanzado de descubrimiento de correlaciones estadísticas para identificar relaciones, patrones y dependencias no obvias en conjuntos de datos multidimensionales complejos.

## Propósito

Correlation Weaver va más allá de las correlaciones pairwise simples para descubrir patrones ocultos, relaciones con retardo, dependencias condicionales e interacciones multivariadas que el análisis estándar pasa por alto. Diseñado para científicos de datos, analistas e ingenieros que trabajan con series temporales, datos de sensores, métricas financieras o cualquier conjunto de datos donde las relaciones son complejas, no lineales o con retraso.

**Casos de Uso Reales:**

- Detectar indicadores líderes/rezagados en mercados financieros: Identificar qué indicadores económicos predicen movimientos de acciones con 3-5 días de antelación, incluso cuando las correlaciones inmediatas parecen aleatorias

- Predicción de fallos en sensores IoT: Encontrar que el sensor de temperatura A se correlaciona con el sensor de presión B 12 horas después, permitiendo sistemas de alerta temprana

- Descubrimiento de patrones de comportamiento del cliente: Descubrir que usuarios que realizan la acción X el martes tienen un 73% más de probabilidad de convertir si también vieron la página Y dentro de 48 horas (patrón temporal no obvio)

- Optimización de procesos de fabricación: Descubrir que ajustar el parámetro X de la máquina solo afecta la calidad del producto después de pasar por la máquina Y intermedia con un retraso de 45 minutos

- Atribución de marketing más allá del último clic: Identificar que los anuncios de display el jueves se correlacionan con conversiones de fin de semana, no con clics del mismo día

- Detección de riesgos en la cadena de suministro: Encontrar que los retrasos en las entregas del proveedor A predicen problemas de calidad del proveedor B 8 días después a través de restricciones logísticas compartidas

- Detección de anomalías de seguridad de red: Descubrir que secuencias específicas de patrones de inicio de sesión fallidos desde diferentes rangos de IP se correlacionan con brechas exitosas 72 horas después

## Alcance

**COMANDOS DENTRO DEL ALCANCE:**

```
correlation-weaver discover --dataset <ruta> --method <pearson|spearman|kendall|mutual|partial|cross|granger|all> [opciones]
  Descubre correlaciones usando método(s) estadístico(s) especificado(s). Soporta análisis de retardo temporal, pruebas de significancia e intervalos de confianza bootstrap.

correlation-weaver matrix --dataset <ruta> --threshold <flotante> --output <formato>
  Genera mapa de calor de matriz de correlación con filtrado de umbral configurable. Formatos de salida: png, svg, pdf, html_interactive.

correlation-weaver network --dataset <ruta> --min-correlation <flotante> --layout <circular|spring|shell|spectral|kamada_kawai>
  Construye grafo de red de variables correlacionadas. Destaca clusters, medidas de centralidad y estructura de comunidades.

correlation-weaver time-lag --dataset <ruta> --target <columna> --max-lag <entero> --step <entero> --method <ccf|granger>
  Analiza relaciones con retardo temporal para variable objetivo. Identifica períodos de retardo óptimos con significancia estadística.

correlation-weaver conditional --dataset <ruta> --condition-on <col1,col2> --target <col3> --method <partial|cond_mutual>
  Calcula correlaciones condicionales controlando variables de confusión. Revela relaciones directas vs indirectas.

correlation-weaver bootstrap --dataset <ruta> --iterations <entero> --confidence <flotante>
  Calcula intervalos de confianza bootstrap para todas las correlaciones. Identifica correlaciones estadísticamente inestables.

correlation-weaver feature-select --dataset <ruta> --target <columna> --method <fwrapper|boruta|rfe> --n-features <entero>
  Realiza selección de características basada en correlación para modelado predictivo. Devuelve lista de características rankeadas con valores p.

correlation-weaver cluster --dataset <ruta> --method <hierarchical|kmeans|dbscan> --distance <correlation|euclidean>
  Agrupa variables basándose en patrones de correlación. Identifica grupos de características correlacionadas para reducción de dimensionalidad.

correlation-weaver compare --dataset1 <ruta> --dataset2 <ruta> --method <stability|drift|ks_test>
  Compara estructuras de correlación entre conjuntos de datos (división temporal, test A/B, segmentos diferentes). Cuantifica deriva de correlación.

correlation-weaver export --results <ruta> --format <csv|json|pickle|excel> --include <lista>
  Exporta resultados de análisis en varios formatos. Puede incluir: correlations, p-values, confidence_intervals, network_data, cluster_assignments.

correlation-weaver validate --dataset <ruta> --ground-truth <ruta> --metric <precision|recall|f1>
  Valida correlaciones descubiertas contra relaciones conocidas. Reporta precision/recall de detección de correlación.

correlation-weaver batch --directory <ruta> --pattern <glob> --output-dir <ruta> --parallel <booleano>
  Procesa múltiples conjuntos de datos en batch. Genera reporte de correlación consolidado a través de todos los archivos.
```

**FUERA DEL ALCANCE:**

- Limpieza o preprocesamiento de datos (usar skill data-cleaner primero)
- Entrenamiento de modelos de machine learning (usar skill model-trainer)
- Inferencia causal que requiera experimentos controlados (usar skill causal-inference)
- Correlación en streaming en tiempo real (usar skill stream-processor)
- Aprendizaje de estructura de modelos gráficos más allá de redes de correlación

## Proceso de Trabajo Detallado

### 1. Fase de Preparación

**Validación de Entrada:**
- Verificar que el conjunto de datos existe en la ruta especificada
- Comprobar formato de archivo (soportados: CSV, Parquet, Feather, HDF5)
- Confirmar filas mínimas (por defecto: 100) y columnas mínimas (por defecto: 3)
- Validar que ninguna columna tiene >80% de valores faltantes
- Detectar y registrar tipos de datos para cada columna

**Configuración de Entorno:**
```bash
export CORRELATION_WEAVER_MAX_MEMORY_GB=8  # Ajustar según tamaño del dataset
export CORRELATION_WEAVER_N_JOBS=4        # Trabajos paralelos (-1 = todos los cores)
export CORRELATION_WEAVER_SIGNIFICANCE_LEVEL=0.01  # Umbral de valor p por defecto
mkdir -p ~/.cache/corr-weaver
```

**Estimación de Memoria:**
- Para dataset con n columnas: memoria ≈ (n² × 16 bytes) + (filas × columnas × 8 bytes)
- Ejemplo: 1000 columnas → ~16MB solo para matriz de correlación
- Si memoria estimada > CORRELATION_WEAVER_MAX_MEMORY_GB, cambiar automáticamente a backend Dask

### 2. Fase de Descubrimiento

**Lógica de Selección de Método:**
```bash
# Todos los métodos (más comprehensivo, más lento)
correlation-weaver discover --dataset data.csv --method all

# Pearson rápido para relaciones lineales
correlation-weaver discover --dataset data.csv --method pearson --threshold 0.7

# Spearman para relaciones monótonas no lineales
correlation-weaver discover --dataset data.csv --method spearman --threshold 0.6

# Información mutua para cualquier tipo de dependencia
correlation-weaver discover --dataset data.csv --method mutual --bins 10 --n_neighbors 5

# Correlación cruzada con retardo para series temporales
correlation-weaver discover --dataset timeseries.csv --method cross --max-lag 30 --step 1

# Causalidad de Granger para relaciones de pronóstico
correlation-weaver discover --dataset economic.csv --method granger --max-lag 10 --test ngranger
```

**Pruebas de Significancia:**
- Por defecto: valor p < 0.05 considerado significativo
- Corrección Bonferroni opcional: `--correction bonferroni`
- FDR de Benjamini-Hochberg: `--correction fdr`
- Bootstrap CI: `--bootstrap 1000 --ci 0.95`

**Manejo de Datos Faltantes:**
- Eliminación pairwise (por defecto): usar todos los pares disponibles
- Casos completos: `--complete-cases-only`
- Imputación: `--impute median|mean|knn|iterative` (no integrado; preprocesar primero)

### 3. Fase de Análisis

**Construcción de Matriz de Correlación:**
- Calcula solo triángulo superior (matriz simétrica)
- Diagonal establecida en 1.0
- Correlaciones faltantes marcadas como NaN
- Almacena tanto correlación cruda como valor p ajustado

**Generación de Grafo de Red:**
- Nodos: variables con correlaciones significativas
- Aristas: |correlación| >= umbral
- Ancho de arista: proporcional al valor absoluto de correlación
- Color de arista: positivo (azul) vs negativo (rojo)
- Tamaño de nodo: proporcional a degree centrality
- Comunidades: algoritmo Louvain para clustering

**Análisis de Retardo Temporal:**
- Calcula función de autocorrelación cruzada (CCF) para cada par en cada retardo
- Pruebas de causalidad de Granger para cada retardo
- Identifica pico de correlación para cada relación
- Almacena valor de retardo y correlación en ese retardo

**Correlación Condicional:**
- Correlación parcial: controla variables de confusión
- Información mutua condicional: dependencias condicionales no lineales
- Resultados: matriz de correlación condicional vs incondicional

### 4. Fase de Visualización

**Mapa de Calor de Matriz:**
```
correlation-weaver matrix \
  --dataset data.csv \
  --threshold 0.5 \
  --output svg \
  --annotate True \
  --cmap \"RdBu_r\" \
  --figsize 12 10 \
  --title \"Correlaciones de Features (|r| > 0.5)\" \
  --save correlations_heatmap.svg
```

**Grafo de Red:**
```
correlation-weaver network \
  --dataset data.csv \
  --min-correlation 0.7 \
  --layout spring \
  --node-coloring community \
  --edge-width-range 1 5 \
  --save network_graph.html \
  --interactive
```

**Gráfico de Retardo:**
```bash
correlation-weaver time-lag \
  --dataset timeseries.csv \
  --target sales \
  --max-lag 30 \
  --step 1 \
  --plot ccf_plot.png \
  --highlight-lags 7,14,21
```

### 5. Fase de Reporte

**Estructura de Resultados:**
```
results/
├── correlations.csv          # columnas: var1, var2, correlation, p_value, ci_lower, ci_upper
├── significant_pairs.json    # lista de pares significativos (var1,var2) con tamaño del efecto
├── network/
│   ├── nodes.csv            # variable, degree, betweenness, community
│   ├── edges.csv            # source, target, weight, lag (si existe)
│   └── graph.graphml        # Archivo NetworkX graph
├── plots/
│   ├── correlation_matrix.svg
│   ├── network_graph.html
│   ├── time_lag_analysis.png
│   └── conditional_heatmap.svg
└── summary.txt              # Hallazgos clave y recomendaciones
```

**Reporte de Resumen Incluye:**
- Número de variables analizadas
- Número de correlaciones significativas encontradas
- Correlaciones positivas/negativas más fuertes
- Relaciones con retardo identificadas
- Grupos de variables clusterizadas
- Características recomendadas para modelado predictivo
- Posibles problemas de calidad de datos (alta Missingness, valores constantes)

## Reglas de Oro

1. **Siempre validar suposiciones antes de correlación Pearson:**
   - Verificar normalidad con prueba Shapiro-Wilk o Kolmogorov-Smirnov
   - Si se viola, cambiar automáticamente a Spearman o transformar datos
   - Comando: `correlation-weaver discover --method pearson --check-assumptions`

2. **Cuidado con la paradoja de Simpson:**
   - Si una correlación se invierte al condicionar en una tercera variable, destacarlo
   - Siempre ejecutar análisis de correlación condicional en correlaciones overall fuertes
   - La bandera `--conditional-on` debe usarse cuando correlación overall > |0.5|

3. **Las series temporales requieren verificaciones de estacionariedad:**
   - Prueba Augmented Dickey-Fuller mandatoria para análisis de retardo temporal
   - Datos no estacionarios deben diferenciarse o des-trendarse primero
   - Flag: `--stationarity-check strict` (falla si no estacionario)

4. **La corrección por múltiples pruebas no es opcional:**
   - Con n variables, ~n²/2 pruebas realizadas
   - Siempre usar Bonferroni o FDR a menos que usuario sobrescriba explícitamente con `--no-correction`
   - Reportar tanto valores p crudos como valores p ajustados

5. **El patrón de datos faltantes importa:**
   - Si Missingness > 30% en cualquier columna, excluir del análisis
   - Si el patrón de Missingness no es aleatorio (MNAR), las estimaciones de correlación están sesgadas
   - Ejecutar prueba MCAR de Little: `correlation-weaver validate --missingness-test`

6. **Correlación ≠ causalidad:**
   - Cada archivo de salida debe incluir descargo de responsabilidad: \"Correlation does not imply causation\"
   - Para causalidad de Granger, declarar explícitamente que solo prueba precedencia predictiva, no causalidad real
   - Nunca reclamar causalidad en summary.txt

7. **Requisitos de tamaño de muestra:**
   - Mínimo 10 observaciones por variable para estimaciones de correlación confiables
   - Para n=1000 y 100 variables, cada correlación pairwise tiene ~1000 observaciones (eliminación pairwise)
   - Si tamaño efectivo de muestra < 50 para cualquier par, excluir del reporte

8. **Manejo de variables categóricas:**
   - One-hot encode automáticamente si usuario especifica `--auto-categorical`
   - Para variables ordinales, usar Spearman por defecto
   - Para variables nominales con >2 categorías, usar V de Cramér (via `--method association`)

9. **No data leakage en series temporales:**
   - Al calcular correlaciones rodantes, asegurar que ningún dato futuro se filtre al pasado
   - Para divisiones train/test, calcular correlaciones separadamente para evitar leakage
   - Usar `correlation-weaver compare --metric drift` para detectar inestabilidad temporal

10. **Siempre proporcionar intervalos de confianza:**
    - Bootstrap con al menos 1000 iteraciones para CI
    - Reportar IC 95% para todas las correlaciones significativas
    - Si CI incluye 0, la correlación no es robusta incluso si p < 0.05

## Ejemplos

### Ejemplo 1: Análisis de Mercado Financiero

**Entrada del usuario:**
```bash
correlation-weaver time-lag \
  --dataset stock_returns_2020_2025.csv \
  --target \"SP500_daily_return\" \
  --max-lag 20 \
  --step 1 \
  --method ccf \
  --significance-level 0.01 \
  --bootstrap 2000 \
  --output lagged_correlations.json \
  --plot sp500_lead_indicators.png
```

**Salida real:**
```json
{
  \"target\": \"SP500_daily_return\",
  \"analysis_period\": \"2020-01-01 to 2025-03-15\",
  \"total_observations\": 1332,
  \"significant_lagged_correlations\": [
    {
      \"predictor\": \"10year_treasury_yield\",
      \"optimal_lag\": -3,
      \"correlation_at_optimal_lag\": -0.234,
      \"p_value\": 0.0012,
      \"ci_95_lower\": -0.312,
      \"ci_95_upper\": -0.148,
      \"interpretation\": \"Treasury yield 3 days ago negatively predicts today's SP500 return\"
    },
    {
      \"predictor\": \"VIX_closing\",
      \"optimal_lag\": 0,
      \"correlation_at_optimal_lag\": -0.567,
      \"p_value\": \"<0.0001\",
      \"ci_95_lower\": -0.634,
      \"ci_95_upper\": -0.492,
      \"interpretation\": \"Same-day VIX strongly negatively correlated with SP500\"
    },
    {
      \"predictor\": \"oil_price\",
      \"optimal_lag\": 5,
      \"correlation_at_optimal_lag\": 0.089,
      \"p_value\": 0.042,
      \"ci_95_lower\": 0.003,
      \"ci_95_upper\": 0.174,
      \"interpretation\": \"Oil price 5 days later positively predicts SP500 (weak but significant)\"
    }
  ],
  \"warning\": \"All correlations bootstrap CI excludes 0 except oil_price (CI barely excludes 0, effect small)\"
}
```

### Ejemplo 2: Descubrimiento en Proceso de Fabricación

**Entrada del usuario:**
```bash
# Paso 1: Ejecutar análisis completo de correlación
correlation-weaver discover \
  --dataset manufacturing_sensors.csv \
  --method all \
  --bootstrap 5000 \
  --significance-level 0.001 \
  --output-dir manufacturing_analysis/

# Paso 2: Construir red de correlaciones fuertes
correlation-weaver network \
  --dataset manufacturing_sensors.csv \
  --min-correlation 0.6 \
  --layout kamada_kawai \
  --node-coloring community \
  --save manufacturing_analysis/network.html \
  --interactive

# Paso 3: Enfocarse en objetivo (product_defect_rate) con retardo temporal
correlation-weaver time-lag \
  --dataset manufacturing_sensors.csv \
  --target product_defect_rate \
  --max-lag 48 \
  --step 4 \
  --method ccf \
  --save manufacturing_analysis/defect_predictors.json
```

**Extracto de summary.txt de salida real:**
```
=== MANUFACTURING CORRELATION ANALYSIS ===
Dataset: 150 sensor readings × 45,678 production hours
Variables: 150 (temperature_1..48, pressure_1..32, vibration_1..24, speed_1..20, quality_metrics_26)

SIGNIFICANT CORRELATIONS (p < 0.001, |r| > 0.3):
- 1,247 pairwise correlations found
- Strongest positive: conveyor_belt_speed_7 ↔ vibration_motor_12 (r=0.84)
- Strongest negative: coolant_temperature_3 ↔ defect_rate (r=-0.56)

TIME-LAGGED PREDICTORS OF DEFECT_RATE (48-hour window):
Critical finding: vibration_sensor_18 shows peak correlation r=0.72 at lag +16 hours
Interpretation: High vibration on machine B 16 hours BEFORE inspection predicts defects
Lead time enables: 2 shifts of intervention before quality check

CLUSTER ANALYSIS:
5 distinct correlation communities detected:
1. Temperature cluster (23 sensors) - highly internally correlated (r > 0.7)
2. Pressure cluster (18 sensors)
3. Vibration cluster (31 sensors) - contains defect predictors
4. Speed cluster (15 sensors)
5. Quality cluster (12 metrics) - low correlation with others

RECOMMENDATION:
Monitor vibration_sensor_18 continuously. Alert threshold: correlation trend > 0.5
Trigger maintenance dispatch if 16-hour ahead prediction > 0.65
```

### Ejemplo 3: Descubrimiento de Patrones de Comportamiento del Cliente

**Entrada del usuario:**
```bash
# Preparar datos: eventos de usuario con timestamps, agregados por usuario-semana
# Asumir ya preprocesados con skill data-cleaner

correlation-weaver conditional \
  --dataset user_behavior_weekly.csv \
  --condition-on \"user_age_group,account_tenure_months\" \
  --target \"converted_to_premium\" \
  --method cond_mutual \
  --bootstrap 2000 \
  --output conditional_dependencies.json

correlation-weaver feature-select \
  --dataset user_behavior_weekly.csv \
  --target converted_to_premium \
  --method fwrapper \
  --n-features 20 \
  --metric mutual_info \
  --save selected_features.csv
```

**Extracto de significant_pairs.json de salida real:**
```json
{
  \"analysis\": \"Conditional mutual information controlling for user demographics\",
  \"target\": \"converted_to_premium\",
  \"conditional_on\": [\"user_age_group\", \"account_tenure_months\"],
  \"top_predictors\": [
    {
      \"feature\": \"tuesday_session_count\",
      \"conditional_mi\": 0.0423,
      \"unconditional_mi\": 0.0112,
      \"p_value\": 0.0008,
      \"interpretation\": \"Tuesday usage is 3.8x more predictive after controlling for age/tenure\"
    },
    {
      \"feature\": \"viewed_pricing_page_within_48h\",
      \"conditional_mi\": 0.0387,
      \"unconditional_mi\": 0.0341,
      \"p_value\": 0.052,
      \"interpretation\": \"Pricing page visits only mildly more predictive conditional; weak signal\"
    },
    {
      \"feature\": \"used_feature_X_three_times\",
      \"conditional_mi\": 0.0312,
      \"unconditional_mi\": 0.0089,
      \"p_value\": 0.0015,
      \"interpretation\": \"Feature X engagement is 3.5x more predictive conditional (non-linear pattern)\"
    }
  ]
}
```

### Ejemplo 4: Procesamiento Batch de Múltiples Conjuntos de Datos

**Entrada del usuario:**
```bash
correlation-weaver batch \
  --directory /data/sensor_readings/2025/03/ \
  --pattern \"sensor_*.parquet\" \
  --output-dir /reports/march_2025_correlations/ \
  --parallel true \
  --method spearman \
  --threshold 0.5 \
  --consolidate final_correlation_report.html
```

**Ejecución real del comando:**
```bash
# Ejecución paralela interna
xargs -P 8 -I {} correlation-weaver discover --dataset {} --method spearman --threshold 0.5 --output-dir /tmp/individual_reports/ ::: /data/sensor_readings/2025/03/sensor_*.parquet

# Consolidación: encuentra correlaciones consistentes en >70% de archivos
python consolidate_correlations.py \
  --input-dir /tmp/individual_reports/ \
  --consistency-threshold 0.7 \
  --output /reports/march_2025_correlations/final_report.html
```

**Reporte consolidado identifica:**
- Correlación entre coolant_temp_5 y vibration_11 aparece en 94% de archivos diarios (r medio=0.73)
- Misma correlación desaparece en días con ambient_temp > 35°C (20 archivos) → patrón condicional
- Recomendación: Instalar enfriador de refrigerante para estabilizar esta relación

## Comandos de Rollback

**1. Revertir a versión de análisis anterior:**
```bash
# Si se usa git para seguimiento de resultados
cd /ruta/a/analysis/results
git log --oneline correlations.csv
git checkout <commit_hash> correlations.csv network/

# Regenerar con parámetros diferentes
correlation-weaver discover \
  --dataset data.csv \
  --method spearman \
  --threshold 0.4 \
  --output-dir analysis_v2/
```

**2. Deshacer cambios de grafo de red (si se sobrescribió original):**
```bash
# Si se sobrescribieron nodes.csv/edges.csv
correlation-weaver export \
  --results previous_results.pickle \
  --format pickle \
  --include all

# Restaurar desde backup (auto-creado cada ejecución)
cp ~/.cache/corr-weaver/backup_2025-03-15_10-30-00/nodes.csv .
cp ~/.cache/corr-weaver/backup_2025-03-15_10-30-00/edges.csv .
```

**3. Deshabilitar una correlación descubierta de uso downstream:**
```bash
# Editar significant_pairs.json para remover falso positivo
jq 'del(.[] | select(.var1==\"sensor_A\" and .var2==\"sensor_B\"))' \
  significant_pairs.json > significant_pairs_filtered.json

# Re-construir red sin esa arista
correlation-weaver network \
  --use-significant-pairs significant_pairs_filtered.json \
  --min-correlation 0.6 \
  --save network_filtered.html
```

**4. Recuperarse de agotamiento de memoria:**
```bash
# Si el análisis falló con OOM, reintentar con Dask
export CORRELATION_WEAVER_USE_DASK=true
correlation-weaver discover \
  --dataset huge_dataset.csv \
  --chunksize 10000 \
  --output results/
```

**5. Recuperarse de cache corrupto:**
```bash
rm -rf ~/.cache/corr-weaver/
# Re-ejecutar análisis (será más lento primera vez)
correlation-weaver discover --dataset data.csv --method all

# O restaurar desde último backup exitoso si disponible
cp ~/.cache/corr-weaver/last_successful_backup/* ~/.cache/corr-weaver/
```

**6. Limpieza completa y reinicio:**
```bash
# Remover todas las salidas de correlation-weaver
find . -name \"*correlation*\" -type f -delete
find . -name \"*network*\" -type f -delete
rm -rf correlation_analysis/ results/

# Limpiar cache
rm -rf ~/.cache/corr-weaver/

# Re-ejecutar con parámetros frescos
correlation-weaver batch \
  --directory clean_data/ \
  --pattern \"*.csv\" \
  --output-dir fresh_analysis/
```

**7. Rollback de visualización específica a versión más simple:**
```bash
# Si grafo de red complejo está muy desordenado, simplificar
correlation-weaver network \
  --dataset data.csv \
  --min-correlation 0.8 \
  --max-nodes 50 \
  --layout circular \
  --save network_simplified.html

# En lugar de min-correlation 0.5 anterior con 150 nodos
```

## Solución de Problemas

**Problema:** \"MemoryError: Unable to allocate correlation matrix for 500 variables\"

**Solución:**
```bash
# Estimación: 500² × 8 bytes ≈ 2MB por matriz (float64) - esto debería funcionar
# Probablemente el problema es almacenar todos los resultados pairwise intermedios

# Solución 1: Usar menos computaciones
correlation-weaver discover \
  --dataset data.csv \
  --method pearson \
  --sample-variables 100 \   # Muestrear 100 variables aleatoriamente
  --threshold 0.7           # Solo calcular |r| > 0.7 (matriz dispersa)

# Solución 2: Usar Dask para out-of-core
export CORRELATION_WEAVER_USE_DASK=true
correlation-weaver discover --dataset huge.csv --chunksize 5000

# Solución 3: Aumentar swap (Linux)
sudo fallocate -l 16G /swapfile_extra
sudo chmod 600 /swapfile_extra
sudo mkswap /swapfile_extra
sudo swapon /swapfile_extra
```

**Problema:** \"All p-values are NaN after bootstrap\"

**Causa:** Demasiados pares faltantes causando insuficientes muestras bootstrap

**Solución:**
```bash
# Verificar patrón de missingness primero
correlation-weaver validate --dataset data.csv --missingness-test

# Usar eliminación pairwise explícitamente
correlation-weaver discover \
  --dataset data.csv \
  --method spearman \
  --pairwise-deletion \
  --bootstrap 1000 \
  --min-observations 30  # Solo calcular correlaciones con ≥30 pares

# O imputar primero (externo a este skill)
# Usar skill data-imputer luego reintentar
```

**Problema:** \"Granger causality test failed: series is non-stationary\"

**Solución:**
```bash
# Probar estacionariedad
correlation-weaver validate --dataset timeseries.csv --stationarity-test

# Si no estacionario, diferenciar datos primero (externo a este skill)
# O usar method=ccf que no requiere estacionariedad

# Forzando Granger con diferenciación (no recomendado):
correlation-weaver time-lag \
  --dataset timeseries.csv \
  --method granger \
  --differencing order1 \   # Diferenciar automáticamente una vez
  --test ngc               # Usar Granger no paramétrico
```

**Problema:** \"Network graph is unreadable (too many edges)\"

**Solución:**
```bash
# Aumentar umbral
correlation-weaver network \
  --dataset data.csv \
  --min-correlation 0.8 \    # Era 0.5
  --max-edges 200 \          # Solo mostrar top 200 aristas más fuertes
  --filter-by p_value \      # Solo incluir aristas con p < 0.001
  --layout fruchterman_reingold \  # Layout mejor para grafos dispersos
  --save network_simplified.html

# O clusterizar primero, mostrar centroides de cluster
correlation-weaver cluster \
  --dataset data.csv \
  --method hierarchical \
  --n-clusters 10

# Luego computar correlaciones promediadas por cluster para red
```

**Problema:** \"Conditional correlation gives different signs than unconditional\"

**Diagnóstico:**
Esto es paradoja de Simpson - esperado, no error.

**Verificación:**
```bash
# Comparar incondicional vs condicional
correlation-weaver discover --method pearson > unconditional.txt
correlation-weaver conditional --condition-on confounder1 > conditional.txt

# Examinar pares con reversal de signo
grep \"negative.*positive\" conditional.txt

# Aceptar como hallazgo válido: variable de confusión estaba distorsionando relación verdadera
```

**Problema:** \"Bootstrap confidence intervals extremely wide (CI includes 0 for almost all)\"

**Causas:** Tamaño efectivo de muestra pequeño debido a datos faltantes

**Soluciones:**
```bash
# Verificar N efectivo por correlación
correlation-weaver discover \
  --dataset data.csv \
  --report-observations-per-pair \
  --min-observations 100 \   # Filtrar pares con <100 obs
  --bootstrap 1000

# Aumentar iteraciones bootstrap para CI más estable (más lento)
correlation-weaver discover \
  --dataset data.csv \
  --bootstrap 10000 \    # Por defecto es 1000
  --ci 0.9              # Usar IC 90% en lugar de 95% (más estrecho)
```

**Problema:** \"correlation-weaver: command not found after installation\"

**Fix:**
```bash
# Verificar ubicación de instalación
which correlation-weaver || echo \"Not in PATH\"

# Añadir a PATH si instalado vía pip
export PATH=\"$HOME/.local/bin:$PATH\"   # Linux por defecto
# o
export PATH=\"/usr/local/bin:$PATH\"

# Para fix persistente, añadir a ~/.bashrc o ~/.zshrc
echo 'export PATH=\"$HOME/.local/bin:$PATH\"' >> ~/.bashrc
source ~/.bashrc

# Reinstalar con ruta explícita
pip install --user --upgrade correlation-weaver
```

**Problema:** \"ImportError: cannot import name 'dask' from 'sklearn'\"

**Causa:** Cambio de API de sklearn en versión 1.2+

**Solución:**
```bash
# Pinear versiones compatibles
pip install 'scikit-learn>=0.24.0,<1.2.0' 'dask[dataframe]>=2021.10.0'

# O actualizar correlation-weaver a versión usando nueva API
pip install --upgrade correlation-weaver

# Verificar versión
correlation-weaver --version
```

**Problema:** \"Network graph export fails: 'GraphML write failed for node attribute'\"

**Causa:** Atributos de nodo contienen tipos no serializables (ej. arrays, None)

**Solución:**
```bash
# Usar exportación más simple
correlation-weaver network \
  --dataset data.csv \
  --format csv \          # En lugar de graphml
  --node-attributes name,degree,community \
  --save network_nodes.csv network_edges.csv

# O limpiar atributos antes de exportar
python -c \"
import pandas as pd
nodes = pd.read_csv('nodes.csv')
# Convertir columnas dict a strings JSON
for col in nodes.select_dtypes(include=object):
    if nodes[col].apply(lambda x: isinstance(x, dict)).any():
        import json
        nodes[col] = nodes[col].apply(json.dumps)
nodes.to_csv('nodes_clean.csv', index=False)
\"
```

**Problema:** \"Time-lag analysis produces NaN at all lags\"

**Diagnosticar:**
```bash
# Verificar si series temporales están ordenadas correctamente
head -5 timeseries.csv | grep timestamp

# Verificar valores constantes
correlation-weaver validate --dataset timeseries.csv --constant-check

# Probable: variable objetivo no tiene varianza
# Solución: Eliminar columnas constantes del análisis
correlation-weaver discover --dataset timeseries.csv --drop-constant --threshold 0.001
```

**Problema:** \"Conditional mutual information computation extremely slow\"

**Optimización:**
```bash
# Reducir número de variables condicionantes (maldición de dimensionalidad)
correlation-weaver conditional \
  --dataset data.csv \
  --condition-on \"only_top_3_confounders\" \  # Seleccionar manualmente
  --method ksg \          # Kraskov-Stögbauer-Grassberger (más rápido)
  --n-neighbors 5 \       # Reducir de default 10
  --bins auto \           # Dejar que elija binning óptimo
  --subsample 10000       # Usar subset de datos para estimación MI
```

**Problema:** \"export command fails: 'Unknown format: xlsx'\"

**Fix:**
```bash
# Instalar dependencia opcional para exportación Excel
pip install openpyxl xlsxwriter

# Usar formato soportado
correlation-weaver export \
  --format csv \    # o json, pickle
  --include correlations,p_values

# Para Excel específicamente:
correlation-weaver export \
  --format excel \  # Requiere openpyxl
  --sheet-name Correlations
```

**Ajuste de Rendimiento para Datos Grandes (n > 1000):**

```bash
# 1. Muestrear variables primero para identificar más interesantes
correlation-weaver discover \
  --dataset huge.csv \
  --method pearson \
  --pre-variance-filter 0.01 \   # Remover varianza cercana a cero
  --pre-correlation-sample 200 \ # Muestrear 200 variables aleatorias para screening inicial
  --output initial_screen.json

# 2. De screening inicial, seleccionar top 50 variables
# (Paso manual: extraer nombres de variable de initial_screen.json)

# 3. Ejecutar análisis completo en subconjunto seleccionado
correlation-weaver discover \
  --dataset huge.csv \
  --variables-from initial_screen.json --top-n 50 \
  --method all \
  --bootstrap 2000 \
  --output final_analysis/
```

**Procesamiento Batch con Eficiencia de Memoria:**

```bash
# Usar Dask con particionado explícito
export DASK_SCHEDULER=threads
export CORRELATION_WEAVER_N_JOBS=4

correlation-weaver batch \
  --directory /big/data/ \
  --pattern \"*.parquet\" \
  --output-dir /reports/ \
  --parallel \
  --chunksize 50000 \    # Procesar 50k filas a la vez
  --method spearman \    # Spearman usa ranking (amigable con Dask)
  --persist-intermediate  # Cachear resultados intermedios a disco
```

## Pasos de Verificación

**Después de ejecutar correlation-weaver discover:**

1. Verificar código de salida: `echo $?` debería ser 0
2. Verificar que archivos de salida existen:
   ```bash
   ls -lah results/correlations.csv
   ls -lah results/summary.txt
   ```
3. Inspeccionar summary:
   ```bash
   grep \"significant\" results/summary.txt
   ```
4. Validar que valores de correlación están en [-1, 1]:
   ```bash
   awk -F, 'NR>1 && $3 !>= -1 && $3 <=1 {print \"INVALID:\", $1, $2, $3}' results/correlations.csv
   ```
5. Verificar que valores p son numéricos y en [0, 1]:
   ```bash
   awk -F, 'NR>1 && ($4 < 0 || $4 > 1) {print \"INVALID_P:\", $1, $2, $4}' results/correlations.csv
   ```

**Después de ejecutar análisis de red:**

1. Verificar que número de nodos coincide con variables únicas:
   ```bash
   wc -l results/network/nodes.csv  # Debería ser razonable (no 100k)
   ```
2. Verificar consistencia de aristas:
   ```bash
   awk -F, 'NR>1 && ($3 < -1 || $3 > 1) {print \"INVALID_EDGE:\", $1, $2, $3}' results/network/edges.csv
   ```
3. Para exportación GraphML, validar XML:
   ```bash
   xmllint --noout results/network/graph.graphml && echo \"Valid XML\"
   ```

**Después de análisis de retardo temporal:**

1. Verificar que valores de retardo dentro del rango solicitado:
   ```bash
   jq '.significant_lagged_correlations[].optimal_lag' results/lagged_correlations.json | sort -n
   ```
2. Verificar que no hay gaps en retardos (si `--step 1`):
   ```bash
   # Debería producir retardos enteros consecutivos
   ```

**Verificación de consistencia entre múltiples ejecuciones:**

```bash
# Ejecutar mismo análisis dos veces
correlation-weaver discover --dataset data.csv --method pearson --seed 42 > run1.txt
correlation-weaver discover --dataset data.csv --method pearson --seed 42 > run2.txt

# Comparar correlaciones top (deberían ser idénticas con misma semilla aleatoria)
diff <(grep \"correlation:\" run1.txt | head -20) <(grep \"correlation:\" run2.txt | head -20)
# Código de salida 0 = idénticas, 1 = diferencias (verificar si semillas bootstrap difieren)
```

**Validación cruzada con R:**
```bash
# Exportar datos, computar correlación en R para spot-check
csvcut -c var1,var2 data.csv | head -1000 > spot_check.csv
Rscript -e \"cor(t(read.csv('spot_check.csv')), use='pairwise.complete.obs')\" > r_cor.txt

# Comparar con resultado Python para var1-var2
grep \"var1,var2\" results/correlations.csv
```
```