# Regresión Lineal con TMDB 5000

Notebook para practicar regresión lineal usando datos reales de películas de The Movie Database.

**Archivos incluidos**

- `tmdb_ejercicio_regresion.ipynb` — notebook con celdas vacías para completar
- `tmdb_soluciones.ipynb` — notebook con todas las soluciones y mejoras

---

## Configuración del entorno con `uv`

> **¿Por qué `uv`?** Es un gestor de paquetes y entornos virtuales escrito en Rust, entre 10× y 100× más rápido que `pip`/`venv`. Reemplaza en un solo binario a `pip`, `pip-tools`, `virtualenv` y parcialmente a `pyenv`.

### 1. Instalar `uv`

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# Con pip (si ya tienes Python)
pip install uv
```

1. La descarga (`curl -LsSf ...`)

Usa la herramienta `curl` para obtener el script de instalación con estas opciones:

- **`-L` (Location):** Sigue redirecciones si la URL ha cambiado.
- **`-s` (Silent):** No muestra la barra de progreso ni mensajes de error comunes.
- **`-S` (Show error):** Si falla (y está en modo silencioso), muestra el motivo del error.
- **`-f` (Fail):** No entrega ninguna salida si el servidor devuelve un error (como un 404). 

2. La ejecución (`| sh`)

- **`|` (Pipe):** Toma la salida del primer comando (el contenido del script) y la pasa al segundo.
- **`sh`:** Ejecuta ese contenido directamente en el intérprete de comandos (Shell) de tu sistema.

---

🚀 ¿Qué es "uv"?

Es una alternativa moderna a herramientas tradicionales como `pip`, `pip-tools` y `poetry`. Sus principales ventajas son: 

- **Velocidad:** Es entre 10 y 100 veces más rápido que `pip`.
- **Todo en uno:** Gestiona versiones de Python, entornos virtuales (`venv`) y dependencias.
- **Sin dependencias:** No necesitas tener Python instalado previamente para instalar `uv`. 

⚠️ Nota de seguridad

Ejecutar scripts directamente desde una URL (`| sh`) es una práctica común en el desarrollo moderno, pero conlleva riesgos. Solo debes hacerlo si confías plenamente en la fuente (en este caso, **Astral** es una empresa reconocida en el ecosistema Python).

---

### 2. Crear el entorno virtual e instalar dependencias

```bash
# Crear entorno con Python 3.11 (recomendado)
uv venv .venv --python 3.11

# Activar el entorno
source .venv/bin/activate        # macOS / Linux
.venv\Scripts\activate           # Windows

# Instalar todas las dependencias de golpe
uv pip install kagglehub scikit-learn pandas numpy matplotlib seaborn jupyter ipykernel
```

> `uv pip install` acepta exactamente la misma sintaxis que `pip install`.  
> La primera vez resuelve y descarga en segundos; las siguientes es casi instantáneo gracias a su caché global.

### 3. (Opcional) Fijar versiones con `requirements.txt`

Para garantizar reproducibilidad entre máquinas, genera un lockfile:

```bash
# Exportar versiones exactas instaladas
uv pip freeze > requirements.txt

