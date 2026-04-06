# Modelado N cuerpos, Mecánica Celeste
**Soleil Niño, Daniel Soto, Sara Calle**


Repositorio de trabajo para el **modelado gravitacional de N‑cuerpos** aplicado a la **dinámica estelar del Centro Galáctico**, con énfasis en el **cúmulo de estrellas S** alrededor del agujero negro supermasivo **Sagittarius A\*** (Sgr A\*).

Este proyecto integra (i) **teoría de mecánica celeste**, (ii) **datos observacionales (catálogos orbitales)** y (iii) **simulación numérica** (integración de EDOs y N‑cuerpos) para responder preguntas sobre la evolución orbital y el rol de perturbaciones estrella–estrella en un potencial dominado por un objeto central masivo.

---

## Contenido del repositorio (qué hay y dónde)
- **Base de datos / catálogo orbital (entrada principal):**
  - `Sgrestrellas.tsv` (39 estrellas; elementos orbitales y metadatos).  
    Archivo: https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/Sgrestrellas.tsv

- **Notebook principal de dinámica del cúmulo S (simulación y análisis):**
  - `modelar-sistema-de-estrellas/S_Stars_Dynamics_V2.ipynb`  
    Archivo: https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/modelar-sistema-de-estrellas/S_Stars_Dynamics_V2.ipynb  
    (Aquí se formulan preguntas de investigación, se implementa la integración y se generan visualizaciones/diagnósticos como conservación de energía y trayectorias 3D.)

- **Reporte en LaTeX (documenta resultados, estadística del catálogo y herramientas):**
  - `modelar-sistema-de-estrellas/reporte_estrellas_S.tex`  
    Archivo: https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/modelar-sistema-de-estrellas/reporte_estrellas_S.tex

- **Material de apoyo / visualización:**
  - `Notas/TutorialPlotly.ipynb` (guía de Plotly para gráficas 2D/3D/animaciones)  
    Archivo: https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/Notas/TutorialPlotly.ipynb

- **Problemset (EDOs e integración numérica, base metodológica):**
  - `Problemset/ParteComputacional.ipynb` (incluye implementación/uso de **Leapfrog**)  
    Archivo: https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/Problemset/ParteComputacional.ipynb  
  - `Problemset/SolucionProblemset.tex` (desarrollo analítico)  
    Archivo: https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/Problemset/SolucionProblemset.tex

---

## Contexto teórico (qué se modela y por qué)
### 1) Mecánica celeste y el problema gravitacional
El movimiento de un sistema de cuerpos bajo gravedad newtoniana se describe (idealmente) por el **problema de N‑cuerpos**, donde cada partícula de masa \(m_i\) experimenta la aceleración debida a todas las demás:

r¨_i = -G * Σ_{j≠i} m_j (r_i - r_j) / ||r_i - r_j||^3


Este sistema es **no lineal** y, para \(N\ge 3\), generalmente **no tiene solución cerrada**; por tanto, se estudia por:
- aproximaciones (por ejemplo, **dos cuerpos + perturbaciones**), o
- **integración numérica** directa de las ecuaciones.

En el Centro Galáctico, muchas estrellas del cúmulo S orbitan a Sgr A\*, por lo que una primera aproximación física natural es:
- **Potencial central dominante** (Sgr A\*) \(\Rightarrow\) dinámica cercana a **dos cuerpos**.
- **Perturbaciones** por estrellas vecinas \(\Rightarrow\) correcciones tipo **N‑cuerpos** para cuantificar interacción estrella–estrella.

### 2) Dos cuerpos, elementos orbitales y datos observacionales
En el problema de dos cuerpos, las órbitas son cónicas y pueden parametrizarse por **elementos orbitales** (semieje mayor \(a\), excentricidad \(e\), inclinación \(i\), argumento del periastro \(\omega\), nodo ascendente \(\Omega\), época de periastro, etc.).  
Los catálogos observacionales del cúmulo S reportan precisamente estos parámetros; en este proyecto se usan como condiciones iniciales (o como base para reconstruir estados cartesianos) a partir del archivo:

- `Sgrestrellas.tsv`  
  https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/Sgrestrellas.tsv

### 3) Integración numérica e integradores (por qué importa el método)
Simular órbitas durante múltiples periodos requiere integradores estables. En dinámica hamiltoniana (gravedad newtoniana), los integradores **simplécticos** (como **Leapfrog**) suelen conservar mejor invariantes (energía/momento angular) a largo plazo que métodos genéricos con el mismo costo.

Como base metodológica (EDOs), se trabajó Leapfrog en:
- `Problemset/ParteComputacional.ipynb`  
  https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/Problemset/ParteComputacional.ipynb

En la aplicación a estrellas S, el notebook principal incluye chequeos de **conservación de energía** y visualización 3D de trayectorias:
- `modelar-sistema-de-estrellas/S_Stars_Dynamics_V2.ipynb`  
  https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/modelar-sistema-de-estrellas/S_Stars_Dynamics_V2.ipynb

---

## Metodología (resumen breve)
1. **Adquisición y preparación de datos:** se toma el catálogo de estrellas S con elementos orbitales desde `Sgrestrellas.tsv`.  
   https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/Sgrestrellas.tsv

2. **Planteamiento físico del modelo:**
   - Caso base: **movimiento dominado por Sgr A\*** (modelo tipo 2‑cuerpos por estrella).
   - Caso extendido: **N‑cuerpos**, incorporando interacciones estrella–estrella para estimar su importancia relativa.

3. **Conversión de variables e inicialización:** se pasan elementos orbitales a estados dinámicos (posición/velocidad) en un marco adecuado y se definen masas/constantes. (El flujo completo está implementado en el notebook principal).

4. **Integración temporal:** se integran las EDOs del sistema usando técnicas numéricas (con énfasis en estabilidad/consistencia), produciendo trayectorias y series temporales de magnitudes dinámicas.

5. **Diagnóstico y análisis:**
   - verificación numérica (por ejemplo, **energía total** y sus componentes),
   - comparación cualitativa y cuantitativa entre escenarios,
   - visualización 2D/3D y (cuando aplica) animaciones.

Todo el pipeline de simulación y figuras principales está en:
- `modelar-sistema-de-estrellas/S_Stars_Dynamics_V3.ipynb`  
  https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/modelar-sistema-de-estrellas/S_Stars_Dynamics_V3.ipynb

---

## Fuentes

### Artículos
* [An Update on Monitoring Stellar Orbits in the Galactic Center](https://arxiv.org/abs/1611.09144)
* [Kinematic Structure of the Galactic Center S-cluster](https://arxiv.org/abs/2006.11399)
* [Sagittarius A* - The Milky Way Supermassive Black Hole](https://arxiv.org/abs/2503.20081)

### Bases de datos
* [25yrs monitoring of stellar orbits in the GC (Gillessen+, 2017)](https://vizier.cds.unistra.fr/viz-bin/VizieR?-source=J/ApJ/837/30)
* [Kinematic structure of the Galactic Center S cluster (Ali+, 2020)](https://vizier.cds.unistra.fr/viz-bin/VizieR?-source=J/ApJ/896/100)

### Paquete para mecánica celeste
* [pymcel](https://github.com/seap-udea/pymcel)

---

## Cita
Si usas este trabajo, revisa `CITATION.cff`:
https://github.com/Daniel-534/ModelamientoNCuerpos/blob/main/CITATION.cff
