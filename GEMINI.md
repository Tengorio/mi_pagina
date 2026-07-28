# GEMINI.md — Contexto Permanente del Proyecto

Este archivo constituye el contexto permanente y normativo para el desarrollo, diseño, redacción y mantenimiento del sitio personal y portafolio profesional de **Javier Arturo Paredes Tenorio**. Es de lectura obligatoria antes de cualquier modificación en el repositorio.

---

## 1. Objetivo del sitio y criterio de éxito medible

- **Objetivo central:** Presentar un portafolio profesional basado en **estudios de caso cuantitativos** que demuestren capacidad técnica real, rigor estadístico e impacto operativo.
- **Criterio de éxito medible:** Que un reclutador técnico, un evaluador de políticas públicas o un comité de posgrado concluya en **menos de 90 segundos**: *"Esta persona sabe modelar y sabe entregar"*.
- **Métricas de calidad del sitio:**
  - 100% de los estudios de caso estructurados bajo la regla obligatoria (incluyendo la decisión técnica no obvia).
  - 0% contenido placeholder, plantillas por defecto o enlaces rotos.
  - Carga veloz e impecable visualización en dispositivos móviles (probado desde 375 px de ancho).

---

## 2. Perfil profesional

- **Nombre canónico en todo el sitio:** **Javier Arturo Paredes Tenorio** *(sin variaciones como "Javier Tenorio" o "Javier Tenorio Tengorio")*.
- **Handle GitHub / Redes:** `tengorio`
- **Formación académica:**
  - **Especialización en Estadística Aplicada** — Instituto de Investigaciones en Matemáticas Aplicadas y en Sistemas (IIMAS), UNAM (2023–2024). Titulado por Examen General de Conocimientos.
  - **Licenciatura en Física** — Facultad de Ciencias, UNAM (2015–2021). Tesis: Espectrómetro didáctico de bajo costo con aplicaciones forenses.
- **Trayectoria profesional:**
  - **Analista Especializado / Científico de Datos** en **Secihti** (Secretaría de Ciencia, Humanidades, Tecnología e Innovación, antes Conahcyt), desde enero de 2025.
  - **Analista Especializado** en **Conahcyt** (agosto – diciembre 2024).
  - **Data Science Analyst** en **Jüsto** (supermercado digital), de marzo a agosto de 2024.
  - **Consultoría independiente:** **Gradiente** (análisis cienciométrico y optimización de procesos).
  - **Docencia universitaria:** Profesor en la Facultad de Ciencias, UNAM (2019–2025): Cálculo Diferencial e Integral I–IV, Termodinámica, Fenómenos Colectivos, Laboratorio de Mecánica y Laboratorio de Electromagnetismo. Tallerista en el programa PAUTA (Adopte Un Talento).
- **Idiomas:** Español (nativo), Inglés (C1), Alemán (A2).
- **Stack tecnológico principal:** Python (`pandas`, `NumPy`, `scikit-learn`, `Streamlit`), R, SQL, LaTeX, QGIS/ArcGIS, Power BI, Git, Quarto.
- **Fortaleza distintiva:** **Estadística seria + capacidad de construir y entregar la herramienta que la pone a operar.** Cruce entre la solidez metodológica cuantitativa de la física/estadística y la ejecución técnica orientada a producción e impacto.

---

## 3. Audiencias prioritarias

El contenido del sitio se redacta y estructura pensando en tres audiencias en estricto orden de prioridad:

1. **Reclutador / Hiring Manager técnico de Ciencia de Datos** (Sector privado, empresas remotas, fintech):
   - *Necesidad:* Quiere ver modelos estadísticos aplicados, código limpio, decisiones de arquitectura y resultados empíricos. Escanea rápido, busca evidencia directa de capacidad técnica.
2. **Organismos de datos para política pública** (J-PAL, BID, Banco Mundial, CEPAL, CONEVAL, Banco de México):
   - *Necesidad:* Buscan rigor metodológico, validez de inferencia, evaluación de impacto y utilidad social en la asignación de recursos públicos.