# Reproducir el entorno en otra máquina
uv pip install -r requirements.txt
```

Contenido típico del `requirements.txt` resultante:

```
kagglehub==0.3.4
scikit-learn==1.5.2
pandas==2.2.3
numpy==1.26.4
matplotlib==3.9.2
seaborn==0.13.2
jupyter==1.1.1
ipykernel==6.29.5
```

### 4. Registrar el kernel en Jupyter

```bash
python -m ipykernel install --user --name tmdb-env --display-name "Python (tmdb-env)"
```

Este comando sirve para que un **entorno virtual** específico (en este caso uno llamado `tmdb-env`) aparezca como una opción seleccionable dentro de **Jupyter Notebook** o **JupyterLab**.

En términos simples: le dice a Jupyter "oye, este entorno existe y quiero usar sus librerías para ejecutar mi código".

---

🔍 Desglose del comando

1. `python -m ipykernel install`

- **`python -m ipykernel`**: Ejecuta el módulo `ipykernel`. Este es el "puente" que permite que Jupyter se comunique con Python.
- **`install`**: Indica que quieres registrar un nuevo "kernel" (el motor que corre el código) en Jupyter.

2. `--user`

- Instala el kernel solo para tu **usuario actual**, no para todo el sistema.
- **Ventaja:** No necesitas permisos de administrador (sudo/root) y no afectas a otros usuarios de la computadora.

3. `--name tmdb-env`

- Es el **nombre interno** que usará Jupyter para identificar el kernel en sus archivos de configuración.
- Debe ser simple, sin espacios ni caracteres especiales.

4. `--display-name "Python (tmdb-env)"`

- Es el **nombre visible** que verás en la interfaz de Jupyter (en el menú desplegable arriba a la derecha).
- Puede tener espacios, mayúsculas y ser tan descriptivo como quieras.

---

💡 ¿Por qué es necesario este comando?

Cuando creas un entorno virtual (con `venv`, `conda` o `uv`), Jupyter no lo detecta automáticamente. Si intentas importar una librería que instalaste en ese entorno (como `pandas` o `sklearn`), Jupyter te dará un error de `ModuleNotFoundError` porque está usando el Python global del sistema.

**Al ejecutar este comando:**

1. Creas un archivo de configuración (`kernel.json`).
2. Jupyter "ve" el nuevo kernel en su lista.
3. Puedes seleccionar **"Python (tmdb-env)"** desde el menú **Kernel -> Change Kernel** dentro de tu cuaderno.

---

🛠️ Pasos previos importantes

Para que este comando funcione, **primero** debes tener instalado el paquete `ipykernel` dentro de tu entorno activo:

bash

```
# Si usas uv
uv pip install ipykernel

# O con pip tradicional
pip install ipykernel
```

Usa el código con precaución.

Después abre el notebook y selecciona el kernel `Python (tmdb-env)` en el menú superior derecho.

### 5. Lanzar Jupyter

```bash
jupyter notebook tmdb_ejercicio_regresion.ipynb
# o con JupyterLab
jupyter lab
```

1>_	Si se escribe jupyter y se presiona Tab aparece:

```bash
Coder@F5-LAPMAD-124 MINGW64 ~/Proyectos/p8_JJ_regression
$ jupyter
jupyter-console.exe       jupyter-lab.exe           jupyter-run.exe
jupyter-dejavu.exe        jupyter-labextension.exe  jupyter-server.exe
jupyter-events.exe        jupyter-labhub.exe        jupyter-troubleshoot.exe
jupyter-execute.exe       jupyter-migrate.exe       jupyter-trust.exe
jupyter-kernel.exe        jupyter-nbconvert.exe     jupyter.exe
jupyter-kernelspec.exe    jupyter-notebook.exe
```

2>_	Por el contrario, si se escribe jupyter y se le da a Entrar aparece:

```bash
Coder@F5-LAPMAD-124 MINGW64 ~/Proyectos/p8_JJ_regression
$ jupyter
usage: jupyter [-h] [--version] [--config-dir] [--data-dir] [--runtime-dir] [--paths] [--json]
               [--debug]
               [subcommand]

Jupyter: Interactive Computing

positional arguments:
  subcommand     the subcommand to launch

options:
  -h, --help     show this help message and exit
  --version      show the versions of core jupyter packages and exit
  --config-dir   show Jupyter config dir
  --data-dir     show Jupyter data dir
  --runtime-dir  show Jupyter runtime dir
  --paths        show all Jupyter paths. Add --json for machine-readable format.
  --json         output paths as machine-readable json
  --debug        output debug information about paths

