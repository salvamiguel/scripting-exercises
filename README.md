# Ejercicios de Scripting en Bash

Repositorio de ejercicios para aprender scripting en Bash, desde lo basico hasta automatizacion DevOps y testing con BATS.

## Estructura del repositorio

```
scripts/
  saludo.sh           # Ejercicio 1.1
  clasificar.sh       # Ejercicio 1.2
  calculadora.sh      # Ejercicio 1.3
  ppt.sh              # Reto 1.4
  genpass.sh          # Reto 1.5
  top_ips.sh          # Ejercicio 2.1
  monitor_disco.sh    # Ejercicio 2.2
  backup.sh           # Ejercicio 2.3
  csv_report.sh       # Reto 2.4
  healthcheck.sh      # Reto 2.5
  tf-wrapper.sh       # Ejercicio 3.1
  tf-wrapper.bats     # Ejercicio 3.2
```

Todos los ficheros ya existen vacios. Tu trabajo es implementarlos.

---

## Como empezar

### 1. Haz fork del repositorio

Desde GitHub, haz clic en **Fork** para crear una copia en tu cuenta.

### 2. Clona tu fork

```bash
git clone https://github.com/<tu-usuario>/scripting-exercises.git
cd scripting-exercises
```

### 3. Crea una rama de trabajo

**Importante:** No trabajes directamente en `main`. Crea una rama para tus ejercicios:

```bash
git checkout -b soluciones
```

### 4. Implementa los ejercicios

Edita los ficheros en `scripts/` con tu solucion. Cada script debe:

- Empezar con el shebang `#!/usr/bin/env bash`
- Tener permisos de ejecucion (`chmod +x scripts/tu_script.sh`)
- Manejar errores con mensajes descriptivos y codigos de salida apropiados

### 5. Haz commit y push

```bash
git add scripts/
git commit -m "feat: implementar ejercicios lab 1"
git push origin soluciones
```

### 6. Crea un Pull Request para evaluar

Cuando quieras evaluar tus ejercicios:

1. Ve a tu fork en GitHub
2. Haz clic en **"Compare & pull request"**
3. Asegurate de que la base es `main` de **tu propio fork** (no el repositorio original)
4. Crea el PR

La creacion del PR disparara automaticamente el **workflow de evaluacion**. Puedes ver los resultados en la pestana **Actions** de tu repositorio o en los checks del PR.

> **Tip:** Puedes hacer push de mas commits a la misma rama y el workflow se volvera a ejecutar automaticamente con cada push.

---

## Evaluacion automatica

