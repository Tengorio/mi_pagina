# CLAUDE.md — Sitio personal de Javier Arturo Paredes Tenorio

Este archivo es el contexto permanente del proyecto. Léelo completo antes de
tocar cualquier archivo.

---

## 1. Qué es esto

Portafolio profesional en **Quarto**, publicado en GitHub Pages.
No es un blog ni un currículum en HTML: es un conjunto de **estudios de caso**
que demuestran capacidad técnica real.

- Repo destino: `tengorio.github.io` (renombrar desde `mi_pagina`)
- URL destino: `https://tengorio.github.io`
- Versión anterior (referencia, no copiar): `https://tengorio.github.io/mi_pagina/`

**Objetivo medible:** que un reclutador técnico o un hiring manager, en menos de
90 segundos, concluya "esta persona sabe modelar y sabe entregar".

---

## 2. Quién es Javier (no preguntes esto, ya está aquí)

- Físico por la Facultad de Ciencias, UNAM. Especialización en Estadística
  Aplicada por el IIMAS, UNAM (titulado por examen general de conocimientos).
- Analista Especializado / científico de datos en **Secihti** (secretaría
  federal de ciencia) desde enero 2025. Antes: **Conahcyt** (ago–dic 2024) y
  **Jüsto**, supermercado digital, como Data Science Analyst (mar–ago 2024).
- Consultoría propia: **Gradiente** (análisis cienciométrico y de procesos).
- Docencia en la Facultad de Ciencias UNAM, 2019–2025: Cálculo I–IV,
  Termodinámica, Fenómenos Colectivos, laboratorios de Mecánica y
  Electromagnetismo.
- Idiomas: español nativo, **inglés C1**, alemán A2.
- Stack: Python (pandas, NumPy, scikit-learn, Streamlit), R, SQL, LaTeX,
  QGIS/ArcGIS, Power BI, Git.
- Fortaleza distintiva: **estadística seria + capacidad de entregar la
  herramienta que la pone a operar.** No es un analista de dashboards ni un
  ingeniero sin fundamento; es el cruce de ambos.

Nombre canónico en todo el sitio: **Javier Arturo Paredes Tenorio**.
(Corregir cualquier variante: "Javier Tenorio", "Javier Tenorio Tengorio".)
Handle: `tengorio`.

---

## 3. Para quién se escribe

Tres audiencias, en este orden de prioridad:

1. **Reclutador / hiring manager de ciencia de datos** (privado, remoto,
   fintech). Quiere ver modelos, código y resultados. Escanea, no lee.
2. **Organismos de datos para política pública** (J-PAL, BID, Banco Mundial,
   CEPAL, CONEVAL, Banco de México). Quieren rigor metodológico, evaluación e
   impacto público.
3. **Comités de admisión de posgrado.** Quieren pensamiento original y
   capacidad de investigación.

Un mismo texto puede servir a las tres si es concreto y honesto.
Nunca escribas para "todo el mundo": escribe para (1) y verifica que (2) y (3)
encuentren lo suyo.

---

## 4. Stack y comandos

```bash
quarto preview        # desarrollo local con recarga
quarto render         # build completo a _site/
```

Publicación: GitHub Pages desde la rama `main`, carpeta `/docs`
(o vía `quarto publish gh-pages`, decidir una y documentarla aquí).

**No migrar a otro framework.** Quarto es la elección correcta: ejecuta
Python y R nativamente, renderiza notebooks, soporta LaTeX y publica sin
fricción. El cuello de botella nunca fue la herramienta, era el contenido.

---

## 5. Estructura

```
.
├── _quarto.yml           # configuración del sitio
├── index.qmd             # landing: quién es + 3 proyectos destacados
├── sobre-mi.qmd          # trayectoria narrativa
├── cv.qmd                # CV en HTML + enlace al PDF
├── proyectos.qmd         # índice de estudios de caso
├── proyectos/
│   ├── _plantilla.qmd    # PLANTILLA — copiar para cada caso nuevo
│   ├── apees.qmd
│   ├── supervivencia-evaluadores.qmd
│   ├── mundialmove.qmd
│   ├── ocr-azure.qmd
│   └── picnik.qmd
├── en/                   # espejo en inglés (misma estructura)
├── assets/
│   ├── css/custom.scss
│   └── img/
└── CLAUDE.md
```

---

## 6. Formato obligatorio de un estudio de caso

Cada archivo en `proyectos/` sigue esta estructura. **Sin excepciones.**

1. **El problema** (2–3 frases). Qué estaba roto, para quién, por qué importaba.
2. **El enfoque** (1 párrafo). Qué se hizo, a grandes rasgos.
3. **La decisión técnica no obvia** ← *esta es la sección que importa.*
   Qué alternativa se descartó y por qué. Ejemplo: "usé mediana en vez de media
   porque las distribuciones estaban sesgadas a la izquierda y había
   heterogeneidad de escala entre evaluadores". Sin esta sección, el caso no se
   publica.
4. **Resultado** con número concreto, o declaración honesta de que no se midió.
5. **Stack** (lista corta) y enlace al repo si es público.

**Extensión:** 400–700 palabras. Si pasa de 700, sobra algo.

### Reglas de escritura

- Voz activa, primera persona, pasado. "Diseñé", no "se diseñó".
- Nada de adjetivos de relleno: *innovador*, *robusto*, *de vanguardia*,
  *potente*, *solución integral*. Si aparecen, bórralos.
