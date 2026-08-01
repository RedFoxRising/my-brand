---
title: "Macro Forecasting Challenge — Banco de Occidente"
translationKey: "macro-forecast-occidente"
date: 2026-03-09
draft: false
tags: ["python", "econometría", "series-de-tiempo", "finanzas", "colombia"]
summary: "Un pipeline completo de pronóstico para un torneo macroeconómico nacional: extracción de datos de 6 fuentes oficiales, un modelo por variable y validación walk-forward de 24 meses."
---

## Descripción general

Un sistema de pronóstico macroeconómico de extremo a extremo, construido para competir en el Macro Forecasting Challenge, un torneo universitario nacional organizado por Banco de Occidente y Occieconómicas. El pipeline pronostica 10 variables financieras y macroeconómicas de Colombia y de mercados internacionales, calificadas contra los datos realizados mediante una métrica de error relativo ponderado.

**Repositorio:** [github.com/RedFoxRising/macro-forecasting-occidente](https://github.com/RedFoxRising/macro-forecasting-occidente)  
**Competencia:** Banco de Occidente × Occieconómicas — 2026

## El problema

El reto exigía pronosticar 10 variables simultáneamente, cada una proveniente de una fuente oficial distinta, con frecuencias distintas y comportamientos estadísticos distintos. Un único enfoque de modelado no iba a funcionar: un método adecuado para un índice accionario es el equivocado para la inflación, y viceversa. El problema real era construir un pipeline unificado que las manejara todas de forma limpia y siguiera siendo lo bastante rápido como para actualizarlo y volver a entregar cada mes.

## Stack tecnológico

- **Lenguaje:** Python (Google Colab)
- **Datos:** `yfinance`, `requests` (API de Datos Abiertos), archivos Excel del DANE, SUAMECA del BanRep, CSV de Investing.com
- **Modelado:** `statsmodels` (SARIMAX, AutoReg), `scikit-learn` (LinearRegression)
- **Procesamiento:** `pandas`, `numpy`
- **Visualización:** `matplotlib`

## Variables y modelos

| Variable | Peso | Modelo |
|---|---|---|
| Inflación colombiana (% mensual) | 15% | SARIMA(1,1,1)(1,1,1,12) |
| Tasa de cambio USD/COP (TRM) | 10% | Random walk + drift |
| Índice de Seguimiento a la Economía (ISE) | 10% | AR(2) |
| Tasa de los TES a 10 años | 10% | Regresión lineal vs. UST10Y |
| Índice accionario Colcap | 10% | Random walk + drift |
| S&P 500 | 10% | Random walk + drift |
| Tasa de política monetaria (BanRep) | 10% | Heurística basada en reglas |
| Tasa de desempleo nacional | 10% | SARIMA(1,1,0)(0,1,1,12) |
| Petróleo Brent | 5% | Random walk + drift |
| Oro | 5% | Random walk + drift |

![Las diez variables de la competencia tras la extracción y la alineación: series mensuales de 2018 a 2026, con la última observación disponible marcada en rojo](/images/mfc-df-master.png)

*El `df_master` unificado: diez fuentes, seis formatos, un solo índice mensual. Toca para ampliar.*

## Pipeline

**Extracción.** Seis fuentes, cada una con su propio parser. Los precios de mercado vienen de Yahoo Finance vía `yfinance`. La TRM viene de la API oficial de Datos Abiertos, la misma serie que usa la competencia para calificar las entregas. La inflación, el ISE y el desempleo vienen de hojas de cálculo del DANE; la tasa de política monetaria, del sistema SUAMECA del BanRep; la tasa de los TES, de una exportación de Investing.com.

**Unificación.** Todas las series se alinean en un único DataFrame mensual que cubre de 2018 en adelante. Las variables de mercado se remuestrean con `resample('BME').last()` para tomar el último día hábil de cada mes; las variables de flujo se indexan al primer día del mes. Equivocarse en esta convención produce, de forma silenciosa, datos desalineados que corrompen todos los modelos posteriores.

**Modelado.** Un modelo por variable, elegido por comportamiento estadístico y no por sofisticación. Cinco precios de mercado usan un random walk ajustado por drift; las dos series estacionales usan SARIMA; el ISE usa un AR(2); la tasa de los TES se regresa contra la de los bonos estadounidenses a 10 años.

**Revisión manual.** Antes de entregar, cada salida de los modelos se contrasta con el contexto cualitativo y se revisa que las unidades tengan sentido. Las dos mitades de esa capa se ganaron su lugar en la ronda uno — ver Resultados.

## Validación

Cada modelo se validó con backtesting walk-forward sobre 24 meses (abril de 2024 – marzo de 2026). Para cada mes de prueba, el modelo se reentrena usando únicamente los datos disponibles antes de ese mes: ninguna observación futura se filtra hacia la ventana de entrenamiento, cosa que una división train/test estándar sí permitiría.

| Variable | Peso | Error del modelo | Random walk | Observaciones |
|---|---|---|---|---|
| TRM | 10% | 2.33% | 2.33% | 24 |
| ISE | 10% | 338.96% | 376.41% | 21 |
| Inflación | 15% | 52.84% | 85.02% | 23 |
| TES 10Y | 10% | 3.79% | 3.79% | 24 |
| Colcap | 10% | 3.64% | 3.64% | 24 |
| S&P 500 | 10% | 2.45% | 2.45% | 24 |
| Brent | 5% | 5.97% | 5.97% | 24 |
| Oro | 5% | 3.25% | 3.25% | 24 |
| Tasa de política monetaria | 10% | 2.18% | 2.18% | 24 |
| Desempleo | 10% | 6.20% | 7.84% | 22 |

Esta tabla dice dos cosas con claridad.

Primero, el modelado solo le ganó al benchmark ingenuo en tres variables: inflación, desempleo y el ISE. Las otras siete muestran errores idénticos porque esas siete son random walks. No es un atajo; es el resultado de probar si algo más elaborado ayudaba y encontrar que no. A un horizonte de un mes en mercados líquidos, superar a un random walk es genuinamente difícil.

Segundo, las cifras del ISE y la inflación son artefactos de la métrica, no fallas del modelo — ver el Reto 1 más abajo.

![Gráfico de barras que compara el error relativo medio del modelo asignado contra el de un random walk puro, para cada una de las diez variables](/images/mfc-backtest-resumen.png)

*Modelo asignado vs. benchmark de random walk. Las barras son idénticas donde el modelo asignado es un random walk. Toca para ampliar.*

![Diez paneles que muestran los valores realizados frente a los pronósticos del modelo y del random walk a lo largo de la ventana walk-forward de 24 meses](/images/mfc-backtest-series.png)

*Realizado vs. pronosticado, mes a mes, a lo largo de toda la ventana walk-forward. Toca para ampliar.*

## Retos

**Reto 1 — Métrica de error inestable cerca de cero.**
La competencia califica con `|P−O|/O × 100`, que explota cuando el valor observado se acerca a cero. La inflación mensual ronda el 0.5%, y la variación anual del ISE estuvo cerca de 0.13% en un mes; una desviación absoluta de 0.5 pp se convierte en un error relativo de 380%. En vez de asumir que los modelos habían fallado, verifiqué las unidades en el DataFrame maestro y confirmé que el problema era la métrica misma. El arreglo usa `max(|O|, ε)` como denominador, con `ε` fijado en el percentil 25 de los valores históricos absolutos de cada variable — estabilizando la métrica sin tocar las observaciones normales.

**Reto 2 — Outliers en la serie de los TES.**
La exportación de Investing.com contenía un error de digitación que producía una tasa por encima del 20%, históricamente imposible para Colombia. Se detecta contra un umbral de plausibilidad y se reemplaza con una interpolación lineal entre observaciones adyacentes.

**Reto 3 — Extracción del ISE desde una hoja de cálculo anidada.**
El archivo del ISE del DANE empaqueta varias tablas de indicadores en una sola hoja, con columnas de año que llevan etiquetas de dato provisional como `2024p`. El extractor ubica la fila del ISE total por nombre del indicador y detecta las columnas de año probando si los primeros cuatro caracteres son dígitos, lo que sobrevive a los cambios periódicos de formato del DANE.

## Resultados — Ronda 1

### La capa manual funcionó dos veces

En la TRM, el juicio le ganó al modelo. El nivel de reservas y la trayectoria de la deuda apuntaban a un peso más débil de lo que esperaba el consenso, así que la entrega se hizo en 3,690 en vez de los 3,762 del modelo.

| Pronóstico de TRM | Valor | Error vs. 3,670 |
|---|---|---|
| Salida del modelo (random walk + drift) | 3,762 | 2.51% |
| Consenso de la competencia | 3,757 | 2.37% |
| Entregado (ajuste manual) | 3,690 | 0.55% |

Fue el pronóstico de TRM más preciso de la ronda.

En el Colcap, la revisión detectó un error de unidades. El pipeline estaba trayendo `ICOLCAP.CL` de Yahoo Finance — el ETF, no el índice —, lo que producía un pronóstico con un orden de magnitud de diferencia frente a la escala contra la que califica la competencia. Nada falló; el número simplemente fluyó por el pipeline luciendo plausible. Se detectó en la revisión manual y en su lugar se entregó el valor equivalente del índice. Un ticker equivocado que no lanza ninguna excepción es el tipo de bug que sobrevive a cualquier chequeo automático.

### Tres formas en que los modelos fallaron

Las variables que salieron mal salieron mal por tres razones distintas, y la distinción importa más que los errores mismos.

**Decisiones discretas.** La heurística proyectó la tasa de política monetaria sin cambios en 10.25%; el BanRep la subió a 11.25%. Error realizado de 8.89%, contra un error del consenso de 6.67% — y aproximadamente cuatro veces lo que 24 meses de validación walk-forward sugerían para esa variable. Ninguna serie de tiempo contiene una decisión de tasas que todavía no ha ocurrido.

**Choques exógenos.** El Brent se pronosticó en 94.20 y cerró en 118. La guerra en Irán no está en la serie histórica, y ninguna cantidad de backtesting la habría anticipado.

**Drift en un régimen volátil.** El oro estaba cerca de 5,000 al momento de la entrega. Esperábamos que el rally continuara pero se desacelerara, y pronosticamos 5,314.70; el mes se revirtió y cerró en 4,648. El término de drift extrapola la tendencia reciente, lo que en un rally significa apostar a que la tendencia se mantiene. El random walk puro, anclado en el nivel de partida, habría quedado más cerca. Esto no es un bug; es el supuesto del modelo haciendo exactamente lo que promete.

![Diez paneles que muestran los últimos 18 meses de historia de cada variable con el pronóstico entregado marcado como un punto rojo](/images/mfc-pronosticos.png)

*Entregas de la Ronda 1 frente a la historia reciente. Toca para ampliar.*

## Lo que aprendí

### No todas las variables son el mismo problema de pronóstico

Al entrar, traté las diez variables como una sola tarea con diez instancias: traer los datos, emparejar un modelo con el comportamiento estadístico, validar, entregar. Los resultados mostraron que caen en tres grupos que apenas tienen que ver entre sí.

Algunas son continuas y razonablemente estables. La TRM, la tasa de los TES, el S&P 500 — con estas el pipeline hizo lo que un pipeline debe hacer. Otras están guiadas por una decisión discreta o un evento externo. La tasa de política monetaria la fijan siete personas en una sala; el Brent respondió a una guerra. Ninguna cantidad de historia contiene una decisión que todavía no se ha tomado, y ajustar un modelo suave a una variable que se mueve a saltos produce un número que parece un pronóstico sin serlo. Un tercer grupo es continuo pero atravesaba un régimen volátil. El oro estaba a mitad de rally, y el término de drift lee la tendencia reciente como información sobre el futuro, lo cual es una apuesta a que la tendencia se mantiene. Cuando el mes se revirtió, fue el drift lo que nos alejó de la respuesta más de lo que lo habría hecho un random walk ingenuo.

La consecuencia práctica es que estas no deberían compartir un método. El pronóstico estadístico es la herramienta correcta para el primer grupo. El segundo necesita escenarios ponderados y una postura explícita sobre lo que es probable que haga cada actor. Tratarlos de forma idéntica es como se termina reportando un pronóstico de tasa de política monetaria con la misma confianza implícita que un índice accionario.

### Lo que un backtest no puede decirte

Mi validación walk-forward dejó la tasa de política monetaria con un error medio de 2.18%, el segundo más bajo de las diez variables. El error realizado fue de 8.89%. Esa brecha es lo más útil que me llevo de este proyecto.

La ventana de validación fue de abril de 2024 a marzo de 2026. En esos 24 meses, el BanRep se movió de forma gradual y predecible, así que la heurística lo siguió bien. El backtest no estaba equivocado; estaba respondiendo a una pregunta más estrecha de lo que yo creía. Un error medio bajo puede significar que el modelo es bueno, o puede significar que la ventana no contenía nada difícil — y la salida se ve idéntica en ambos casos.

Esa distinción importa más allá de esta competencia. La validación te dice cómo se desempeñó un modelo contra el pasado que se le mostró, no cómo se desempeñará contra un futuro que difiera estructuralmente de ese pasado. Ahora pienso que la pregunta útil no es "cuál fue el error medio" sino "qué fue lo más difícil dentro de la ventana, y lo manejó el modelo". Si la respuesta es que no pasó nada difícil, la cifra de error está describiendo la muestra, no el modelo.

### La mejor decisión y la peor vinieron del mismo lugar

En la TRM no tomamos la salida del modelo. El nivel de reservas y la trayectoria de la deuda sugerían que el peso estaba bajo más presión de la que el consenso estaba incorporando, así que ajustamos la entrega a la baja. Fue el pronóstico de TRM más preciso de la ronda, y aproximadamente cuatro veces mejor que lo que habría producido el modelo por sí solo.

En la tasa de política monetaria dejamos correr la heurística sin tocarla.

Lo que me incomoda es que aplicaba el mismo razonamiento. Un escenario de estrés cambiario y fiscal es precisamente un escenario en el que un banco central endurece su política. Teníamos el análisis, y ya habíamos decidido que era lo bastante sólido como para pasar por encima de un modelo. Simplemente no lo trasladamos a la variable donde más importaba — posiblemente porque pronosticar una tasa de cambio se sentía como algo sobre lo que yo podía tener una postura, y pronosticar la decisión de un banco central no.

La lección no es que los modelos fallen. Es que el juicio discrecional hay que aplicarlo a todo el conjunto o a nada. Aplicarlo solo donde te sientes calificado para opinar significa que tus errores se concentran justo donde estuviste menos dispuesto a pensar.

La ronda uno además contuvo tres choques exógenos en un solo mes: el conflicto en Irán, la escalada entre el ejecutivo y el banco central, y una reversión en los metales. Eso explica el tamaño de las desviaciones. No explica la que importaba, que fue una falla de proceso y no de suerte.

### Parsear datos oficiales del mundo real

Las fuentes gubernamentales no son limpias. El archivo de inflación del DANE usa una disposición ancha (años como columnas, meses como filas) que hay que transformar a formato largo antes de poder usarla. La exportación del BanRep trae logos y notas al pie encima de los datos. Cada fuente necesitó su propia función de extracción con lógica específica para su formato — y cada hora invertida en hacer una robusta se pagó sola en todos los meses siguientes.

### Backtesting honesto, y sus límites

La validación walk-forward es la forma correcta de probar un modelo de series de tiempo, e hizo su trabajo: mostró qué modelos eran genuinamente predictivos y cuáles solo se veían bien dentro de la muestra. Pero un backtest solo puede probar contra lo que ocurrió dentro de su ventana. Esa distinción terminó importando más que la metodología misma.

### Cuando lo simple le gana a lo complejo

Para las cinco variables de precios de mercado, un random walk ajustado por drift igualó a todo lo más sofisticado que probé. Es el resultado esperado para mercados eficientes a un horizonte de un mes, y un recordatorio de que complejidad no es calidad.

## Mejoras futuras

* Agregar el índice de precios de alimentos SIPSA como regresor externo para la inflación (SARIMAX)
* Agregar datos de consumo de energía de XM como proxy del ISE
* Construir un módulo de escenarios (bajista / base / alcista) con ponderación por probabilidad
* Separar el conjunto de variables de forma explícita: pronóstico estadístico para las series continuas, escenarios ponderados para todo lo guiado por una decisión discreta
* Agregar aserciones automáticas de unidades y escala para que los tickers equivocados fallen ruidosamente
* Automatizar la actualización de datos para que el pipeline corra sin descargas manuales
