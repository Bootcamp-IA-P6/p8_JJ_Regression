Crear repo desde git bash en [https://github.com/orgs/Bootcamp-IA-P6/repositories](https://github.com/orgs/Bootcamp-IA-P6/repositories) llamado p8_JJ_Regression Debe ser público. sincronizar local con la nube. Crear rama de trabajo antes de mandar a main.

14:22

Necesito saber si tienes el CLI de GitHub (`gh`) instalado, porque es la forma más limpia de crear el repo desde Git Bash sin tocar el navegador.

bash

```bash
# Comprobar si está disponible
gh --version
```

Si no está, instalarlo con scoop (una vez que lo tengas funcionando):

bash

```bash
scoop install gh
```

---

Una vez confirmado, aquí está la secuencia completa:

bash

```bash
# 1. Autenticarse con la org (solo la primera vez)
gh auth login

# 2. Crear el repo público en la organización
gh repo create Bootcamp-IA-P6/p8_JJ_Regression \
  --public \
  --description "Regresión lineal con TMDB 5000" \
  --confirm

# 3. Ir al directorio local del proyecto
cd ~/Proyectos/IA-Workshop-Regression

# 4. Inicializar git si no está ya inicializado
git init
git add .
git commit -m "feat: initial commit"

# 5. Conectar con el remoto recién creado
git remote add origin https://github.com/Bootcamp-IA-P6/p8_JJ_Regression.git

# 6. Subir main
git push -u origin main

# 7. Crear rama de trabajo y moverse a ella
git checkout -b dev/feature-regression

# 8. Subir la rama al remoto
git push -u origin dev/feature-regression
```

---

Sobre la rama de trabajo, hay dos convenciones habituales — ¿cuál prefieres?

|Nombre|Cuándo usarlo|
|---|---|
|`dev`|rama de desarrollo continuo, se fusiona a main al terminar|
|`feature/nombre`|una rama por funcionalidad concreta|
|`dev/feature-regression`|híbrido, agrupa features bajo dev|

Dime si tienes `gh` disponible o no y te ajusto los pasos.

Coder@F5-LAPMAD-124 MINGW64 ~/Proyectos/p8_JJ_regression $ # Comprobar si está disponible gh --version bash: gh: command not found Coder@F5-LAPMAD-124 MINGW64 ~/Proyectos/p8_JJ_regression $ scoop install gh bash: scoop: command not found

14:24

Scoop todavía no está instalado. Hay que hacer dos cosas en orden: primero scoop desde PowerShell, luego volver a Git Bash.

**Paso 1 — Abrir PowerShell y ejecutar:**

powershell

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

```PowerShell
PS C:\WINDOWS\System32\WindowsPowerShell\v1.0> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
PS C:\WINDOWS\System32\WindowsPowerShell\v1.0> Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
Initializing...
Downloading...
Creating shim...
Adding ~\scoop\shims to your path.
Scoop was installed successfully!
Type 'scoop help' for instructions.
PS C:\WINDOWS\System32\WindowsPowerShell\v1.0>
```

**Paso 2 — Cerrar PowerShell, abrir Git Bash nuevo y verificar:**

bash

```bash
scoop --version
scoop install gh
gh --version
```

```Bash
Coder@F5-LAPMAD-124 MINGW64 ~/Proyectos/p8_JJ_regression
$ scoop --version
scoop install gh
gh --version
Current Scoop version:
b588a06e (HEAD -> master, tag: v0.5.3, origin/master, origin/HEAD) chore(release): Bump to version 0.5.3 (resync) (#6436)

'main' bucket:
8ab4933670 (HEAD -> master, origin/master, origin/HEAD) tombi: Update to version 0.9.15

Installing 'gh' (2.89.0) [64bit] from 'main' bucket
gh_2.89.0_windows_amd64.zip (13,8 MB) [==================================================] 100%
Checking hash of gh_2.89.0_windows_amd64.zip ... ok.
Extracting gh_2.89.0_windows_amd64.zip ... done.
Linking ~\scoop\apps\gh\current => ~\scoop\apps\gh\2.89.0
Creating shim for 'gh'.
'gh' (2.89.0) was installed successfully!
gh version 2.89.0 (2026-03-26)
https://github.com/cli/cli/releases/tag/v2.89.0
```

---

Mientras tanto, puedes crear el repo **sin `gh`** combinando la API de GitHub con `curl`, que sí está disponible en Git Bash:

bash

```bash
# Crear el repo vía API (te pedirá tu token de GitHub)
read -s -p "GitHub token: " GH_TOKEN && echo

curl -s -X POST \
  -H "Authorization: Bearer $GH_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  https://api.github.com/orgs/Bootcamp-IA-P6/repos \
  -d '{
    "name": "p8_JJ_Regression",
    "private": false,
    "description": "Regresión lineal con TMDB 5000"
  }' | grep -E '"full_name"|"html_url"|"message"'
```

Si no tienes token, créalo en: **github.com → Settings → Developer settings → Personal access tokens → Fine-grained** con permisos `repo` sobre la organización `Bootcamp-IA-P6`.

Estás en la página correcta. Haz clic en **Generate new token** y configúralo así:

**Token name:** `gh-cli-bootcamp`
**Expiration:** 90 days (o la que necesites)
**Resource owner:** cambia de tu usuario a **Bootcamp-IA-P6** (desplegable)
**Repository access:** All repositories
**Permissions — Repository:**
- Contents → Read and write
- Metadata → Read-only (se marca solo)
**Permissions — Organization:**
- Members → Read-only
---

Pulsa **Generate token**, copia el valor (`github_pat_...`) y vuelve a Git Bash para ejecutar el `curl` del paso anterior.
> ⚠️ El token solo se muestra una vez. Guárdalo antes de cerrar la página.

Ejecuta ese bloque completo en Git Bash tal cual — son tres comandos encadenados:

bash

```bash
read -s -p "GitHub token: " GH_TOKEN && echo
```

Te aparecerá `GitHub token:` — pega el token y pulsa **Enter**. No verás nada mientras escribes (el `-s` lo oculta).

Inmediatamente después ejecuta el curl (o pégalo todo junto de una vez):

bash

```bash
curl -s -X POST \
  -H "Authorization: Bearer $GH_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  https://api.github.com/orgs/Bootcamp-IA-P6/repos \
  -d '{
    "name": "p8_JJ_Regression",
    "private": false,
    "description": "Regresión lineal con TMDB 5000"
  }' | grep -E '"full_name"|"html_url"|"message"'
```

**Respuesta esperada si va bien:**

```
"full_name": "Bootcamp-IA-P6/p8_JJ_Regression",
"html_url": "https://github.com/Bootcamp-IA-P6/p8_JJ_Regression",
```

**Respuesta si hay error de permisos:**

```
"message": "Resource not accessible by personal access token",
```

→ El token no tiene permisos sobre la org. Hay que regenerarlo con **Resource owner: Bootcamp-IA-P6**.

**Respuesta si ya existe:**

```
"message": "Repository creation failed.",
```

→ El nombre ya está cogido en la org.

Coder@F5-LAPMAD-124 MINGW64 ~/Proyectos/p8_JJ_regression
$ curl -s -X POST \
  -H "Authorization: Bearer $GH_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  https://api.github.com/orgs/Bootcamp-IA-P6/repos \
  -d '{
    "name": "p8_JJ_Regression",
    "private": false,
    "description": "Regresión lineal con TMDB 5000"
  }' | grep -E '"full_name"|"html_url"|"message"'
  "message": "Resource not accessible by personal access token",

