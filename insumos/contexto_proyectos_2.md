# Instrucciones para Agente: Nuevo Tablero Web

Documento de referencia para replicar la arquitectura del tablero `rxp-tablero` en una nueva carpeta.
El tablero de referencia está en `/home/cb2/japt/rxp-tablero/`.

---

## Contexto

El nuevo tablero comparte la misma arquitectura que `rxp-tablero`:
- Panel de control estático hospedado en GitHub Pages
- Python script que consulta una BD y genera un `data.json`
- `index.html` (vanilla JS) que carga `data.json` cada 15 minutos
- Misma estética visual (paleta, fuentes, componentes)

**Diferencias clave:**
- Base de datos diferente (host, motor y credenciales distintos — ver sección TODO)
- Datos y métricas distintos (el usuario proporcionará las queries y el schema)
- Ya existe un borrador HTML del frontend — úsalo como base en lugar de crear desde cero

---

## Estructura de Carpetas a Crear

```
nuevo-tablero/
├── index.html                  # Borrador existente — adaptar con JS de carga de datos
├── data.json                   # Generado por el updater; iniciar con estructura vacía
├── .gitignore                  # Copiar y adaptar del proyecto de referencia
├── start.sh                    # Script para desarrollo local
│
└── updater/
    ├── updater.py              # Script principal — adaptar de rxp_updater.py
    ├── config.json             # Credenciales BD + queries SQL (NO commitear)
    ├── requirements.txt        # Dependencias Python
    └── exportar_base.py        # Opcional: exportar tablas a Excel para auditoría
```

---

## Paso 1: Inicializar el Repositorio

```bash
mkdir nuevo-tablero && cd nuevo-tablero
git init
git remote add origin https://github.com/Tengorio/NOMBRE-REPO.git  # TODO: definir nombre
```

---

## Paso 2: Configurar `.gitignore`

Copiar de referencia y ajustar:

```
updater/config.json
updater/venv/
updater/__pycache__/
*.log
*.xlsx
*.csv
conexion_db.md
reportes_db/
scratch/
```

---

## Paso 3: Configurar el Updater Python

### 3.1 `updater/requirements.txt`

```
requests>=2.31
# Agregar el driver correspondiente al motor de BD:
# mysql-connector-python>=8.0   (MySQL)
# psycopg2-binary>=2.9          (PostgreSQL)
# pyodbc>=4.0                   (SQL Server)
```

### 3.2 `updater/config.json` — TODO: Completar con credenciales reales

```json
{
  "base_de_datos": {
    "tipo": "mysql",        // TODO: "mysql" | "postgresql" | "sqlserver" | "sqlite"
    "host": "TODO_HOST",
    "port": 3306,           // TODO: ajustar según motor
    "dbname": "TODO_DBNAME",
    "user": "TODO_USER",
    "password": "TODO_PASSWORD"
  },
  "consultas": {
    // TODO: Definir las queries para cada métrica que mostrará el tablero
    // Ver sección "Patrón de Queries" más abajo
  },
  "github": {
    // Opcional: si se actualiza data.json vía GitHub API desde servidor
    "token": "TODO_GITHUB_TOKEN",
    "repo": "Tengorio/NOMBRE-REPO",
    "branch": "main",
    "path": "data.json"
  }
}
```

### 3.3 Patrón de `updater.py`

Copiar `rxp_updater.py` del proyecto de referencia y adaptar:

**Función de conexión** — ya soporta MySQL, PostgreSQL, SQL Server y SQLite sin cambios:
```python
def get_connection(cfg):
    tipo = cfg["tipo"].lower()
    if tipo == "mysql":
        import mysql.connector
        return mysql.connector.connect(
            host=cfg["host"], port=cfg["port"],
            database=cfg["dbname"], user=cfg["user"], password=cfg["password"]
        )
    elif tipo == "postgresql":
        import psycopg2
        return psycopg2.connect(
            host=cfg["host"], port=cfg["port"],
            dbname=cfg["dbname"], user=cfg["user"], password=cfg["password"]
        )
    # ... ver archivo fuente para SQL Server y SQLite
```

**Función de consulta** — adaptar `fetch_counts()` para las métricas del nuevo tablero:
```python
def fetch_counts(conn, cfg):
    cursor = conn.cursor()
    resultado = {}
    for seccion, queries in cfg["consultas"].items():
        resultado[seccion] = {}
        for metrica, sql in queries.items():
            cursor.execute(sql)
            row = cursor.fetchone()
            resultado[seccion][metrica] = int(row[0]) if row and row[0] else 0
    return resultado
```

**Función de guardado** — sin cambios respecto al proyecto de referencia:
```python
def save_data_json(payload, path="data.json"):
    import json, os
    with open(path, "w", encoding="utf-8") as f:
        json.dump(payload, f, ensure_ascii=False, indent=2)
```

**`main()`** — flujo completo:
```python
def main():
    cfg = load_config()          # Lee config.json
    conn = get_connection(cfg["base_de_datos"])
    data = fetch_counts(conn, cfg)
    data["updated_at"] = datetime.now(tz_cdmx).isoformat()
    data["updated_by"] = "updater.py"
    save_data_json(data)
    # Opcional: push a GitHub via API
```

---

## Paso 4: Estructura de `data.json`

El frontend espera un `data.json` con esta forma general. Adaptar las claves a las métricas reales del nuevo tablero:

```json
{
  "seccion_1": {
    "metrica_a": 0,
    "metrica_b": 0
  },
  "seccion_2": {
    "metrica_a": 0,
    "metrica_b": 0
  },
  "updated_at": "2026-01-01T00:00:00-06:00",
  "updated_by": "updater.py"
}
```