Los ejercicios se evaluan con tests [BATS](https://github.com/bats-core/bats-core) (Bash Automated Testing System) que se ejecutan en GitHub Actions.

- Cada ejercicio tiene su suite de tests propia
- Los tests validan argumentos, salida esperada, codigos de retorno y casos limite
- No puedes modificar los tests: estan en un repositorio privado del profesor

### Interpretar los resultados

En la pestana **Actions** o en los checks del PR, veras un job por cada grupo de ejercicios:

| Job | Ejercicios | Tipo |
|-----|-----------|------|
| `grade-lab1` | saludo, clasificar, calculadora | Obligatorios |
| `grade-lab1-retos` | ppt, genpass | Opcionales |
| `grade-lab2` | top_ips, monitor_disco, backup | Obligatorios |
| `grade-lab2-retos` | csv_report, healthcheck | Opcionales |
| `grade-lab3` | tf-wrapper.sh, tf-wrapper.bats | Obligatorios |

Si un test falla, el log mostrara exactamente que se esperaba y que recibio tu script.

---

## Ejercicios

### Lab 1 - Bash basico

#### 1.1 `saludo.sh` - Saludo por edad

Recibe un nombre y una edad como argumentos. Muestra un saludo con un mensaje segun el rango de edad.

```bash
./scripts/saludo.sh "Ana" 25
# Hola Ana, estas en edad laboral
```

**Requisitos:**
- Exactamente 2 argumentos (nombre y edad). Si no, `exit 1` con mensaje de `Error`
- La edad debe ser un numero positivo. Si no, `exit 1` con mensaje de `Error`
- Rangos de edad:
  - `< 18`: mensaje con `menor de edad`
  - `18-64`: mensaje con `edad laboral`
  - `>= 65`: mensaje con `jubilacion`
- El nombre del usuario debe aparecer en la salida
- Los errores van a `stderr`

#### 1.2 `clasificar.sh` - Clasificar contenido de un directorio

Recibe una ruta a un directorio y clasifica su contenido por tipo.

```bash
./scripts/clasificar.sh /tmp/mi_directorio
```

**Requisitos:**
- 1 argumento: ruta a un directorio existente. Si no, `exit 1` con `Error`
- Clasificar en tres categorias: `Archivos regulares`, `Directorios`, `Enlaces simbolicos`
- Los enlaces simbolicos NO deben contarse como archivos regulares
- Mostrar un `Resumen` con contadores por tipo, por ejemplo: `Archivos regulares: 2`

#### 1.3 `calculadora.sh` - Calculadora basica

Recibe tres argumentos: numero, operador y numero. Muestra el resultado.

```bash
./scripts/calculadora.sh 6 "*" 7
# 6 * 7 = 42
```

**Requisitos:**
- Exactamente 3 argumentos. Si no, `exit 1` con `Error`
- Ambos numeros deben ser numericos (admite negativos). Si no, `exit 1` con `Error`
- Operadores soportados: `+`, `-`, `*`, `/`
- Operador no soportado: `exit 1` con `Error`
- Division entre cero: `exit 1` con mensaje que contenga `Error` y `cero`
- La division es entera (trunca decimales)
- Formato de salida: `num1 op num2 = resultado`

#### Reto 1.4 `ppt.sh` - Piedra, papel o tijera

Juego interactivo contra la maquina. Lee la eleccion del usuario por stdin en un bucle.

```bash
./scripts/ppt.sh
# Elige (piedra/papel/tijera/salir): piedra
# Maquina elige: tijera
# Ganas!
```

**Requisitos:**
- Acepta `piedra`, `papel`, `tijera` como entradas validas
- `salir` termina el juego
- La maquina elige aleatoriamente entre las tres opciones
- Determina ganador en cada ronda (`gana`, `pierde`, `empate`)
- Al salir, muestra un marcador final (victorias/derrotas/empates)
- Juega multiples rondas (la salida debe tener mas de 3 lineas por varias rondas)

#### Reto 1.5 `genpass.sh` - Generador de contrasenas

Genera contrasenas aleatorias con opciones configurables.

```bash
./scripts/genpass.sh -l 20 -n 3 --no-symbols
```

**Requisitos:**
- Sin argumentos genera 1 contrasena de longitud 16 (por defecto)
- `-l <num>`: longitud de la contrasena
- `-n <num>`: numero de contrasenas a generar
- `--no-symbols`: excluir caracteres especiales (solo alfanumerico)
- `--no-uppercase`: excluir mayusculas
- `--no-numbers`: excluir digitos
- Las flags se pueden combinar
- Mostrar estimacion de entropia (`entropia`, `Entropia`, o `bits`)

---

### Lab 2 - Scripting DevOps

#### 2.1 `top_ips.sh` - Analisis de logs de acceso

Analiza un fichero de log de acceso (formato Apache/Nginx) y muestra las IPs mas frecuentes y un resumen de errores.

```bash
./scripts/top_ips.sh /var/log/access.log
```

**Requisitos:**
- 1 argumento: ruta al fichero de log. Si no, `exit 1` con `Error`
- Mostrar las Top 10 IPs mas frecuentes con su numero de peticiones
- El conteo debe preceder a la IP (ejemplo: `4 192.168.1.100`)
- Mostrar resumen de errores con contadores de `4xx` y `5xx`

#### 2.2 `monitor_disco.sh` - Monitor de uso de disco

Monitoriza el uso de disco y alerta cuando se supera un umbral.

```bash
./scripts/monitor_disco.sh -t 80
```

**Requisitos:**
- Sin argumentos: usa un umbral por defecto. Salida con `INFO`
- `-t <num>`: umbral en porcentaje
- `-h`: muestra ayuda (`Uso` o `uso`)
- Umbral no numerico: `exit 1` con `ERROR` o `Error`
- Codigos de salida: `0` si todo OK, `2` si alguna particion supera el umbral
- La salida incluye timestamps con formato `YYYY-MM-DD`
- Muestra informacion de particiones y puntos de montaje
- Con umbral `1%` debe disparar alertas (`WARN` o `SUPERA`)
- Limpia ficheros temporales al terminar (usa `trap`)

#### 2.3 `backup.sh` - Sistema de backups

Crea backups comprimidos con rotacion automatica.

```bash
./scripts/backup.sh -s /datos -d /backups -n 5
```

**Requisitos:**
- `-s <dir>`: directorio origen (obligatorio)
- `-d <dir>`: directorio destino (obligatorio)
- `-n <num>`: numero maximo de backups a mantener (rotacion)
- `-h`: muestra ayuda
- Sin `-s` o `-d`: `exit 1` con `Error`
- Directorio origen inexistente: `exit 1` con `Error`
- Crea el directorio destino si no existe
- Genera un fichero `.tar.gz` que contenga todos los ficheros del origen
- El nombre del backup debe incluir un timestamp (patron: `backup_<nombre>_YYYYMMDD_HHMMSS.tar.gz`)
- Rotacion: si hay mas backups que `-n`, elimina los mas antiguos
- La salida incluye timestamps con formato `YYYY-MM-DD`

#### Reto 2.4 `csv_report.sh` - Informe desde CSV

Genera un informe a partir de un fichero CSV de empleados.

```bash
./scripts/csv_report.sh empleados.csv
```

El CSV tiene el formato: `nombre,departamento,salario,fecha_alta`

**Requisitos:**
- 1 argumento: ruta al CSV. Si no, `exit 1` con `Error`
- CSV solo con cabecera (sin datos): `exit 1` con error
- Mostrar el numero total de empleados
- Mostrar salario minimo y maximo
- Agrupar empleados por departamento
- Listar los 3 empleados con mayor salario
- Salida formateada (mas de 5 lineas)

#### Reto 2.5 `healthcheck.sh` - Monitor de salud de URLs

Comprueba la disponibilidad de una lista de URLs desde un fichero de configuracion.

```bash
./scripts/healthcheck.sh urls.txt
```

**Requisitos:**
- 1 argumento: ruta al fichero de configuracion con URLs (una por linea)
- Fichero inexistente: `exit 1` con `Error`
- Usa `curl` con timeout (`--max-time` o `-m`)
- Logs con timestamps (formato `YYYY-MM-DD`)
- Detecta respuestas `5xx` y genera alertas
- Usa `flock` o mecanismo de locking para evitar ejecuciones concurrentes

---

### Lab 3 - Testing avanzado

#### 3.1 `tf-wrapper.sh` - Wrapper de Terraform

Script que ejecuta un pipeline de validacion de Terraform: formato, validacion y plan.

```bash
./scripts/tf-wrapper.sh -d infra/
```

**Requisitos:**
- `-d <dir>`: directorio con ficheros `.tf` (obligatorio)
- `-h`: muestra ayuda
- Comprueba que `terraform` esta instalado. Si no, `exit 1`
- Directorio inexistente: `exit 1`
- Argumento desconocido: `exit 1`
- Codigos de salida especificos:
  - `1`: error general (terraform no instalado, directorio no encontrado, argumento invalido)
  - `2`: fallo en `terraform fmt -check`
  - `3`: fallo en `terraform validate`
  - `4`: fallo en `terraform plan`
- La salida incluye timestamps con formato `YYYY-MM-DD`

#### 3.2 `tf-wrapper.bats` - Tests para tf-wrapper.sh

En este ejercicio, **tu escribes los tests**. Crea un fichero BATS que valide el comportamiento de `tf-wrapper.sh`.

> **Nota:** Este fichero va en `scripts/tf-wrapper.bats` (no en la carpeta `tests/`).

**Requisitos:**
- Shebang de bats o bash en la primera linea
- Funcion `setup()` que cree un directorio temporal
- Funcion `teardown()` que limpie el directorio temporal
- Minimo 4 tests con `@test`
- Usar el comando `run` de BATS para ejecutar el script
- Comprobar `$status` para validar codigos de salida
- Tests que cubran al menos:
  - Terraform no instalado (manipular `PATH`)
  - Directorio inexistente
  - Flag de ayuda (`-h`)

---

## Herramientas recomendadas

### Extensiones de VS Code

El repositorio incluye recomendaciones de extensiones. Instalalas para mejor experiencia:

- **BATS** (`jetmartin.bats`): Syntax highlighting para ficheros `.bats`
- **Bash IDE** (`mads-hartmann.bash-ide-vscode`): Autocompletado y navegacion
- **ShellCheck** (`timonwong.shellcheck`): Linter en tiempo real para Bash

### Testing local con BATS

Puedes instalar BATS para probar tus scripts localmente antes de hacer push:

```bash
# macOS
brew install bats-core

# Ubuntu/Debian
sudo apt-get install bats
```

> **Nota:** Los tests oficiales de evaluacion estan en el repositorio del profesor y no son accesibles, pero puedes escribir tus propios tests BATS locales para verificar tu trabajo antes de enviar.

---

## Flujo de trabajo completo

```
1. Fork del repositorio
2. Clonar tu fork
3. Crear rama: git checkout -b soluciones
4. Implementar ejercicios en scripts/
5. Probar localmente
6. Commit y push a tu rama
7. Crear Pull Request en tu fork (base: main)
8. Ver resultados en Actions/Checks del PR
9. Corregir y hacer push si hay fallos (se re-evalua automaticamente)
```

---

## Consejos generales

- **Lee los mensajes de error de los tests.** Te dicen exactamente que se esperaba.
- **Usa `shellcheck`** para detectar errores comunes antes de hacer push.
- **Los mensajes de error van a `stderr`** (`echo "Error: ..." >&2`), no a stdout.
- **Los codigos de salida importan.** Usa `exit 0` para exito y `exit 1` (o el codigo especificado) para errores.
- **No uses tildes ni caracteres especiales** en las palabras clave que buscan los tests (`jubilacion`, no `jubilación`; `Uso`, no `Usó`).
- **Empieza por el Lab 1.** Los ejercicios estan ordenados por dificultad. Los retos son opcionales.
