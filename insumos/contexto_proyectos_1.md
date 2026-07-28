# Reporte de Auditoría Técnica y Extracción de Evidencia de Ingenieria de Datos, Estadística Aplicada y ML

Candidato / Investigador: Javier Arturo Paredes Tenorio
Rol del Evaluador: Investigador Senior de Código y Auditor de Datos
Propósito: Sintetizar evidencia técnica, decisiones no obvias, arquitectura
de software y métricas empíricas para el enriquecimiento del Currículum Vitae
y Portafolio Profesional.
──────

## 1\. Proyecto: sp (Sistema de Gestión, Auditoría y Clasificación ICPC)

• Ruta local: /home/japt/sp
• Categoría: Automatización \& Data Eng / Procesamiento Documental \& OCR
• Stack Principal: Python 3.11 (pandas, openpyxl), EasyOCR, SQLite (sqlite3),
Streamlit, Regex.

### 1\. El Problema Resuelto

Inconsistencias y vacíos de trazabilidad en la auditoría de un universo de
\~760 proyectos de investigación científica (fondo CB-2017-2018), provocados
por dispersión de fuentes, metadatos incidentales y registros desactualizados
en oficios de trámite (OTI) y certificados de entrega (CETA). Se requería
auditar signatarios reales, reconciliar la entrega de productos compromisos y
automatizar la generación de dictámenes ICPC.

### 2\. El Enfoque y Metodología

• Pipeline ETL \& Normalización Jerárquica: Desarrollo de scripts de
sincronización (enriquecer\_base\_v6.py, parse\_rutas.py) basados en
diccionarios conceptuales para clasificar rutas documentales sin redundancia.
• Pipeline OCR \& Auditoría de Dictámenes: Extracción automatizada con EasyOCR
sobre 363 PDFs de oficios para rescatar el dictamen real (sobresaliente,
satisfactorio, cumplio\_sin\_nivel) e identificar los signatarios efectivos de
cada período (Eras I, II y 0).
• Modelo de Batches \& Priorización: Categorización del universo en batches (A,
B, C, D\_OTI, D\_OTE3) asociados a prioridades (P1 a P7) con trazabilidad
SQLite (db/cb\_2017\_2018.db).

### 3\. La Decisión Técnica No Obvia (Crítica)

• Prevalencia de Dictámenes OCR sobre Discriminantes Matemáticos y
Diferenciación de Formulas Era I vs Era II:
• Alternativa descartada: Aplicar un ratio discriminante uniforme (



&#x20;           entregados
    disc = ─────────────
           comprometidos


) o confiar ciegamente en la metadata histórica de la base de datos.

• Razonamiento técnico: Para la Era I (Gelover), los intervalos de confianza
del 95% (LI = 1.149,LS = 1.623) no reflejaban el criterio real de
dictaminación institucional; por ello, se estableció que el dictamen extraído
por OCR prevalece sobre el cálculo. Para la Era II (Carlo/Triana), se
descartó el ratio simple y se implementó un discriminante de distancia
absoluta

&#x20;             │E          + Add - Faltantes│
              │ efectivos                  │
    disc\_v1 = ──────────────────────────────
                      Comprometidos


con topes de percentiles IQR (Q3 = 0 → CAPS\_RAROS) y tratamiento opcional de
reportes técnicos (rtec), evitando penalizar a proyectos por discrepancias de
catálogo.

### 4\. Métricas y Resultados Cuantitativos

• Volumen procesado: Auditoría completa de \~760 proyectos y 358 OTIs (205 de
un firmante, 87 de otro, 66 de un tercero).
• Batching \& Reconciliación: Identificación exacta de 138 proyectos P1
(listos para emisión de ICPCs), 219 en P3, y 66 en P4.
• Detección y corrección de anomalías: Corrección del proyecto A1-S-37838,
donde la extracción previa mediante un LLM había inflado la variable de
compromisos CAR\_db de 1 a 102 registros.

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 5
• Estado de código público: Requiere sanitización de datos (los datos
institucionales deben ser reemplazados por una base sintética de proyectos de
investigación).
──────

## 2\. Proyecto: tablero-cbf2026 (Mecanismo APEES \& Dashboard de Prelación)

• Ruta local: /home/japt/tablero-cbf2026
• Categoría: Optimización / Estadística Aplicada / Web App
• Stack Principal: React, Vite, Node.js (Plugin de Backend embebido),
JavaScript (ES6+), MySQL, HTML5/CSS3.

### 1\. El Problema Resuelto

Asignación inequitativa e ineficiente de fondos públicos de investigación en
convocatorias masivas nacionales (ej. CBF-2026, IH, PEE), donde el
ordenamiento exclusivo por promedio aritmético de calificaciones genera
empates masivos e ignora distorsiones de equidad social y rezago regional
entre instituciones participantes.