Available subcommands: console dejavu events execute kernel kernelspec lab labextension labhub
migrate nbconvert notebook run server troubleshoot trust

Please specify a subcommand or one of the optional arguments.
```

La diferencia es simplemente el comportamiento del shell en cada caso:

**`jupyter` + Tab** — el shell busca todos los ejecutables en el PATH que empiezan por `jupyter` y los lista. No ejecuta nada, es autocompletado del sistema de ficheros. Cada entrada es un `.exe` independiente instalado en tu entorno.

**`jupyter` + Enter** — lanza `jupyter.exe` sin argumentos, que es el ejecutable principal/dispatcher. Al no recibir ningún subcomando, imprime la ayuda y sale.

```txt
Qué es el dispatcher.
¿por qué los exe funcionan si se omiten las extensiones, es decir:
Si escribo jupyter y le doy a enter ejecuta jupyter.exe
Si escribo jupyter-lab y le doy a enter ejecuta jupyter-lab.exe
etc.
?.
```

**El dispatcher**

`jupyter.exe` es un programa central que recibe el primer argumento como nombre de subcomando y lo traduce a una llamada al ejecutable correspondiente. Cuando escribes `jupyter lab`, internamente hace:

```
jupyter.exe "lab" → busca jupyter-lab.exe → lo ejecuta
```

Es el mismo patrón que usan `git`, `docker` o `uv` — un único punto de entrada que delega en subcomandos. La ventaja es que no necesitas recordar los nombres exactos de los ejecutables individuales.

---

**Por qué funcionan sin extensión `.exe`**

Es una característica de Windows heredada del intérprete de comandos. Windows tiene una variable de entorno llamada `PATHEXT` que define qué extensiones se consideran ejecutables y en qué orden probarlas:

bash

```bash
# Ver su contenido desde Git Bash
echo "$PATHEXT"
# .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC
```

Cuando escribes `jupyter` sin extensión, el shell recorre el PATH buscando en cada carpeta un fichero que se llame `jupyter` con alguna de esas extensiones, en ese orden. Encuentra `jupyter.exe` y lo ejecuta.

Es exactamente el mismo mecanismo por el que puedes escribir `python` en lugar de `python.exe`, o `git` en lugar de `git.exe`.

---

En resumen, ambas cosas son conveniencias del sistema operativo: `PATHEXT` elimina la necesidad de escribir `.exe`, y el dispatcher elimina la necesidad de recordar el nombre completo de cada subcomando.

---

## Credenciales de Kaggle

El dataset se descarga automáticamente con `kagglehub`, pero necesitas una cuenta de Kaggle.

1. Ve a [kaggle.com/settings](https://www.kaggle.com/settings) → *API* → **Create New Token**
2. Descarga el archivo `kaggle.json`
3. Colócalo en la ruta correcta:

```bash
# macOS / Linux
mkdir -p ~/.kaggle
mv ~/Downloads/kaggle.json ~/.kaggle/
chmod 600 ~/.kaggle/kaggle.json   # ← importante: permisos restrictivos

# Windows
mkdir %USERPROFILE%\.kaggle
move %USERPROFILE%\Downloads\kaggle.json %USERPROFILE%\.kaggle\
```

4. Verifica que funciona:

```bash
python -c "import kagglehub; print(kagglehub.__version__)"
```

---

## Guía por secciones

---

### Sección 0 — Instalación y librerías

Importa todas las librerías necesarias. Si alguna falta, instálala con `uv pip install <nombre>` en la terminal (con el entorno activado), no dentro del notebook.

> ⚠️ **Depuración frecuente:** `ModuleNotFoundError` suele significar que Jupyter está usando un kernel diferente al entorno virtual. Verifica con:
> ```python
> import sys; print(sys.executable)
> ```
> Debe apuntar a `.venv/bin/python`.

---

### Sección 1 — Descarga y carga de datos

Descarga el dataset con `kagglehub.dataset_download()` usando el identificador `'tmdb/tmdb-movie-metadata'`. Devuelve una ruta local. Desde esa ruta carga los dos CSVs con `pd.read_csv()`.

**Pista:** usa `os.path.join(path, 'nombre_archivo.csv')` para construir la ruta completa.

Resultado esperado:
```
movies shape:  (4803, 20)
credits shape: (4803, 4)
```

<details>
<summary>Ver respuesta</summary>

```python
import kagglehub

