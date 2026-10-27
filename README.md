# Análisis de uso de llamadas y mensajes – ConnectaTel 

Objetivo
Evaluar el comportamiento de los clientes de ConnectaTel, una empresa de telecomunicaciones en Latinoamérica, con datos registrados hasta 2024. Se exploran, limpian y analizan los datos para construir un perfil de los clientes, detectar comportamientos atípicos, crear segmentos y proponer mejoras en los planes y en la retención.

Datasets utilizados

-plans.csv (2 filas): planes actuales, con precio, minutos y GB incluidos, y costo por extra.
-users_latam.csv (4,000 filas): edad, ciudad, fecha de registro, plan y fecha de baja.
-usage.csv (40,000 filas): llamadas y mensajes de los usuarios.

Etapas del análisis

1. Carga y exploración
Se cargaron los tres archivos y se revisaron con .head(), .shape e .info().

2. Problemas de calidad de datos

city: 469 nulos (11.7%) y 96 con el valor “?”, para un total de 565 faltantes (14.1%).
churn_date: 3,534 nulos (88.4%). No es un error: significa usuario activo.
age: contenía el valor -999, que distorsionaba la media (33.7) y la desviación estándar (123).
reg_date: 40 registros con año 2026, imposible porque los datos llegan hasta 2024.
usage.date: 50 nulos (0.1%).
duration y length: 55.2% y 44.7% de nulos. Dependen del tipo de uso (duración solo en llamadas, longitud solo en mensajes), así que son nulos estructurales.

3. Limpieza

age: el -999 se reemplazó por la mediana (48). Quedó entre 18 y 79 años.
city: el “?” se convirtió en nulo (565 nulos).
reg_date: los 40 registros de 2026 se marcaron como nulos.
duration y length: se dejaron como nulos.

4. Resumen de uso por usuario
Se sumaron los mensajes, las llamadas y los minutos de cada usuario y se unieron con la tabla de usuarios. En promedio, cada usuario tiene 5.5 mensajes, 4.5 llamadas y 23.3 minutos de llamada. El 64.9% de los usuarios está en Basico y el 35.1% en Premium.

5. Distribuciones y outliers
Los mensajes, las llamadas y los minutos tienen distribución sesgada a la derecha, y la edad es casi uniforme. No hay diferencias entre planes. Solo hay outliers en el extremo alto:

Mensajes: 46 usuarios (1.1%), con límite de 11.5.
Llamadas: 30 usuarios (0.8%), con límite de 10.5.
Minutos: 109 usuarios (2.7%), con límite de 61.9.

Se conservaron, porque son usuarios intensivos reales y no errores.

6. Segmentación

Por uso: Uso medio 2,943 (≈74%), Bajo uso 779 (≈19.5%) y Alto uso 278 (≈7%).
Por edad: Adulto 2,018 (≈50%), Adulto Mayor 1,222 (≈30%) y Joven 760 (≈19%).

7. Insight ejecutivo
Basico y Premium tienen un patrón de uso casi idéntico (Alto uso: 7.1% en Basico y 6.6% en Premium). Es decir, el plan no distingue el nivel de uso.

Recomendaciones

Revisar la propuesta de valor de Premium, porque su uso es igual al de Basico.
Crear un plan para Alto uso (≈7%), con más minutos o llamadas a un precio mayor.
Crear una tarifa de entrada para Bajo uso (≈19.5%), para reducir el riesgo de baja.
Analizar a los 466 usuarios que ya se dieron de baja y actuar sobre el segmento de Bajo uso.
Corregir en origen la captura de age, city y reg_date.

Cómo ejecutar el notebook
Abre el notebook en Google Colab y ejecuta las celdas en orden, de arriba hacia abajo. El notebook lee los datos desde /datasets/, así que los archivos deben estar en esa ruta o hay que cambiarla en la celda de carga. Usa pandas, numpy, seaborn y matplotlib.