### 2\. El Enfoque y Metodología

• Algoritmo APEES (Scaled Prioritization with Social Equity): Implementación
en JavaScript del algoritmo determinista en tres fases:
1. Fase 0: Cálculo de agregador robusto (mediana recortada por propuesta).
2. Fase 1: Ordenamiento por excelencia científica (



&#x20;   r
     ref


).
3. Fase 2: Definición de la franja crítica de indiferencia estadística (±δ)
alrededor del corte, y reordenamiento lexicográfico por desventaja compuesta
(equidad de género, instituciones vulnerables, regiones rezagadas, primera
generación universitaria, pertenencia a grupos prioritarios y reintentos R4).

• Dashboard Web de Alta Performance: Interfaz reactiva para la carga de
matrices de calificaciones, ajuste de parámetros (θ,δ, presupuesto, meta de
proyectos targetN) y simulación en tiempo real.

### 3\. La Decisión Técnica No Obvia (Crítica)

• Corte por Meta de Proyectos (targetN) vs. Corte por Presupuesto Acumulado \&
Aislamiento de Líneas Prioritarias:
• Alternativa descartada: Realizar el corte acumulando presupuestos
solicitados hasta agotar la bolsa.
• Razonamiento técnico: El corte por presupuesto acumulado causaba
inestabilidad marginal debido a proyectos con presupuestos dispares en el
umbral de corte. Se optó por fijar la meta de proyectos (targetN = 484),
utilizando la calificación de la propuesta en la posición targetN como



&#x20;   r
     ref


para trazar la franja δ = 0.05. Además, se descartó sumar la "Línea
Prioritaria (LP)" dentro de la escala de equidad social; se aisló como una
condición fija antecedente (R₅) para evitar distorsionar los índices de
vulnerabilidad social.

### 4\. Métricas y Resultados Cuantitativos

• Volumen procesado: Universo de 3,631 propuestas filtradas en base de datos
(rev\_tecnica != 'Descartada' AND rev\_admon = 'Aprobada'), resultando en 2,062
proyectos elegibles.
• Métricas de corte (CBF-2026):

&#x20;   r    = 9.19
     ref


, franja crítica \[9.14,9.24] conteniendo 154 proyectos reordenados por
criterios de equidad, logrando la asignación transparente de $336,497,131
MXN.

• Rendimiento: Ejecución en el navegador en menos de 200 ms para más de 3,600
filas con filtrado en memoria.

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 5
• Estado de código público: Público (el motor src/lib/apees.js y componentes
pueden mostrarse abiertamente con datos simulados).
──────

## 3\. Proyecto: formalizacion-cbf-2026 (Modelado Estocástico y Aditividad de

Formalización)

• Ruta local: /home/japt/formalizacion-cbf-2026
• Categoría: Machine Learning / Estadística Aplicada / Optimización
• Stack Principal: Python 3.11 (numpy, pandas, scipy), Beta-Binomial
Jerárquico, Teoría de Colas (M/M/k), HTML5/JS (Dashboard).

### 1\. El Problema Resuelto

Incertidumbre en la proyección de tiempos y cuellos de botella para la
formalización administrativa de N = 484 proyectos de investigación que
requieren la firma del Convenio de Asignación de Recursos (CAR) por 6 figuras
administrativas (3 institucionales: RA, RL, RT; y 3 de la secretaría: TUAF,
STS, ST).

### 2\. El Enfoque y Metodología

• Modelado Bayesian-Jerárquico (Beta-Binomial): Inferencia estocástica con
contracción parcial (Empirical Bayes / shrinkage) para calcular las
probabilidades institucionales de re-tramitación por cambios en figuras
administrativas (pᴿᴬ,pᴿᴸ,pᴿᵀ).
• Aditividad Tipo Ley de Hess: Descomposición del costo/tiempo administrativo
esperado en función de los cuatro caminos posibles de tramitación (R₁ directo,
R₂ con cambios RALT, R₃ con ajuste financiero, R₄ ambos).
• Ciclos Reversibles y Teoría de Colas: Modelado de rebotes en vistos buenos
(VoBo) mediante distribución Geométrica (

&#x20;   1/p  - 1
       c


) y teoría de colas M/M/1 / M/M/k para la saturación de los firmantes
oficiales.

### 3\. La Decisión Técnica No Obvia (Crítica)

• Descomposición Jerárquica Independiente por Figura (RA,RL,RT) vs Tasa
Agregada Global \& Migración Inferencia Python:
• Alternativa descartada: Estimar una tasa agregada simple



&#x20;    RALT
    p


por institución o realizar la inferencia en tiempo real con JavaScript en el
frontend.