path = kagglehub.dataset_download('tmdb/tmdb-movie-metadata')

df_movies  = pd.read_csv(os.path.join(path, 'tmdb_5000_movies.csv'))
df_credits = pd.read_csv(os.path.join(path, 'tmdb_5000_credits.csv'))

print(f"movies shape:  {df_movies.shape}")
print(f"credits shape: {df_credits.shape}")
```

</details>

> ⚠️ **Depuración frecuente:** si `kagglehub` lanza `OSError: kaggle.json not found`, revisa los permisos del archivo (`chmod 600`) y que la ruta sea exactamente `~/.kaggle/kaggle.json`.

---

### Sección 2 — Merge de los dos datasets

`df_movies` tiene columna `id` y `df_credits` tiene columna `movie_id`. Son la misma clave. Después del merge aparecen columnas duplicadas (`title_x`, `title_y`, `movie_id`) que hay que limpiar.

**Pista:** usa `pd.merge(..., left_on='id', right_on='movie_id')` y luego `drop()` y `rename()`.

Resultado esperado:
```
merged shape: (4803, 23)
```

<details>
<summary>Ver respuesta</summary>

```python
df = df_movies.merge(df_credits, left_on='id', right_on='movie_id', how='inner')

df = df.drop(columns=['movie_id', 'title_y'], errors='ignore')
df = df.rename(columns={'title_x': 'title'})

print(f"merged shape: {df.shape}")
```

</details>

---

### Sección 3 — Parseo de columnas JSON

Las columnas `genres`, `cast` y `crew` no son listas Python reales, son strings con formato JSON. Antes de usarlas hay que parsearlas con `json.loads()`.

Implementa tres funciones:

- `parse_json_col(val)` — convierte el string a lista de dicts. Usa `try/except` para manejar valores nulos.
- `extract_names(val, key, limit)` — llama a la anterior y extrae el valor de `key` de cada dict.
- `get_director(crew_str)` — recorre el crew buscando el dict donde `job == 'Director'` y devuelve su `name`.

Luego crea las columnas: `genres_list`, `main_genre` (primer elemento), `director`, y `release_year` (desde `release_date` con `.dt.year`).

**Pista:** usa `.apply()` para aplicar una función a toda una columna. Para el año convierte primero con `pd.to_datetime(..., errors='coerce')`.

<details>
<summary>Ver respuesta</summary>

```python
def parse_json_col(val):
    """Convierte un string JSON a lista de dicts. Retorna [] ante errores."""
    try:
        return json.loads(val)
    except (ValueError, TypeError):
        return []

def extract_names(val, key='name', limit=None):
    """Extrae el campo `key` de cada elemento de la lista JSON."""
    items = parse_json_col(val)
    names = [d[key] for d in items if key in d]
    return names[:limit] if limit else names

def get_director(crew_str):
    """Retorna el nombre del primer Director encontrado en el crew, o NaN."""
    for member in parse_json_col(crew_str):
        if member.get('job') == 'Director':
            return member.get('name', np.nan)
    return np.nan