3. **Comités de admisión de posgrado:**
   - *Necesidad:* Buscan pensamiento cuantitativo riguroso, capacidad de investigación original y sólida fundamentación teórico-matemática.

---

## 4. Stack tecnológico y comandos

- **Framework principal:** **Quarto** (`.qmd`).
  - *REGLA INVIOLABLE:* **NO migrar a otro framework** (como Next.js, Hugo, React, Astro, etc.). Quarto es la herramienta adecuada porque ejecuta Python y R de manera nativa, renderiza Jupyter Notebooks, soporta notación matemática en LaTeX y compila a HTML estático. El cuello de botella histórico del sitio ha sido la calidad del contenido, no la herramienta de renderizado.
- **Alojamiento y despliegue:** GitHub Pages desde la rama `main` (directorio `/docs` o mediante `quarto publish gh-pages`).
- **Comandos de desarrollo:**
  ```bash
  quarto preview    # Servidor local de desarrollo con recarga en vivo
  quarto render     # Compilación estática completa del sitio hacia docs/
  ```

---

## 5. Estructura de archivos del repositorio

```
.
├── _quarto.yml           # Configuración global del sitio Quarto (navegación, tema, opciones)
├── index.qmd             # Portada (Landing): presentación breve + tarjetas de proyectos destacados
├── sobre-mi.qmd          # Trayectoria narrativa, experiencia laboral, docencia y formación
├── cv.qmd                # Currículum Vitae en versión web HTML + enlace al PDF maestro
├── proyectos.qmd         # Índice general de estudios de caso por categoría
├── proyectos/            # Directorio de estudios de caso (artículos técnicos)
│   ├── _plantilla.qmd    # Plantilla base para redactar nuevos estudios de caso
│   ├── apees.qmd         # Caso APEES (priorización lexicográfica de proyectos)
│   ├── mundialmove.qmd   # Caso MundialMove (movilidad urbana con ML + Streamlit)
│   ├── supervivencia-evaluadores.qmd # Caso supervivencia en tiempos de evaluación
│   ├── ocr-azure.qmd     # Caso pipeline de extracción documental OCR
│   └── picnik.qmd        # Caso picnik_webapp (cinética térmica)
├── en/                   # Espejo bilingüe en inglés (misma estructura de navegación)
├── assets/
│   ├── css/custom.scss   # Estilos SCSS personalizados y tokens de diseño
│   ├── img/              # Fotografía profesional e imágenes de soporte
│   └── cv-japt.pdf       # PDF oficial del CV maestro
├── CLAUDE.md             # Insumo previo de agente anterior (referencia histórica)
└── GEMINI.md             # CONTEXTO PERMANENTE Y GUÍA NORMATIVA DEL PROYECTO (Este archivo)
```

---

## 6. Formato obligatorio de estudio de caso

Todo artículo en `proyectos/` debe cumplir estrictamente con la siguiente estructura fija (5 secciones):

1. **El problema:** (2 a 3 frases) Contexto claro de qué estaba roto, para quién y por qué importaba resolverlo.
2. **El enfoque:** (1 párrafo) Explicación de alto nivel sobre la metodología general o herramienta construida.
3. **LA DECISIÓN TÉCNICA QUE NO ERA OBVIA:** *(Sección crítica y obligatoria)*
   - Explicación detallada de qué alternativa técnica razonable se descartó y por qué.
   - *Ejemplo:* "Se utilizó la mediana en lugar de la media aritmética debido al marcado sesgo a la izquierda en las distribuciones de puntuación y a la heterogeneidad en la escala de severidad de los evaluadores."
   - **Sin esta sección, el estudio de caso NO SE PUBLICA.**
4. **Resultado:** Métrica concreta obtenida (R², reducción de tiempos, volumen procesado) o declaración explícita y honesta de que la métrica no fue cuantificada.
5. **Stack y código:** Lista corta de tecnologías empleadas y enlace al repositorio de GitHub correspondiente (si es público).