• Razonamiento técnico: Asumir una tasa agregada ocultaba que las
instituciones tienen 1 Representante Legal (RL), pero múltiples Responsables
Administrativos (RA) y Técnicos (RT), distorsionando los denominadores. Se
derivó la fórmula estocástica

&#x20;    RALT
    p     = 1 - (1 - pᴿᴬ)(1 - pᴿᴸ)(1 - pᴿᵀ)


. Asimismo, se aisló el motor bayesiano en Python (src/inferencia.py) para
exportar artefactos estáticos (datos\_calibrados\_2026.js), garantizando
reproducibilidad y eliminando la latencia en el tablero cliente.

### 4\. Métricas y Resultados Cuantitativos

• Volumen procesado: N = 484 proyectos proyectados a través de 4 rutas
administrativas (R₁ a R₄).
• Precisión de Inferencia: Contracción bayesiana ajustada por hiperparámetros
α,β estimados por método de momentos sobre historiales de múltiples
convocatorias anuales.
• Optimización de proceso: Identificación del firmante TUAF/SA como el cuello
de botella primario en el flujo de colas (§7.2 del formalismo).

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 5
• Estado de código público: Requiere sanitización de datos (el formalismo
matemático en docs/formalismo\_mecanismo.md y los scripts en src/ son 100%
elegibles para publicación técnica).
──────

## 4\. Proyecto: apees\_article (Manuscrito Científico OUP - Science and Public

Policy)

• Ruta local: /home/japt/apees\_article
• Categoría: Estadística Aplicada / Optimización / Publicación Científica
• Stack Principal: LaTeX (oup-authoring-template), TeXLive, Markdown,
Scripting de Integración.

### 1\. El Problema Resuelto

Falta de mecanismos formales, auditables y con soporte axiomatico en la
literatura de política científica para resolver el problema de la
priorización de fondos públicos de investigación en América Latina,
distinguiendo conceptualmente la fase de evaluación por pares de la fase de
priorización presupuestal.

### 2\. El Enfoque y Metodología

• Redacción y Formato Académico: Preparación del manuscrito "Designing
Auditable Priority Mechanisms for Public Research Funding: Merit, Equity, and
the APEES Algorithm" para la revista Science and Public Policy (Oxford
University Press).
• Análisis Axiomático Formal: Demostración matemática del comportamiento del
algoritmo APEES frente a las Condiciones de Imposibilidad de Arrow (1951) y
los teoremas de Incompatibilidad de Incentivos de Gibbard-Satterthwaite.
• Calibración Empírica: Validación del ancho de banda de indiferencia (δ = 0.
5) sobre datos históricos.

### 3\. La Decisión Técnica No Obvia (Crítica)

• Violación Deliberada Aislada de los Principios de Arrow dentro de la Franja
Crítica (ℱ):
• Alternativa descartada: Exigir la propiedad de Independencia de
Alternativas Irrelevantes (IIA) y Eficiencia de Pareto en todo el dominio
de propuestas.
• Razonamiento técnico: Demostrar que dentro de la franja crítica de
indiferencia (ℱ), las calificaciones son estadísticamente indistinguibles
debido al bajo acuerdo inter-evaluador (r-confiabilidad de pares).
Violando deliberadamente IIA únicamente dentro de esta franja de
indiferencia, se habilita la activación legítima de criterios de equidad
social sin comprometer el mérito científico global.



### 4\. Métricas y Resultados Cuantitativos

• Volumen de validación empírica: Calibración basada en 5,078 propuestas
evaluadas en convocatorias nacionales.
• Rigor de integración: Manuscrito estructurado de 7 secciones, cumplimiento
estricto del límite de Abstract (≤150 palabras; exacto 148 palabras) y
conversión de 28 referencias bibliográficas maestras a notas al pie según el
estándar OUP SPP.

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 5
• Estado de código público: Público (Manuscrito en LaTeX elegible para
repositorios como arXiv / SSRN).
──────

## 5\. Proyecto: fusionador (Aplicación Desktop GUI para Expedientes VeriSAT)

• Ruta local: /home/japt/fusionador
• Categoría: Automatización \& Data Eng / Software Desktop
• Stack Principal: Python 3.11, tkinter, pypdf, reportlab, openpyxl,
pyinstaller (Windows Executable).

### 1\. El Problema Resuelto

Eficiencia operativa deficiente al preparar e imprimir lotes de expedientes
documentales PDF de auditoría (VeriSAT), los cuales carecen de
identificadores visibles en su cuerpo. Existía además el riesgo de
desalineación física de hojas al imprimir documentos de páginas impares a
doble cara (dúplex).

### 2\. El Enfoque y Metodología