df['genres_list']  = df['genres'].apply(extract_names)
df['main_genre']   = df['genres_list'].apply(lambda x: x[0] if x else 'Unknown')
df['director']     = df['crew'].apply(get_director)
df['release_date'] = pd.to_datetime(df['release_date'], errors='coerce')
df['release_year'] = df['release_date'].dt.year
```

</details>

> ⚠️ **Depuración frecuente:** si `json.loads` lanza `JSONDecodeError` de forma masiva, el CSV usa comillas simples en lugar de dobles. Añade un paso de limpieza: `val.replace("'", '"')` antes de parsear (con cuidado, solo si el formato lo permite).

---

### Sección 4 — Limpieza de datos

`budget == 0` y `revenue == 0` no son ceros reales, son valores faltantes que el dataset codifica así. Fíltralos junto con películas no estrenadas o sin director conocido.

**Pista:** encadena las condiciones con `&` dentro de un solo filtro booleano. Recuerda añadir `.copy()` al final para evitar el `SettingWithCopyWarning`.

Resultado esperado:
```
budget == 0:  1037
revenue == 0: 1427

clean shape: (3154, 26)
```

<details>
<summary>Ver respuesta</summary>

```python
print(f"budget == 0:  {(df['budget'] == 0).sum()}")
print(f"revenue == 0: {(df['revenue'] == 0).sum()}")

df = df[
    (df['budget']  > 100_000) &
    (df['revenue'] > 100_000) &
    (df['status']  == 'Released') &
    (df['runtime'] > 0) &
    (df['director'].notna())
].copy()

print(f"\nclean shape: {df.shape}")
```

</details>

> 💡 **Mejora:** en lugar del umbral fijo `100_000`, podrías usar el percentil 5 del dataset (`df['budget'].quantile(0.05)`) para que el filtro se adapte automáticamente a otros datasets.

---

### Sección 5 — Análisis exploratorio

El objetivo es entender la forma de las distribuciones antes de modelar. Genera tres gráficos: histogramas de `revenue` y `budget`, revenue mediano por género (top 10), y scatter de `budget` vs `revenue`.

**Pista:** para el gráfico de géneros usa `.groupby('main_genre')['revenue'].median().sort_values()` y `.plot(kind='barh')`.

<details>
<summary>Ver respuesta</summary>

```python
# Histogramas
fig, axes = plt.subplots(1, 2, figsize=(12, 4))
axes[0].hist(df['revenue'], bins=50, color='steelblue', edgecolor='white')
axes[0].set_title('Revenue')
axes[1].hist(df['budget'],  bins=50, color='coral', edgecolor='white')
axes[1].set_title('Budget')
plt.tight_layout()
plt.show()

# Revenue mediano por género
(df.groupby('main_genre')['revenue']
   .median()
   .sort_values(ascending=True)
   .tail(10)
   .plot(kind='barh', figsize=(8, 4), color='steelblue'))
plt.xlabel('Revenue mediano ($)')
plt.title('Top 10 géneros por revenue mediano')
plt.tight_layout()
plt.show()

# Scatter
plt.figure(figsize=(7, 5))
plt.scatter(df['budget'], df['revenue'], alpha=0.3, s=10)
plt.xlabel('Budget ($)')
plt.ylabel('Revenue ($)')
plt.tight_layout()
plt.show()
```

</details>

---

### Sección 6 — Transformación logarítmica

`revenue`, `budget` y `popularity` tienen distribuciones con cola muy larga. La transformación `log` comprime esa escala y hace la relación más lineal, que es exactamente lo que asume la regresión lineal.

Usa `np.log1p()` en lugar de `np.log()` porque es seguro cuando hay valores en cero.

Para la matriz de correlación usa `sns.heatmap()` con `annot=True` sobre el resultado de `.corr()`.

**Pista:** en una relación log-log, el coeficiente de regresión se interpreta como elasticidad: *"por cada 1% de aumento en X, Y cambia un β%"*.

Resultado esperado:
```
log_revenue      1.0000
log_budget       0.6411
log_popularity   0.6725
log_vote_count   0.7196
vote_average     0.1299
runtime          0.1873
```

<details>
<summary>Ver respuesta</summary>

```python
df['log_revenue']    = np.log1p(df['revenue'])
df['log_budget']     = np.log1p(df['budget'])
df['log_popularity'] = np.log1p(df['popularity'])
df['log_vote_count'] = np.log1p(df['vote_count'])

