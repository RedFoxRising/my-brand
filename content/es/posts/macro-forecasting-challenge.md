---
title: "Pronosticar el futuro: mi estrategia para el Macro Challenge de Banco de Occidente"
translationKey: "macro-forecasting-challenge"
date: 2026-02-21
draft: false
hiddenInHomeList: true
summary: "Cómo construí un pipeline completo de pronóstico macroeconómico — desde los datos crudos de la API hasta el backtesting walk-forward — para competir en un torneo universitario nacional."
tags: ["Econometría", "Pronósticos", "Análisis de Datos", "Python", "Finanzas", "Colombia"]
---

## El reto

Actualmente estoy compitiendo en el **Macro Forecasting Challenge**, un torneo nacional organizado por Banco de Occidente y su división de investigación, Occieconómicas. El objetivo es sencillo en concepto pero exigente en la práctica: pronosticar 10 variables macroeconómicas y financieras clave para Colombia y los mercados internacionales, y luego comparar las predicciones contra los datos realizados.

La competencia se desarrolla en tres rondas de entrega — marzo, abril y mayo — y el equipo ganador se determina por el menor error relativo ponderado promedio en las tres. El premio del primer lugar es una práctica de verano de 6 semanas en la Mesa de Dinero (Trading Desk) del banco en Bogotá.

### Las 10 variables

Las variables tienen pesos distintos en la fórmula de puntuación, lo que definió directamente cómo prioricé mi esfuerzo de modelado:

| Variable | Peso | Tipo |
|---|---|---|
| Inflación colombiana (mensual) | **15%** | Macro local |
| Tasa de cambio USD/COP (TRM) | 10% | Divisas |
| Índice de Seguimiento a la Economía (ISE) | 10% | Macro local |
| Tasa de los TES a 10 años (2036) | 10% | Renta fija |
| Índice accionario Colcap | 10% | Renta variable |
| S&P 500 | 10% | Renta variable |
| Tasa de política monetaria | 10% | Macro local |
| Tasa de desempleo nacional | 10% | Macro local |
| Petróleo Brent | 5% | Materias primas |
| Oro | 5% | Materias primas |

La fórmula de puntuación es: `Error = |Forecast − Observed| / Observed × 100%`, promediada entre variables y ponderada por la importancia asignada a cada una.

---

## Parte 1: Construir el pipeline de datos

El primer reto fue construir desde cero un conjunto de datos limpio y unificado. Cada variable vive en una fuente distinta, con un formato, una frecuencia y un nivel de desorden distintos. Construí funciones de extracción para cada una.

**Las variables de mercados internacionales** (S&P 500, Brent, Oro, Colcap) vinieron de Yahoo Finance vía `yfinance`, la fuente más limpia del pipeline. Extraje los precios de cierre diarios desde 2018 en adelante para capturar tanto el ciclo prepandemia como la normalización posterior a 2020.

**La TRM** (tasa de cambio USD/COP) se descargó de la API oficial de datos abiertos de Colombia (`datos.gov.co`), que entrega la tasa diaria certificada por la Superintendencia Financiera — la misma fuente que usa la competencia para los valores observados.

**Los datos del Banco de la República** (tasa de política monetaria) exigieron parsear un archivo Excel no estándar de su portal SUAMECA, con logos y notas al pie en las filas de encabezado. La función de extracción maneja esto automáticamente con `skiprows` y una pasada de validación de fechas para descartar las filas que no lo son.

**Los datos del DANE** (inflación, ISE, desempleo) presentaron los retos de parseo más complejos. La tabla de inflación usa un formato "ancho" donde los años son columnas y los meses son filas — la función la transforma a una serie de tiempo en formato largo estándar. El archivo del ISE tiene varias tablas incrustadas en una sola hoja, así que el extractor busca la fila específica que contiene el ISE total haciendo pattern-matching sobre el nombre del indicador. Los datos de desempleo (GEIH) traen asteriscos de la época de la pandemia y tipos de datos mezclados, que requirieron una pasada de limpieza robusta antes de que la serie fuera utilizable.

**Las tasas de los TES a 10 años** vinieron de Investing.com como un CSV descargado manualmente. Los datos crudos contenían un outlier (probablemente un error de digitación que producía una tasa implausible) que se detectó de forma programática y se interpoló linealmente.

---

## Parte 2: Unificar en `df_master`

Con 10 series limpias en mano, el siguiente paso fue alinearlas en un único DataFrame mensual: `df_master`. Esto es menos trivial de lo que suena, porque las fuentes operan a frecuencias distintas.

Mi solución: para las series diarias (TRM, S&P 500, etc.), uso `resample('BME').last()` para extraer el valor del último día hábil de cada mes — que es exactamente lo que mide la competencia. Para las variables mensuales de flujo (inflación, ISE, desempleo), normalizo el índice al primer día de cada mes como llave de unión.

El resultado es un DataFrame de 10 columnas con un DatetimeIndex mensual desde 2018 hasta el presente, que sirve de base para todos los modelos.

---

## Parte 3: Modelos de pronóstico