• Arquitectura Multihilo Desktop: Aplicación GUI en Tkinter con ejecutable
portable (.exe standalone de \~25 MB generado vía PyInstaller), integrando
worker threads para prevenir el congelamiento de la interfaz durante
operaciones I/O pesadas.
• Motor de Fusión \& Indización Automática: Ensamblado de PDFs con pypdf,
generación dinámica de carátulas de índice en PDF multicolumna mediante
reportlab y exportación paralela de índices auditables en Excel con openpyxl.

### 3\. La Decisión Técnica No Obvia (Crítica)

• Regex en Metadatos de Nombre de Archivo vs. OCR en Contenido \& Paridad
Forzada Impare:
• Alternativa descartada: Ejecutar un pipeline OCR sobre el cuerpo de
cada hoja de los PDFs o realizar un armado manual de páginas.
• Razonamiento técnico: El procesamiento OCR introducía latencias
inaceptables (\~30-60 s por documento) en equipos sin GPU dedicada. Se
eligió una arquitectura de detección por patrones Regex configurables
(KNOWN\_PATTERNS) en el nombre de archivo. Adicionalmente, se implementó
una lógica de inserción automática de hojas en blanco si un PDF
individual posee un número impar de páginas, garantizando que cada
documento inicie invariablemente en el anverso de la hoja al imprimir a
doble cara.



### 4\. Métricas y Resultados Cuantitativos

• Despliegue operativo: Generación de un ejecutable único de \~25 MB probado y
distribuido para uso final sin necesidad de instalación de entornos Python.
• Rendimiento: Fusión e indización instantánea de cientos de expedientes PDF
en menos de 5 segundos; 100% de eliminación de desalineaciones en impresión
física dúplex.

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 4
• Estado de código público: Público (Código fuente de la aplicación GUI
completamente libre de datos confidenciales).
──────

## 6\. Proyecto: prelacion\_2026 (Análisis Estadístico Empírico de Agregadores

y Franja de Mediana)

• Ruta local: /home/japt/prelacion\_2026
• Categoría: Estadística Aplicada / Data Eng
• Stack Principal: Python 3.11 (pandas, numpy, scipy, matplotlib), Jupyter
Notebooks.

### 1\. El Problema Resuelto

Sensibilidad e inestabilidad en las decisiones de otorgamiento de fondos de
investigación causadas por la métrica de agregación utilizada en páneles de
evaluación por pares, donde la media aritmética tradicional se ve severamente
distorsionada por evaluadores atípicos (extremadamente laxos o estrictos).

### 2\. El Enfoque y Metodología

• Análisis Exploratorio de Datos (EDA) Masivo: Evaluación de la varianza
inter e intra-evaluador en dictámenes históricos (convocatorias CBF-2025 e
IH-2025).
• Evaluación de Agregadores Estocásticos: Comparación empírica entre el
promedio aritmético y la mediana recortada.
• Modelado de Reglas de Desempate y Revisiones Asimétricas: Verificación de
reglas operativas donde una revisión "No Aprobada" activa un tercer evaluador
pero es excluida del promedio cuantitativo final.

### 3\. La Decisión Técnica No Obvia (Crítica)

• Adopción de Mediana Recortada frente a la Media Aritmética \& Exclusión de
Revisiones Rechazadas en Ventana de Desempate:
• Alternativa descartada: Mantener el promedio ponderado convencional de
todas las revisiones recibidas.
• Razonamiento técnico: Se demostró empíricamente que la media aritmética
sufre de baja robustez breakdown (0%), permitiendo que un solo evaluador
sesgado descalifique una propuesta excelente. Se adoptó la mediana
recortada y se demostró que en esquemas de 3 evaluadores por desempate,
promediar la calificación de la revisión "No Aprobada" inflaba la
varianza; la regla óptima requiere usar la evaluación negativa como
detonador de revisión, pero promediar únicamente las revisiones aprobadas
numéricamente.



### 4\. Métricas y Resultados Cuantitativos

• Volumen de datos: Análisis de 515,646 filas de evaluaciones desagregadas en
CBF-2025 (reporte\_completo\_califs.xlsx).
• Caracterización de franja: Identificación precisa de la franja de
indiferencia directiva \[8.975,9.075] (δ = 0.05) conteniendo 226 proyectos
disputados en CBF-2025 y 3,620 proyectos rankeados en CBF-2026.

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 4
• Estado de código público: Requiere sanitización de datos (los notebooks de
análisis estadístico pueden hacerse públicos reemplazando las
identificaciones de las propuestas).
──────

## 7\. Proyecto: rxp (Verificación de Conflictos de Interés COI \& Asignación

RCEEA)

• Ruta local: /home/japt/rxp
• Categoría: Machine Learning / Optimización / Data Eng
• Stack Principal: Python 3.11 (pandas, numpy, pickle), Regex.

### 1\. El Problema Resuelto

Riesgo de sesgo por Conflictos de Interés (COI) y asignación inválida en la
evaluación por pares de convocatorias simultáneas (CBF, IH, PEE 2026), debido
a evaluadores registrados que al mismo tiempo fungen como investigadores
solicitantes en la misma u otra convocatoria del ciclo.