vars_corr = ['log_revenue', 'log_budget', 'log_popularity',
             'log_vote_count', 'vote_average', 'runtime']

mask = np.triu(np.ones(len(vars_corr), dtype=bool))
plt.figure(figsize=(7, 5))
sns.heatmap(df[vars_corr].corr(), mask=mask, annot=True,
            fmt='.2f', cmap='coolwarm', center=0)
plt.tight_layout()
plt.show()

print("\nCorrelación con log_revenue:")
print(df[vars_corr].corr()['log_revenue'].sort_values(ascending=False))
```

</details>

---

### Sección 7 — Regresión lineal simple

El modelo más básico: una sola variable de entrada. Sirve como punto de referencia (*baseline*) para comparar con los modelos más complejos.

Flujo estándar: **split → fit → predict → evaluar con R² y RMSE**.

**Pista:** `r2_score(y_test, y_pred)` y `np.sqrt(mean_squared_error(y_test, y_pred))`. Para la línea de regresión en el scatter, genera puntos con `np.linspace` y aplica manualmente `intercept_ + coef_[0] * x`.

Resultado esperado:
```
X_train: (2523, 1)   X_test: (631, 1)

beta0: 4.0181
beta1: 0.8062
R²:    0.4062
RMSE:  1.3041
```

<details>
<summary>Ver respuesta</summary>

```python
X = df[['log_budget']]
y = df['log_revenue']

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
print(f"X_train: {X_train.shape}   X_test: {X_test.shape}")

modelo = LinearRegression().fit(X_train, y_train)
y_pred = modelo.predict(X_test)

r2_simple   = r2_score(y_test, y_pred)
rmse_simple = np.sqrt(mean_squared_error(y_test, y_pred))

print(f"\nbeta0: {modelo.intercept_:.4f}")
print(f"beta1: {modelo.coef_[0]:.4f}")
print(f"R²:    {r2_simple:.4f}")
print(f"RMSE:  {rmse_simple:.4f}")

# Scatter con línea de regresión
x_line = np.linspace(df['log_budget'].min(), df['log_budget'].max(), 200)
plt.figure(figsize=(7, 5))
plt.scatter(df['log_budget'], df['log_revenue'], alpha=0.3, s=10, label='Datos')
plt.plot(x_line, modelo.intercept_ + modelo.coef_[0] * x_line,
         color='red', lw=2, label='Regresión')
plt.xlabel('log(Budget)')
plt.ylabel('log(Revenue)')
plt.legend()
plt.tight_layout()
plt.show()
```

</details>

---

### Sección 8 — Diagnóstico de residuos

R² alto no garantiza que el modelo sea correcto. Revisa los residuos (`y_real - y_predicho`) para detectar problemas:

- Si el gráfico *residuos vs predichos* muestra un patrón (embudo, curva), hay algún supuesto violado.
- El histograma de residuos debería ser aproximadamente normal y centrado en 0.
- En *real vs predicho*, los puntos deben seguir la diagonal.

**Pista:** los residuos se calculan simplemente como `y_test - y_pred`.

<details>
<summary>Ver respuesta</summary>

```python
residuos = y_test - y_pred

fig, axes = plt.subplots(1, 3, figsize=(15, 4))

axes[0].scatter(y_pred, residuos, alpha=0.4, s=10)
axes[0].axhline(0, color='red', ls='--')
axes[0].set_title('Residuos vs Predichos')
axes[0].set_xlabel('Predichos')
axes[0].set_ylabel('Residuos')

axes[1].hist(residuos, bins=40)
axes[1].axvline(0, color='red', ls='--')
axes[1].set_title(f'Distribución de residuos  (μ={residuos.mean():.3f})')