- Números siempre que existan: ~5,000 proyectos, 44,890 registros, R²=0.978.
- Si no hubo métrica, dilo. La honestidad se nota y suma.
- Todo caso con código público debe enlazar el repo. Todo repo enlazado debe
  tener README decente.

---

## 7. Confidencialidad — LEER ANTES DE ESCRIBIR SOBRE TRABAJO INSTITUCIONAL

Buena parte del trabajo de Javier es de una secretaría federal e involucra
datos de personas y proyectos que **no son públicos**.

Reglas duras:

- **Nunca** publicar datos reales, extractos de bases institucionales, nombres
  de evaluadores, de solicitantes o de proyectos, ni capturas con datos vivos.
- Escribir el **método**, no el caso. La lógica lexicográfica del APEES es
  publicable; los resultados de la convocatoria no.
- Para ilustrar: **generar datos sintéticos** que reproduzcan la forma de la
  distribución (sesgo, escala, dispersión) sin corresponder a nada real. El
  script generador va en el repo y se declara como sintético en el texto.
- No publicar umbrales, cortes ni parámetros que permitan inferir o anticipar
  decisiones de asignación de recursos aún vigentes.
- Ante la duda: pregúntale a Javier antes de publicar. No decidas tú.

---

## 8. Bilingüe

El sitio es español-primero con espejo en inglés en `/en/`.
Razón: los roles remotos y los organismos internacionales (J-PAL, Banco
Mundial) leen inglés. Con C1 no hay excusa para no tenerlo.

- No traducir literal: reescribir. El inglés técnico es más directo y más corto.
- Prioridad de traducción: `index` → 3 casos destacados → `cv` → el resto.
- Un caso puede existir solo en español mientras se traduce; el menú debe
  degradar con elegancia, no romperse.

---

## 9. Diseño

- Tipografía legible, jerarquía clara, mucho aire. Un solo color de acento.
- Sin animaciones, sin carruseles, sin fondos de partículas.
- Móvil primero: mucha gente abrirá esto desde el teléfono.
- Debe cargar rápido con conexión mala.
- Foto: una, profesional, en `sobre-mi`. No en el landing.

---

## 10. Definition of done (antes de cada `git push`)

- [ ] `quarto render` corre sin errores ni warnings
- [ ] Cero enlaces rotos, cero pestañas con contenido de ejemplo
- [ ] Ninguna página muestra placeholders (`Lorem`, `TODO`, demos de Quarto)
- [ ] Revisión de confidencialidad hecha (sección 7)
- [ ] Nombre canónico correcto en todas las páginas
- [ ] Se ve bien en pantalla de 375 px de ancho
- [ ] Ortografía y acentuación revisadas

---

## 11. Backlog priorizado

**P0 — hacer primero, es lo que más daña hoy**
- [ ] Eliminar la página `projects.html` heredada (contiene el demo por
      defecto de Quarto: gráfico polar de matplotlib y `x = 1/2`). Quitarla
      del menú hasta tener contenido real.
- [ ] Renombrar el repo `mi_pagina` → `tengorio.github.io`.
- [ ] Actualizar `sobre-mi`: la versión vieja describe el puesto en Secihti
      solo como seguimiento técnico, OCR y Crossref. Falta todo lo de 2025–26.

**P1 — los tres casos que sostienen el portafolio**
- [ ] `apees.qmd` — algoritmo de priorización con criterios de equidad sobre
      ~5,000 proyectos. Fases lexicográficas, mediana sobre media, franja
      crítica. Fundamento en teoría de elección social. *Aplicar sección 7.*
- [ ] `mundialmove.qmd` — app de movilidad en Streamlit, Random Forest
      (R²=0.978), Folium, LLM como motor de razonamiento geoespacial.
- [ ] `supervivencia-evaluadores.qmd` — modelos de supervivencia para tiempos
      de respuesta a invitaciones de evaluación. *Aplicar sección 7.*

**P2 — completar y conectar**
- [ ] `ocr-azure.qmd` — pipeline de extracción documental.
- [ ] `picnik.qmd` — app Streamlit de análisis isoconversional (repo público:
      `picnik_webapp`).
- [ ] Enlazar desde el sitio los repos existentes: `picnik_webapp`,
      `doi_metadata_extractor`, `directorio-secihti`, `IsoTablas`.
- [ ] Limpiar el perfil de GitHub: el fork de `Webscraping_Inmuebles24` está
      entre los repos destacados y no es trabajo propio; despinnearlo.
- [ ] README decente en cada repo enlazado.

**P3**
- [ ] Espejo en inglés.
- [ ] CV en PDF descargable, generado desde `cv.qmd`.
- [ ] Notas técnicas cortas (opcional, solo si hay algo que decir).

---

## 12. Cómo trabajar conmigo

- Un cambio a la vez. Nada de reescribir el sitio entero en un commit.
- Antes de escribir un caso de estudio, **pregunta por los detalles que faltan**
  (cifras, decisiones, qué se descartó). No inventes números ni resultados.
  Un dato inventado en un portafolio se convierte en una mentira en una
  entrevista.
- Si algo del backlog ya no aplica, propón quitarlo en vez de hacerlo a medias.
- Commits en español, imperativo, concretos: `agrega caso de estudio APEES`.