### 2\. El Enfoque y Metodología

• Cruce Relacional de Alta Velocidad: Pipeline de normalización de
identificadores de Currículum Vitae Único (CVU) y nombres.
• Clasificación RCEEA: Emparejamiento borroso y estricto contra el Registro
de Evaluadores Externos (bases 1999-2018, 2019-2023, RCEEA-2025 y RCEEA-2026).
• Matriz de Validación de Conflictos Cross-Convocatoria: Detección de
traslapes en matrices N:M entre listas de asignación y reportes de
solicitantes.

### 3\. La Decisión Técnica No Obvia (Crítica)

• Matriz Granular Inter/Intra-Convocatoria vs. Descalificación Global \&
Caching Pickle:
• Alternativa descartada: Descalificar globalmente a cualquier evaluador
que tuviese una solicitud activa en el sistema.
• Razonamiento técnico: Una descalificación global reducía drásticamente
el padrón de dictaminadores calificados. Se implementó un motor de
filtrado granular por matriz de convocatoria (convocatoria\_asignacion vs
convocatoria\_solicitud) que distingue conflictos estrictos de
asignaciones válidas en áreas no competidoras. Además, se implementó
serialización en formato .pkl para optimizar la lectura repetitiva de
millones de pares evaluador-solicitud en memoria.



### 4\. Métricas y Resultados Cuantitativos

• Volumen procesado: Auditoría y deduplicación de miles de CVUs únicos
cruzados contra el universo RCEEA histórico (1999–2026).
• Calidad auditada: 100% de eliminación de asignaciones con conflicto de
interés en las carpetas de salida (asignaciones\_para\_enviar).

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 4
• Estado de código público: Requiere sanitización de datos (código de
matching 100% reutilizable; datos personales CVU deben anonimizarse).
──────

## 8\. Proyecto: verisat (Consolidación e Indización de Expedientes Fiscales)

• Ruta local: /home/japt/verisat
• Categoría: Automatización \& Data Eng
• Stack Principal: Python 3.11, pypdf, reportlab, openpyxl.

### 1\. El Problema Resuelto

Dificultad de consulta y trazabilidad en expedientes de verificación del SAT
(VeriSAT) integrados por múltiples fragmentos PDF de una sola página sin
foliación unificada ni tabla de contenido ejecutable.

### 2\. El Enfoque y Metodología

• Script de Ensamblado e Indización: Procesamiento automatizado que detecta
archivos bajo patrones 04\_\*\_VeriSAT.pdf, calcula el ordenamiento de páginas e
inserta dinámicamente una carátula de índice estilizada en PDF (3 columnas) y
una hoja de control en Excel.
• Validación Layout \& Canvas: Verificación en tiempo de ejecución de que la
tabla de índice encaje exactamente en 1 sola página mediante cálculo dinámico
de alturas de fila y paddings con reportlab.

### 3\. La Decisión Técnica No Obvia (Crítica)

• Construcción de Tabla Multicolumna Anidada en ReportLab vs. Generación
Multi-página:
• Alternativa descartada: Permitir que el índice fluyera libremente a
través de 2 o más páginas.
• Razonamiento técnico: Para fines de auditoría física, la carátula debe
actuar como una hoja resumen de una vista. Se diseñó una estructura donde
las sub-tablas se dividen en grupos (rows\_per\_col = ceil(N / n\_cols)) y
se envuelven dentro de una tabla contenedora externa con celdas de
separación implícitas (GAP = 0.5 cm).



### 4\. Métricas y Resultados Cuantitativos

• Volumen procesado: Consolidación e indización automática de lotes de
expedientes VeriSAT.
• Exactitud: Reducción a 0 páginas excedentes en carátula y exportación
síncrona a Excel en menos de 2 segundos.

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 3
• Estado de código público: Público (código utility scripts).
──────

## 9\. Proyecto: fac\_formulario (Formularios Financieros de Ajuste de Costos)

• Ruta local: /home/japt/fac\_formulario
• Categoría: Web App / Data Eng
• Stack Principal: HTML5, CSS3, JavaScript (Vanilla ES6), JSON Data
Interchange.

### 1\. El Problema Resuelto

Captura inconsistente y propensa a errores manuales en la revisión financiera
de rubros de gasto para los Formulario de Ajuste de Costos (FAC) en la
convocatoria CBF (período 2026-2031).

### 2\. El Enfoque y Metodología

• Plantillas Web Interactivas: Desarrollo de componentes livianos en HTML/JS
para presentar y editar formularios FAC específicos por proyecto (FAC\_CBF-
2026-\*\_RT.html).
• Validación Financiera Cliente-Servidor: Cálculo en tiempo real de techos
presupuestales y desglose por partidas (becas, infraestructura, reactivos,
viajes) previniendo descuadres de suma antes del envío.