mn = min(y_test.min(), y_pred.min())
mx = max(y_test.max(), y_pred.max())
axes[2].scatter(y_test, y_pred, alpha=0.4, s=10)
axes[2].plot([mn, mx], [mn, mx], color='red', ls='--')
axes[2].set_title('Real vs Predicho')
axes[2].set_xlabel('Real')
axes[2].set_ylabel('Predicho')

plt.tight_layout()
plt.show()

print(f"Media residuos: {residuos.mean():.4f} | Std: {residuos.std():.4f}")
```

</details>

> 💡 **Mejora:** si el histograma tiene colas pesadas, prueba a identificar outliers extremos con `residuos[residuos.abs() > 3 * residuos.std()]` y analiza qué películas son.

---

### Sección 9 — Regresión múltiple

Se añaden más variables predictoras. Al tener features en escalas muy distintas (`log_budget` ≈ 18, `runtime` ≈ 100), estandarizar con `StandardScaler` permite comparar los coeficientes directamente: el más grande en valor absoluto es el que más influye.

**Pista:** ajusta el scaler solo con train (`fit_transform`) y aplica la transformación a test (`transform`). Nunca al revés o habrá *data leakage*.

Resultado esperado:
```
R² simple:   0.4062    RMSE simple:   1.3041
R² múltiple: 0.6236    RMSE múltiple: 1.0382

Coeficientes estandarizados:
  log_budget:      0.6097
  log_popularity:  0.0016
  log_vote_count:  0.9002
  vote_average:   -0.0365
  runtime:         0.0378
```

<details>
<summary>Ver respuesta</summary>

```python
feature_cols = ['log_budget', 'log_popularity', 'log_vote_count', 'vote_average', 'runtime']

X_m = df[feature_cols].fillna(df[feature_cols].median())
X_tr, X_te, y_tr, y_te = train_test_split(X_m, y, test_size=0.2, random_state=42)

scaler   = StandardScaler()
modelo_m = LinearRegression().fit(scaler.fit_transform(X_tr), y_tr)
y_pred_m = modelo_m.predict(scaler.transform(X_te))

r2_multi   = r2_score(y_te, y_pred_m)
rmse_multi = np.sqrt(mean_squared_error(y_te, y_pred_m))

print(f"R²:   {r2_multi:.4f}")
print(f"RMSE: {rmse_multi:.4f}")

# Coeficientes
coefs = pd.Series(modelo_m.coef_, index=feature_cols).sort_values()
coefs.plot(kind='barh',
           color=['salmon' if c < 0 else 'steelblue' for c in coefs],
           figsize=(7, 4))
plt.axvline(0, color='black', lw=0.8)
plt.title('Coeficientes estandarizados — Modelo Múltiple')
plt.tight_layout()
plt.show()
```

</details>

---

### Sección 10 — Ingeniería de características

Se aprovecha la información ya extraída del JSON para crear dos nuevas features:

- **Dummies de género:** convierte `main_genre` en columnas binarias (0/1), una por género. Permite al modelo aprender que cada género tiene distinto impacto en revenue.
- **`director_avg_revenue`:** el promedio histórico de `log_revenue` de cada director en el dataset. Es un proxy de su "poder de taquilla".

Al final compara los tres modelos en una tabla con R², RMSE y número de features.

**Pista:** para los dummies usa `value_counts().head(8).index` para obtener los géneros más frecuentes. Para el promedio del director usa `groupby().transform('mean')` (más robusto que `groupby().mean()` + `map()` porque preserva el índice).

> ⚠️ **Depuración frecuente:** si `director_avg_revenue` tiene muchos NaN después del `map()`, es porque algunos nombres de director tienen espacios extra o codificación distinta. `transform('mean')` evita este problema por completo.

<details>
<summary>Ver respuesta</summary>

```python
# Dummies de los 8 géneros más frecuentes
top_genres = df['main_genre'].value_counts().head(8).index.tolist()
for g in top_genres:
    df[f'genre_{g}'] = (df['main_genre'] == g).astype(int)