**Importante:** Definir la estructura de `data.json` ANTES de escribir el JavaScript del frontend. El JS mapea IDs de elementos HTML a claves de este objeto.

---

## Paso 5: Integrar JS de Carga de Datos en el Frontend

El borrador HTML ya existe con la estética correcta. Agregar este bloque JS al final de `index.html`:

### Patrón de carga (copiar y adaptar)

```javascript
// Constantes de tiempo
const TARGETS = {
  // TODO: Definir fechas límite relevantes para el nuevo tablero
  // Ejemplo del proyecto de referencia:
  // PEE: new Date('2026-08-31T23:59:59-06:00'),
};

const STARTS = {
  // TODO: Definir fechas de inicio si aplica
};

// Carga principal
function loadData() {
  fetch('./data.json?v=' + Date.now())
    .then(r => r.json())
    .then(applyData)
    .catch(err => console.error('Error cargando data.json:', err));
}

// Aplicar datos al DOM
function applyData(d) {
  // TODO: Mapear claves de d a IDs de elementos HTML
  // Patrón del proyecto de referencia:
  const updateVal = (id, val) => {
    const el = document.getElementById(id);
    if (el) {
      el.textContent = typeof val === 'number'
        ? val.toLocaleString('es-MX')
        : val;
      el.classList.toggle('zero', val === 0);
    }
  };

  // Ejemplo:
  // updateVal('total-solicitudes', d.seccion_1.metrica_a);
  // updateVal('solicitudes-evaluadas', d.seccion_1.metrica_b);

  // Timestamp de actualización
  const tsEl = document.getElementById('updated-at');
  if (tsEl && d.updated_at) {
    const dt = new Date(d.updated_at);
    tsEl.textContent = dt.toLocaleString('es-MX', { timeZone: 'America/Mexico_City' });
  }
}

// Inicialización
loadData();
setInterval(loadData, 15 * 60 * 1000); // Recargar cada 15 min
updateCountdowns();
setInterval(updateCountdowns, 1000);
```

---

## Paso 6: Estética Visual (Referencia)

El borrador ya tiene la paleta. Por consistencia, estos son los valores del proyecto de referencia:

### Variables CSS
```css
:root {
  --guinda: #9b2247;
  --verde:  #1e5b4f;
  --dorado: #a57f2c;
  --bg:     #f4f6f9;
  --text-main:  #2c313a;
  --text-muted: #6b7280;
}
```

### Tipografía (Google Fonts)
```html
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Orbitron:wght@400;700&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```
- Headlines: `'Bebas Neue'`
- Números destacados: `'Orbitron'`
- Cuerpo: `'Inter'`

### Componentes reutilizables del proyecto de referencia
- **Flip cards** (dos vistas por panel): perspectiva 3D con `rotateY(180deg)` en hover/toggle
- **Pulsing badge** (indicador de estado en vivo): animación CSS `pulse`
- **Sign panel** (encabezado tricolor): 3 columnas con colores institucionales
- **Responsive grid**: columnas a 800px breakpoint

Ver `/home/cb2/japt/rxp-tablero/index.html` líneas 1–460 para el CSS completo de estos componentes.

---

## Paso 7: Script de Desarrollo Local (`start.sh`)

```bash
#!/bin/bash
cd "$(dirname "$0")"

echo "Iniciando servidor en http://localhost:8080 ..."
python3 -m http.server 8080 &
SERVER_PID=$!

echo "Iniciando updater (cada 15 min)..."
while true; do
  python3 updater/updater.py
  sleep 900
done

wait $SERVER_PID
```

---

## Paso 8: Deploy en GitHub Pages

1. Crear repositorio en GitHub (cuenta: `Tengorio`)
2. Push rama `main`
3. Activar GitHub Pages: Settings → Pages → Branch: `main` → `/` (root)
4. URL pública: `https://tengorio.github.io/NOMBRE-REPO/`

**Actualización automática de `data.json`:**
- El updater puede hacer push a GitHub vía API (ver `rxp_updater.py` del proyecto de referencia, sección de GitHub API)
- Configurar cron en servidor institucional: `0 8,14,20 * * * /ruta/al/script`

---

## TODO: Lista de Decisiones Pendientes

El agente debe solicitar o esperar estos datos del usuario antes de implementar:

- [ ] **Nombre del repositorio GitHub** para el nuevo tablero
- [ ] **Credenciales de la BD** (host, puerto, motor, nombre de BD, usuario, contraseña)
- [ ] **Schema de las tablas** relevantes (nombres de tablas y columnas a consultar)
- [ ] **Queries SQL** para cada métrica que mostrará el tablero
- [ ] **Estructura de `data.json`** — qué secciones y métricas tendrá
- [ ] **Mapeo DOM** — qué IDs de elementos HTML del borrador corresponden a qué métricas
- [ ] **Fechas clave** para countdowns (si aplica al nuevo tablero)
- [ ] **Carpeta destino** donde crear el proyecto

---

## Referencia Rápida: Archivos Fuente

| Archivo fuente | Ruta en proyecto de referencia | Para qué sirve |
|---|---|---|
| HTML completo | `/home/cb2/japt/rxp-tablero/index.html` | Base CSS/JS/componentes |
| Updater Python | `/home/cb2/japt/rxp-tablero/updater/rxp_updater.py` | Lógica de extracción BD |
| Exportador Excel | `/home/cb2/japt/rxp-tablero/updater/exportar_base.py` | Auditoría de datos |
| Script inicio | `/home/cb2/japt/rxp-tablero/start.sh` | Servidor local |
| .gitignore | `/home/cb2/japt/rxp-tablero/.gitignore` | Exclusiones de git |