### 3\. La Decisión Técnica No Obvia (Crítica)

• Generación de Interfaz HTML Autónoma Standalone por Proyecto vs. SPA
Compleja:
• Alternativa descartada: Desplegar un framework SPA pesado
(React/Angular) conectado a un servidor backend dedicado para las
revisiones financieras FAC.
• Razonamiento técnico: El equipo de dictaminadores financieros operaba
en entornos con conectividad intermitente y restricciones de red. Se
generaron archivos HTML/JS autocontenidos que cargan la estructura del
proyecto directamente en el DOM, permitiendo edición offline y
exportación de JSONs validados.



### 4\. Métricas y Resultados Cuantitativos

• Volumen procesado: Decenas de plantillas HTML financieras generadas
dinámicamente (FAC\_CBF-2026-XXXX\_RT.html).
• Calidad: Cero errores de desbordamiento presupuestal en las solicitudes
capturadas.

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 3
• Estado de código público: Público (Plantillas de interfaz y validadores
Vanilla JS sanitizables).
──────

## 10\. Proyecto: sankey\_dineros (Visualización Dinámica de Flujos

Presupuestales)

• Ruta local: /home/japt/sankey\_dineros
• Categoría: Data Eng / Visualización de Datos
• Stack Principal: Python 3.11, Plotly (plotly.graph\_objects), HTML5, pandas.

### 1\. El Problema Resuelto

Opacidad y dificultad analítica para visualizar la distribución y el
ejercicio del presupuesto asignado a programas de fomento a la investigación
(ej. fondos F003 / ejercido 2025) a través de múltiples niveles de agregación
institucional y partidas individuales.

### 2\. El Enfoque y Metodología

• Ingeniería de Visualización de Flujos: Script en Python (sankey\_f003.py)
que transforma registros de contabilidad financiera en estructuras de nodos y
enlaces (sources, targets, values, colors).
• Exportación a Dashboards HTML Interactivos: Generación de diagramas Sankey
interactivos (sankey\_f003\_2025\_ejercido.html) con tooltips dinámicos,
resaltado de trayectorias presupuestales y paletas cromáticas accesibles.

### 3\. La Decisión Técnica No Obvia (Crítica)

• Generación de Sankey Vía Grafos en Plotly con Nodos Jerárquicos Únicos vs.
Librerías de Inteligencia de Negocios (PowerBI/Tableau):
• Alternativa descartada: Exportar dashboards estáticos en PowerBI o
Tableau.
• Razonamiento técnico: Las herramientas BI estándar no permitían la
incrustación limpia ni el control programático exacto sobre el mapeo de
colores según la naturaleza del gasto. Se programó el pipeline
directamente en Python/Plotly, deduplicando claves de nodo para evitar
ciclos recursivos en los diagramas Sankey.



### 4\. Métricas y Resultados Cuantitativos

• Volumen auditado: Renderizado interactivo del 100% de las partidas de
presupuesto ejercido F003 2025.
• Interactividad: Tiempos de carga <1 s en navegador web estándar.

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 3
• Estado de código público: Público (Scripts en Python sanitizando los datos
de montos).
──────

## 11\. Proyecto: semillero (Procesamiento con Docling \& Agentes de Tendencias

Científicas)

• Ruta local: /home/japt/semillero
• Categoría: Machine Learning / NLP \& GenAI / Data Eng
• Stack Principal: Python 3.11, Docling (PDF->Markdown), LLM Agents (curador.
py, germinador.py), Markdown.

### 1\. El Problema Resuelto

Extracción de conocimiento, síntesis metodológica y detección de tendencias a
partir de grandes corpus de literatura científica en PDF (convocatorias,
grants, publicaciones), los cuales sufren de estructuras complejas (múltiples
columnas, tablas y fórmulas) que rompen los parsers de texto convencionales.

### 2\. El Enfoque y Metodología

• Pipeline de Digestión de Documentos: Uso de Docling para transformar PDFs
estructurados a Markdown limpio (pdf\_a\_md\_docling.py), conservando
encabezados, listas y tablas.
• Arquitectura de Agentes Multi-Etapa: Scripts especializados (curador.py,
germinador.py) para filtrar, resumir e integrar evidencia técnica y evaluar
hipótesis complejas (ej. hipótesis bijuntivas en tendencias de investigación).

### 3\. La Decisión Técnica No Obvia (Crítica)