# MEJORA: transform en lugar de map para evitar pérdida de índice
df['director_avg_revenue'] = df.groupby('director')['log_revenue'].transform('mean')

genre_cols      = [f'genre_{g}' for g in top_genres]
feature_cols_v2 = feature_cols + genre_cols + ['director_avg_revenue']

print(f"Top 8 géneros: {top_genres}")
print(f"Total features: {len(feature_cols_v2)}")

X_v2 = df[feature_cols_v2].fillna(df[feature_cols_v2].median())
y_v2 = df['log_revenue']
X_tr2, X_te2, y_tr2, y_te2 = train_test_split(X_v2, y_v2, test_size=0.2, random_state=42)

sc2 = StandardScaler()
modelo_v2 = LinearRegression().fit(sc2.fit_transform(X_tr2), y_tr2)
y_pred_v2 = modelo_v2.predict(sc2.transform(X_te2))

r2_enrich   = r2_score(y_te2, y_pred_v2)
rmse_enrich = np.sqrt(mean_squared_error(y_te2, y_pred_v2))

# Tabla comparativa
print(pd.DataFrame({
    'Modelo':   ['Simple', 'Múltiple', 'Enriquecido'],
    'R²':       [r2_simple, r2_multi, r2_enrich],
    'RMSE':     [rmse_simple, rmse_multi, rmse_enrich],
    'Features': [1, len(feature_cols), len(feature_cols_v2)]
}).to_string(index=False))
```

</details>

> 💡 **Mejora:** añade validación cruzada (5-fold) para estimar la varianza real del R² y detectar sobreajuste:
> ```python
> from sklearn.model_selection import cross_val_score
> cv = cross_val_score(LinearRegression(),
>                      sc2.fit_transform(X_v2.fillna(X_v2.median())),
>                      y_v2, cv=5, scoring='r2')
> print(f"CV R² 5-fold: {cv.mean():.4f} ± {cv.std():.4f}")
> ```

---

### Sección 11 — Reflexión final

No hay código. Responde en la celda Markdown del notebook.

Preguntas guía:

1. ¿Qué variable numérica tiene mayor correlación con `log_revenue` y por qué tiene sentido?
2. ¿Cuánto mejoró el R² del modelo simple al enriquecido?
3. ¿Qué género tiene el coeficiente positivo más alto? ¿Coincide con lo que esperarías?

---

## Conceptos clave

| Concepto | Dónde se usa |
|---|---|
| `uv venv` + `uv pip install` | Gestión de entorno reproducible y rápida |
| `json.loads` + `.apply` | Parseo de columnas JSON embebidas |
| `np.log1p` | Transformación de variables con cola larga |
| `train_test_split` | Separación de datos para evaluar sin sesgo |
| `StandardScaler` | Estandarización para comparar coeficientes |
| Residuos | Diagnóstico de supuestos del modelo |
| Dummies categóricas | Incluir variables de texto en regresión |
| `groupby + transform` | Features basadas en estadísticas por grupo, sin data leakage |
| `cross_val_score` | Estimación robusta del rendimiento real |

---

## Solución de problemas comunes

| Error | Causa probable | Solución |
|---|---|---|
| `ModuleNotFoundError` | Jupyter usa un kernel diferente al venv | `python -m ipykernel install --user --name tmdb-env` |
| `OSError: kaggle.json not found` | Archivo en ruta incorrecta o permisos | `chmod 600 ~/.kaggle/kaggle.json` |
| `SettingWithCopyWarning` | Filtrado sin `.copy()` | Añadir `.copy()` al final del filtro |
| `JSONDecodeError` en parseo | CSV con comillas simples | Pre-procesar con `.replace("'", '"')` |
| Muchos NaN en `director_avg_revenue` | Usar `map()` en vez de `transform()` | Reemplazar por `groupby().transform('mean')` |