### Reglas de redacción:
- **Extensión:** Entre 400 y 700 palabras por estudio de caso.
- **Voz y tono:** Voz activa, primera persona del singular, tiempo pasado ("Diseñé", "Formulé", "Construí", no "Se diseñó").
- **Vocabulario:** Prohibidos los adjetivos de relleno empresarial o publicitario (*"innovador"*, *"robusto"*, *"de vanguardia"*, *"potente"*, *"solución integral"*).
- **Rigor de datos:** Usar números reales siempre que existan (ej. `~5,000 proyectos`, `44,890 registros`, `R² = 0.978`). Si no hay métrica registrada, expresarlo explícitamente sin inventar datos.

---

## 7. POLÍTICA DE CONFIDENCIALIDAD (Sección crítica — Inviolable)

Buena parte del trabajo desarrollado en la secretaría federal (Secihti / Conahcyt) involucra **información confidencial, datos personales no públicos y proyectos en proceso de asignación de recursos**.

### Reglas duras de confidencialidad:
1. **Protección de datos reales:** NUNCA publicar datos reales, extractos de bases de datos institucionales, nombres de evaluadores, nombres de solicitantes, nombres de proyectos evaluados ni capturas de pantalla con datos en vivo.
2. **Abstracción metodológica:** Escribir y documentar el **MÉTODO y el ALGORITMO**, no el caso específico de un participante o proyecto. Por ejemplo: la lógica de priorización lexicográfica del algoritmo APEES es completamente publicable como desarrollo metodológico; los montos asignados o el dictamen de una convocatoria específica NO lo son.
3. **Generación de datos sintéticos:** Para ilustrar gráficos, ejecuciones o código de ejemplo, escribir scripts que generen **datos sintéticos** que reproduzcan la forma de la distribución real (sesgo, dispersión, escala) sin corresponder a ninguna persona ni registro real. Los scripts generadores deben adjuntarse al repositorio y el carácter sintético de los datos debe ser explícitamente declarado en el texto.
4. **Protección de criterios de asignación vigentes:** No publicar umbrales, cortes, ponderaciones internas o parámetros que permitan inferir o anticipar decisiones de asignación de fondos públicos en convocatorias abiertas o vigentes.
5. **Criterio de duda:** Ante cualquier duda sobre la sensibilidad de un dato, parámetro o metodología, **preguntar directamente a Javier Arturo Paredes Tenorio** antes de redactar o publicar. No asumir ni decidir de manera autónoma.

---

## 8. Estrategia bilingüe (Español / Inglés)

- **Prioridad:** El sitio es **español primero**, con un espejo estructurado en inglés en la ruta `/en/`.
- **Justificación:** Las postulaciones a roles remotos y organismos internacionales (J-PAL, Banco Mundial, BID) requieren el idioma inglés.
- **Regla de traducción:** **No traducir literalmente.** Reescribir en inglés técnico directo, conciso y natural.
- **Despliegue progresivo:** Los estudios de caso pueden existir inicialmente solo en español; el menú de navegación en `/en/` debe degradar de forma elegante sin generar enlaces rotos.
- **Prioridad de publicación en inglés:** `index.qmd` → 3 casos destacados (APEES, MundialMove, supervivencia) → `cv.qmd` → resto de casos.

---

## 9. Criterios de diseño e interfaz

- **Tipografía y Maquetación:** Limpia, altamente legible, jerarquía clara y con **abundante espacio en blanco** ("mucho aire").
- **Paleta de color:** Un solo color de acento sobre fondo sobrio.
- **Restricciones visuales:** Prohibidas las animaciones complejas, carruseles de imágenes, fondos en movimiento o elementos de distracción.
- **Enfoque técnico:** **Móvil primero** (*Mobile-first*). El sitio debe ser completamente legible y funcional en pantallas de 375 px de ancho.
- **Rendimiento:** Optimizado para carga ultrarrápida bajo conexiones deficientes o de baja latencia.
- **Fotografía:** Una sola fotografía profesional en la página `sobre-mi.qmd`. **Ninguna foto en la portada / landing page.**