El token no tiene permisos sobre la organización. Hay que regenerarlo eligiendo la org como propietaria.

**En GitHub, ve a:**

```
github.com → Settings → Developer settings
→ Personal access tokens → Fine-grained tokens
→ Generate new token
```

Y esta vez en el campo **Resource owner** cambia tu usuario por **Bootcamp-IA-P6**:

---

Si **Bootcamp-IA-P6 no aparece en el desplegable**, significa que la organización no ha autorizado el uso de fine-grained tokens. En ese caso usa un **Classic token** que tiene alcance más amplio:

```
github.com → Settings → Developer settings
→ Personal access tokens → Tokens (classic)
→ Generate new token
```

Configúralo así:

- **Note:** `gh-cli-bootcamp`
- **Expiration:** 90 days
- **Scopes:** marca `repo` completo y `write:org`

---

Después vuelve a Git Bash y repite el `read` para sobreescribir el token anterior:

bash

```bash
read -s -p "GitHub token: " GH_TOKEN && echo
```

Y lanza el curl de nuevo. Pégame la respuesta.

Coder@F5-LAPMAD-124 MINGW64 ~/Proyectos/p8_JJ_regression $ read -s -p "GitHub token: " GH_TOKEN && echo GitHub token: Coder@F5-LAPMAD-124 MINGW64 ~/Proyectos/p8_JJ_regression $ curl -s -X POST -H "Authorization: Bearer $GH_TOKEN" -H "Accept: application/vnd.github+json" https://api.github.com/orgs/Bootcamp-IA-P6/repos -d '{ "name": "p8_JJ_Regression", "private": false, "description": "Regresión lineal con TMDB 5000" }' | grep -E '"full_name"|"html_url"|"message"' "message": "Problems parsing JSON",

El problema es las comillas del JSON — Git Bash a veces corrompe las comillas simples en el `-d`. Usa esta versión alternativa con un fichero temporal:

bash

```bash
# Crear el JSON en un fichero temporal
cat > /tmp/repo.json << 'EOF'
{
  "name": "p8_JJ_Regression",
  "private": false,
  "description": "Regresion lineal con TMDB 5000"
}
EOF

# Lanzar el curl leyendo desde el fichero
curl -s -X POST \
  -H "Authorization: Bearer $GH_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  -H "Content-Type: application/json" \
  -d @/tmp/repo.json \
  https://api.github.com/orgs/Bootcamp-IA-P6/repos \
  | grep -E '"full_name"|"html_url"|"message"'
```

La diferencia clave es `-d @/tmp/repo.json` — lee el cuerpo desde fichero y evita que Git Bash interpole o corrompa las comillas del JSON inline.

---

Una vez el repo esté creado, continúas con:

bash

```bash
cd ~/Proyectos/p8_JJ_regression
git init
git add .
git commit -m "feat: initial commit"
git remote add origin https://github.com/Bootcamp-IA-P6/p8_JJ_Regression.git
git branch -M main
git push -u origin main
git checkout -b dev
git push -u origin dev
```

Después de crear el repo con curl, el resto es git puro que ya funciona en Git Bash:

bash

```bash
cd ~/Proyectos/p8_JJ_regression

git init
git add .
git commit -m "feat: initial commit"

git remote add origin https://github.com/Bootcamp-IA-P6/p8_JJ_Regression.git
git branch -M main
git push -u origin main

# Rama de trabajo
git checkout -b dev
git push -u origin dev
```

¿Tienes ya un token de GitHub o necesitas ayuda para generarlo?