• Parsing Estructurado con Docling (Layout-Aware) vs. OCR Tradicional / PyPDF
Text Extraction:
• Alternativa descartada: Extraer texto mediante PyPDF2 / pdfminer o
aplicar OCR ciego.
• Razonamiento técnico: Los parsers simples destruyen el flujo de lectura
en artículos científicos de dos columnas y convierten las tablas en texto
incomprensible. Docling comprende la maquetación visual del documento,
permitiendo generar Markdown listo para consumo directo de modelos de
lenguaje (LLMs) sin alucinaciones por desorden contextual.



### 4\. Métricas y Resultados Cuantitativos

• Volumen procesado: Conversión e integración idempotente de corpus de
artículos científicos en carpetas Grants\_md.
• Rendimiento: Idempotencia verificada por timestamps de archivos, reduciendo
el tiempo de procesamiento repetitivo a 0 segundos en documentos no
modificados.

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 4
• Estado de código público: Público (Código de integración y pipeline de
procesamiento documental 100% abierto).
──────

## 12\. Proyecto: syllabus\_code (Desarrollo y Validacion de Algoritmos ML

From-Scratch)

• Ruta local: /home/japt/syllabus\_code
• Categoría: Machine Learning / Estadística Aplicada
• Stack Principal: Python 3.12, numpy 2.4, pandas 3.0, scikit-learn 1.9,
spacy 3.8, nltk 3.9, matplotlib.

### 1\. El Problema Resuelto

Necesidad de demostrar y validar pedagógicamente la corrección matemática de
modelos de Machine Learning (Regresión Logística, Random Forest, Naïve Bayes)
implementados completamente from-scratch (usando únicamente operaciones
matriciales con NumPy) versus las implementaciones optimizadas de Scikit-
Learn, aplicados a la predicción de asignación de evaluadores y convocatorias
públicas.

### 2\. El Enfoque y Metodología

• Desarrollo Algorítmico desde Cero: Implementación manual de la función de
costo de entropía cruzada, descenso de gradiente, ganancia de información
(Information Gain) para árboles de decisión y ensamble por votación
(Hard/Soft Voting).
• Benchmarking \& Validaciones: Suite de pruebas (REPORTE\_VALIDACION.md)
comparando métricas de exactitud, curvas ROC-AUC y matrices de confusión
entre el código artesanal y scikit-learn.

### 3\. La Decisión Técnica No Obvia (Crítica)

• Corrección de Cálculo de Ganancia de Información en Splits Asimétricos
(Vectorizado NumPy):
• Alternativa descartada: Calcular tamaños de nodos utilizando longitud
de arreglos booleanos (len(bool\_array)).
• Razonamiento técnico: En la versión inicial de
random\_forest\_fromscratch.py, el uso de len(bool\_array) devolvía el
número total de observaciones N en lugar de las verdaderas sumas
booleanas np.sum(bool\_array) para las ramas izquierda y derecha. Esto
distorsionaba la ganancia de información en divisiones asimétricas,
colapsando el modelo a predecir siempre la clase mayoritaria (exactitud
57.5% vs 90.0% de sklearn). La corrección vectorizada alineó la exactitud
a 88.5% (diferencia de solo 1.5%).



### 4\. Métricas y Resultados Cuantitativos

• Exactitud comparativa Regresión Logística: From-scratch 0.5833 vs Scikit-
Learn 0.5833 (diferencia exacta de 0.0000).
• Exactitud comparativa Random Forest: From-scratch 0.8850 vs Scikit-Learn 0.
9000 (OOB Score: 0.9000 vs 0.8938).
• Pipeline NLP (demo\_preprocesamiento.py): Reducción de 102 tokens a 47 lemas
limpios mediante spacy y remoción de stopwords.

### 5\. Candidato a Estudio de Caso en Portafolio

• Nivel de relevancia (1-5): 5
• Estado de código público: Público (Excelente material demostrativo de
dominio de fundamentos teóricos y matemáticos de Machine Learning para el
portafolio).
──────

## 📈 Resumen Ejecutivo para la Síntesis del CV / Portafolio

