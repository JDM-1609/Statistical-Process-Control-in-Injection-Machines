# SPC-BASED CONTROL PLAN OPTIMIZER
**1. PROPÓSITO DEL PROGRAMA**

Este programa es una herramienta de análisis estadístico desarrollada para apoyar la aplicación sistemática del Control Estadístico de Procesos (SPC) sobre los planes de control establecidos en el   sistema ALS para máquinas de inyección de plástico.

Objetivos principales:
* Analizar datos históricos del proceso registrados en el ALS
* Evaluar estabilidad, centrado y variabilidad del proceso
* Proponer límites de especificación (LSL/USL) técnicamente coherentes
* Simular la capacidad del proceso (Cp, Cpk) bajo nuevos límites antes de su implementación en planta

Este sistema esta diseñado como soporte a la metodología desarrollada para la optimización y monitoreo de planes de control mediante SPC, no como un sistema autónomo de control.

**2. ENTRADAS DEL PROGRAMA**

* Archivo exportado del ALS en formato CSV
* Formato esperado:
  - Separador de columnas: ","
  - Separador decimal: ","
  - Valores numéricos entre comillas: "14,35"
  - Encoding: CP1250
* Estructura del CSV base:
  - Sección de información contextual (Índice 0)
  - Sección de estadisticos calculados por el ALS (Índice 2 hasta 21)
  - Sección de datos crudos por subgrupos con n=5 (Índice 23 hasta final)

**3. MÓDULOS FUNCIONALES**

* Módulo 1: visualización de datos y cartas de control mediante SPC
  - Lectura y separación del CSV en las secciones definidas
  - Corrección de desplazamientos de encabezados comunes en archivos del ALS
  - Generación de los DataFrame base para los análisis (df_info, df_stats, df_raw, df_resumen)
  - Generación de los DataFrame resultantes de los cálculos necesarios para la graficación de cartas de control (df_limpio, df_xbar, df_s, xbar_prom, s_prom)
  - Generación de las cartas de control con ventana deslizante, menú desplegable para selección de la variable e identificación de puntos fuera de especificación

* Módulo 2: optimizador automático de limites de especificación
  - Identificación automática de tramos de operación estables por variable
  - Filtrado de valores atípicos mediante:
    - Método IQR (Turkey o Rango intercuartílico)
    - Método de percentiles
  - Propuesta de nuevos límites bajo criterios estadísticos
  - Inclusión de restricciones prácticas ("Candados"):
    - Cobertura de los nuevos límites mínima sobre datos
    - Porcentaje de reducción mínimo del rango de tolerancia
    - Validación de capacidad mínima para optimizar
   
* Módulo 3: simulador de capacidad del proceso
  - Recalcula indicadores de capacidad (Cp, Cpk y desviación relativa)
  - Los límites a simular deben ser ingresador por el usuario
  - Permite evaluar escenarios alternativos sin esperar una nueva producción

**4. SALIDAS DEL PROGRAMA**

- Tablas de resultados en cada módulo
  - Estadísticos principales iniciales para formato de seguimientos
  - Límites optimizados recomendados con las validaciones respectivas
  - Estadísticos principales resultados de la simulación
- Cartas de control SPC (Individuales, X y S)

**5. CONSIDERACIONES Y SUPOSICIONES TÉCNICAS**

- Tamaño de los subgrupos fijo (n = 5)
- Los índices de df_stats (tabla de estadísticos) corresponden a los nombres estándar que vienen del ALS (Ej: Valos nominal, xqq, LISx, LIIx, etc.)
- El optimizador asume que los datos a analizar provienen de un proceso en estabilidad estadística y centrado (esto se revisa en los análisis anteriores a esto)
- El sistema no reemplaza la validación en planta. La simulación es una herramienta de apoyo a los análisis.

**6. LIMITACIONES ACTUALES**

- Interfaz orientada a usuarios técnicos (con bases en la utilización de Colab y lenguaje Python)
- No incluye registro automático de eventos anómalos vistos en el proceso
- No implementa reglas para la detección de patrones anómalos (Reglas Western Electric)

**7. POSIBLES DESARROLLOS FUTUROS**

- Interfaz gráfica para fácil uso de usuarios en planta
- Exportación de los reportes para los seguimientos (Excel o PDF)
- Integración de módulo para el registro de eventos anómalos del proceso etiquetados con su ventana de datos para almacenarlos en un dataset que permita recolectar conocimientos sobre los comportamientos comúnes del proceso
- Implementación de herramientas de Machine Learning y modelos predictivos para la detección de patrones anómalos comúnes

**8. ENTREGABLES ASOCIADOS**
- Notebook desarrollado con descripciones del programa (.ipynb)
- Script Python (.py)
- Archivos CSV de ejemplo
- Documentación metodológica para el uso del programa bajo la metodología propuesta

**EL VALOR PRINCIPAL DEL PROGRAMA NO RESIDE ÚNICAMENTE EN EL CÓDIGO, SINO EN LA METODOLOGÍA DE ANÁLISIS Y OPTIMIZACIÓN QUE ACOMPAÑA.** 

**Esta herramienta fue diseñada para ser utilizada y extendida en su desarrollo por la empresa SIMEX SAS según las necesidades propias de la empresa**