---

## 10. Definition of Done (DoD) antes de cada `git push`

Antes de realizar cualquier commit y push a la rama `main`, se deben verificar los siguientes puntos:

- [ ] `quarto render` ejecuta y compila exitosamente sin errores ni advertencias.
- [ ] Cero enlaces rotos o rutas absolutas locales.
- [ ] Cero contenido placeholder, textos `Lorem Ipsum`, notas `TODO` visibles al público o demos por defecto de Quarto (ej. gráfico polar de matplotlib / ecuación $x = 1/2$).
- [ ] Revisión estricta de confidencialidad completada (cumplimiento total de la Sección 7).
- [ ] Nombre canónico verificado en todas las páginas (*Javier Arturo Paredes Tenorio*).
- [ ] Verificación de maquetación responsive a 375 px de ancho de pantalla.
- [ ] Revisión de ortografía, acentuación y gramática en español e inglés.

---

## 11. Backlog priorizado de tareas

### **P0 — Correcciones inmediatas (Daño reputacional actual)**
- [ ] **Eliminar / reemplazar contenido demo por defecto de Quarto:** Remover la vista `projects.html` / demo polar de matplotlib con $x = 1/2$. Asegurar que el menú de proyectos dirija únicamente a contenido real o deshabilitarlo temporalmente.
- [ ] **Preparar migración de URL/Repo:** Planear el renombrado del repositorio de `mi_pagina` a `tengorio.github.io`.
- [ ] **Actualizar `sobre-mi.qmd`:** Reescribir la trayectoria congelada en 2024 para reflejar la experiencia actual (2025–2026 en Secihti: APEES, modelos de supervivencia, motores de asignación, etc.).

### **P1 — Los tres estudios de caso principales**
- [ ] `apees.qmd`: Algoritmo de priorización lexicográfica con criterios de equidad sobre ~5,000 proyectos de investigación. *(Aplicar política de confidencialidad y datos sintéticos).*
- [ ] `mundialmove.qmd`: Aplicación de movilidad urbana en Streamlit con Random Forest ($R^2 = 0.978$), Folium e integración de LLM para razonamiento geoespacial.
- [ ] `supervivencia-evaluadores.qmd`: Modelado de análisis de supervivencia para estimación de tiempos de respuesta a invitaciones de evaluación dictaminadora. *(Aplicar política de confidencialidad).*

### **P2 — Completar portafolio y conexión de repositorios**
- [ ] `ocr-azure.qmd`: Pipeline automatizado de extracción documental.
- [ ] `picnik.qmd`: Aplicación en Streamlit para análisis isoconversional en cinética térmica (`picnik_webapp`).
- [ ] Vincular repositorios públicos existentes en el sitio (`picnik_webapp`, `doi_metadata_extractor`, `directorio-secihti`, `IsoTablas`).
- [ ] Limpiar perfil de GitHub: despinnear repositorios que no correspondan a trabajo original propio (ej. fork `Webscraping_Inmuebles24`).
- [ ] Redactar/mejorar documentación README en cada repositorio público enlazado.

### **P3 — Expansión bilingüe y CV oficial**
- [ ] Creación de espejo bilingüe en `/en/`.
- [ ] Generación e integración del CV descargable en formato PDF (`assets/cv-japt.pdf`) alineado con la versión HTML de `cv.qmd`.

---

## 12. Reglas de trabajo y colaboración

- **Desarrollo incremental:** Un cambio atómico a la vez. No realizar reescrituras masivas en un solo commit.
- **Cero invención de datos:** Si falta información sobre una cifra, resultado o decisión técnica, solicitar la precisión a Javier o dejar un marcador `TODO` explícito en el código fuente. Nunca inventar métricas ni fechas.
- **Formato de Commits:** Commits redactados en español, en modo imperativo y descriptivos (ejemplo: `agrega estudio de caso APEES en proyectos/apees.qmd`).
- **Verificación continua:** No declarar finalizada ninguna tarea sin la correspondiente verificación de renderizado en Quarto (`quarto render`).