Proyecto     │ Categoría Do… │ Contribución… │ Métricas / I… │ Relevancia …
──────────────┼───────────────┼───────────────┼───────────────┼──────────────
sp           │ Data Eng /    │ Pipeline ETL, │ 138 proyectos │ 5/5
│ OCR           │ reconciliació │ P1 filtrados; │
│               │ n de 760      │ fix anomalías │
│               │ proyectos y   │ 100x.         │
│               │ OCR sobre 363 │               │
│               │ PDFs.         │               │
tablero-     │ Optimización  │ Mecanismo     │ 3,631         │ 5/5
cbf2026      │ / Web         │ APEES de      │ propuestas    │
│               │ prelación     │ procesadas;   │
│               │ determinista  │ $336.5M       │
│               │ con franja de │ asignados.    │
│               │ indiferencia  │               │
│               │ δ.            │               │
formalizacio │ ML /          │ Inferencia    │ N = 484       │ 5/5
n-cbf-2026   │ Estadística   │ Beta-Binomial │ proyectados;  │
│               │ Jerárquica y  │ estimación de │
│               │ aditividad    │ colas TUAF.   │
│               │ estocástica   │               │
│               │ tipo Ley de   │               │
│               │ Hess.         │               │
apees\_articl │ Publicación / │ Artículo      │ 5,078         │ 5/5
e            │ Teórica       │ científico    │ propuestas    │
│               │ para OUP      │ analizadas;   │
│               │ sobre equidad │ 148 palabras  │
│               │ acotada y     │ abstract.     │
│               │ condiciones   │               │
│               │ de Arrow.     │               │
syllabus\_cod │ Machine       │ Algoritmos de │ Diferencia <  │ 5/5
e            │ Learning      │ ML from-      │ 1.5% vs       │
│               │ scratch       │ Scikit-Learn; │
│               │ (Random       │ OOB 0.90.     │
│               │ Forest,       │               │
│               │ LogReg)       │               │
│               │ vectorizados. │               │
fusionador   │ Software      │ App GUI       │ Exe 25MB      │ 4/5
│ Desktop       │ Windows       │ standalone;   │
│               │ (.exe) multi- │ 100% sin      │
│               │ hilo para     │ desalineación │
│               │ fusión e      │ .             │
│               │ indización    │               │
│               │ dúplex de     │               │
│               │ PDFs.         │               │
prelacion\_20 │ Estadística   │ Evaluación    │ 515,646       │ 4/5
26           │ Aplicada      │ empírica de   │ evaluaciones  │
│               │ agregadores   │ analizadas;   │
│               │ (mediana vs   │ 226 en        │
│               │ media) y      │ franja.       │
│               │ franja        │               │
│               │ crítica.      │               │
rxp          │ Data Eng / QA │ Algoritmo de  │ Matcheo       │ 4/5
│               │ filtrado      │ histórico     │
│               │ relacional de │ pases RCEEA   │
│               │ Conflictos de │ 1999–2026.    │
│               │ Interés (COI) │               │
│               │ en CVUs.      │               │
semillero    │ NLP \& GenAI   │ Extracción    │ Deduplicación │ 4/5
│               │ estructurada  │ e             │
│               │ de PDFs       │ idempotencia  │
│               │ científicos   │ por hashes.   │
│               │ con Docling   │               │
│               │ para agentes  │               │
│               │ LLM.          │               │
verisat      │ Data Eng      │ Generación de │ 1 página      │ 3/5
│               │ índices PDF   │ índice        │
│               │ de 3 columnas │ garantizada   │
│               │ ajustados a   │ sin           │
│               │ canvas en     │ desbordamient │
│               │ ReportLab.    │ o.            │
sankey\_diner │ Visualización │ Diagramas     │ 100% de       │ 3/5
os           │               │ Sankey        │ partidas      │
│               │ interactivos  │ ejercidas     │
│               │ Plotly/HTML   │ trazadas.     │
│               │ para flujos   │               │
│               │ de            │               │
│               │ presupuesto.  │               │
fac\_formular │ Web App /     │ Interfaz      │ Cero          │ 3/5
io           │ Finanzas      │ offline       │ descuadres en │
│               │ HTML/JS para  │ rubros        │
│               │ captura y     │ financieros.  │
│               │ validación    │               │
│               │ presupuestal  │               │
│               │ FAC.          │               │
──────

### Recomendación de Organización en el Portafolio Profesional

1. Estudios de Caso Destacados (Proyectos 5/5):
• Mecanismos de Priorización Pública \& Algoritmo APEES: Combina tablero-
cbf2026 y apees\_article mostrando diseño de algoritmos de optimización
social, formalismo matemático y ejecución web reactiva.
• Modelado Estocástico y Inferencia Bayesian-Jerárquica: Presenta
formalizacion-cbf-2026 destacando el uso de Empirical Bayes (Beta-
Binomial) y teoría de colas aplicada a la optimización de procesos
gubernamentales.
• Auditoría Masiva de Datos \& OCR Pipeline: Presenta sp destacando las
técnicas de reconciliación de datos, parsing de rutas jerárquicas y OCR
sobre documentos oficiales.
• Machine Learning Core \& Implementación From-Scratch: Presenta
syllabus\_code para evidenciar dominio profundo de las matemáticas detrás
de los modelos predictivos (NumPy puro vs Scikit-Learn).
2. Proyectos de Ingenieria de Datos y Software (Proyectos 4/5):
• Aplicación Desktop / Fusión e Indización: Presenta fusionador como
muestra de desarrollo de software distribuible de impacto operativo
directo.
• NLP \& LLM Document Pipelines: Presenta semillero para destacar
pipelines modernos de digestión documental layout-aware (Docling) para
arquitecturas GenAI.