Elegí deliberadamente tipos de modelo distintos para cada variable, ajustándome al comportamiento estadístico de la serie en vez de aplicar un solo método de manera uniforme.

**Random Walk con drift** para las cinco variables de precios de mercado (TRM, S&P 500, Brent, Oro, Colcap). En mercados eficientes, el precio actual contiene toda la información disponible sobre el futuro. Para un horizonte de 1 mes, un random walk ajustado por drift — donde el drift es el cambio mensual promedio de los últimos 12 meses — es un benchmark extremadamente difícil de superar de forma sistemática. Puse esa afirmación a prueba en la fase de backtesting.

**Regresión lineal contra la tasa de los bonos del Tesoro de EE. UU. a 10 años** para los TES. Las tasas soberanas colombianas se mueven de cerca con las tasas estadounidenses más un spread que varía en el tiempo. El modelo regresa la tasa histórica de los TES sobre el UST 10Y y usa la tasa estadounidense actual como predictor para el mes siguiente. Los diagnósticos del spread se muestran explícitamente para poder ajustarlos manualmente si cambia el panorama de riesgo crediticio.

**SARIMA(1,1,1)(1,1,1,12)** para la inflación. La serie mensual de inflación muestra tanto una tendencia a la baja (el ciclo de desinflación de Colombia desde 2023) como patrones estacionales fuertes: enero y diciembre suelen ser meses de inflación alta por los ajustes de tarifas de servicios públicos y la temporada de fin de año. El ARIMA estacional captura ambos componentes.

**AR(2)** para el ISE. La tasa de crecimiento anual de la actividad económica tiene una inercia alta de un mes a otro — conocer los últimos dos meses basta para obtener un pronóstico razonable a un paso. Aquí se prefiere un modelo parsimonioso porque la serie del ISE es más corta que las demás (se publica con dos meses de rezago).

**SARIMA(1,1,0)(0,1,1,12)** para el desempleo. El desempleo colombiano tiene un patrón estacional anual muy pronunciado, con picos en enero y julio. La diferenciación estacional y el término de media móvil manejan bien esto sin sobreajustar.

**Regla heurística** para la tasa de política monetaria. El BanRep anuncia las fechas de sus reuniones con bastante anticipación, y el mercado incorpora las decisiones esperadas a través de las tasas OIS. En vez de ajustar un modelo estadístico a una variable que es intrínsecamente discreta y guiada por calendario, uso la última tasa conocida y aplico un ajuste manual con base en las comunicaciones más recientes del banco central antes de cada entrega.

---

## Parte 4: Backtesting walk-forward

Antes de confiarle a cualquier modelo una entrega real, validé cada uno usando **validación walk-forward** sobre los últimos 24 meses de datos disponibles.

El principio clave: para cada mes de prueba `t`, el modelo se entrena exclusivamente con datos de `[0, t-1]`. Nunca ve el futuro. Esta es la única forma estadísticamente honesta de evaluar un modelo de pronóstico de series de tiempo — cualquier división que asigne observaciones al azar entre entrenamiento y prueba filtraría información futura hacia la ventana de entrenamiento.

Para cada variable calculé dos conjuntos de errores: el error del modelo y el error de un random walk puro usado como benchmark. La comparación responde a la pregunta: *¿mi modelo realmente aporta valor, o estoy complicando las cosas de más?*

Fue necesaria una corrección técnica: la fórmula de error de la competencia `|P-O|/O × 100` se vuelve inestable cuando el valor observado está cerca de cero. Para la inflación (~0.3%) y el ISE (que puede estar cerca de 0% durante desaceleraciones económicas), un error absoluto perfectamente razonable de 0.15 puntos porcentuales produce un error relativo que se ve catastróficamente grande. Lo abordé usando `max(|O|, ε)` como denominador, donde `ε` es el percentil 25 de los valores históricos absolutos de cada variable — esto estabiliza la métrica y preserva su comportamiento para observaciones de magnitud normal.

---

## Resultados y próximos pasos

El backtesting reveló que el Random Walk es genuinamente difícil de superar para precios de mercado a un horizonte de 1 mes — que es el resultado esperado y valida la selección de modelos. Los modelos con más espacio de mejora son los de inflación e ISE, donde incorporar regresores externos (índices de precios de alimentos de la encuesta SIPSA del DANE, datos de consumo de energía de XM) debería reducir el error absoluto medio.

El siguiente paso de mayor impacto antes de cada entrega es la **capa de ajuste manual**: revisar las minutas más recientes del BanRep, los datos semanales de precios de alimentos del SIPSA y las curvas forward del petróleo antes de cerrar cualquier número. Los modelos estadísticos aportan un ancla insesgada; la capa de juicio la calibra contra información que los modelos no pueden ver.

Actualizaré esta publicación con los errores reales de cada ronda a medida que se publiquen los datos oficiales.

---

*El pipeline completo — extracción de datos, construcción de `df_master`, modelos y backtesting — está documentado en el notebook complementario disponible en el repositorio del proyecto.*
