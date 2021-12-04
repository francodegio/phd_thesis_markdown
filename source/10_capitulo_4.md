# Especificación y estimación de modelos no paramétricos
## Introducción

Tal como se describió previamente, en esta sección se procederá a la implementación de los algoritmos de _Random Forest_ y _Gradient Boosting_, la optimización de sus parámetros utilizando validación cruzada y su poder de generalización con un set de testeo. En el caso del primer algoritmo, se procederá al ajuste del modelo utilizando la librería _Scikit-Learn_[^14] de python debido a su simplicidad de uso, así como también a su popularidad. En cuanto al modelo de Gradient Boosting, se procederá a su ajuste a través de la librería _CatBoost_[^15] ya que la misma no requiere de optimización de hiper-parámetros y es una de las versiones más eficientes en términos computacionales para el ajuste del modelo.
Tanto el algoritmo de RandomForest como CatBoost nos permiten crear subdivisiones en el espacio de las características, generando así, interacciones no lineales entre las mismas. Esto nos permite arrojar por la borda toda formalidad de especificación del problema, ya que no se plantea la existencia de una forma funcional que exprese la relación de las variables, sino que simplemente se intenta predecir valores de la variable objetivo en base  a los datos observados. Es por esta razón que es extremadamente importante dividir el dataset de las observaciones en al menos dos grupos: el conjunto de entrenamiento y el conjunto de validación. De esta forma podemos comprobar que el modelo generaliza bien a datos no observados.

[^14]: @scikit-learn
[^15]: @catboost2018

## Métodos de conjuntos secuenciales
### CatBoost sin variables autocorrelacionadas

Para obtener los mejores resultados se realizó una búsqueda de hiper-parámetros donde el espacio de posibles valores estuvo dado por:
- Máxima profundidad de los árboles : 2, 4, 10 ó sin límite.
- Tasa de aprendizaje: 0.001, 0.01, 0.1 ó 1.
- Cantidad de estimadores: 50, 200, 500 ó 1000.

Luego de ajustar los tres pliegues de las 64 combinaciones posibles, la mejor combinación de  hiper-parámetros resultó ser: máxima profundidad de 4 hojas, tasa de aprendizaje de 0.1 y 500 estimadores.
Con esta configuración se obtuvo un $R^2$ de $0.977$ en entrenamiento y $R^2$ de $0.909$ en el conjunto de testeo. En este caso se pudo observar un leve sobreajuste (elevada varianza) del modelo a los datos que podría disminuirse utilizando menos estimadores o cambiando el tipo de regularización empleada.

![Importancia relativa de las características según CatBoost, sin Lag X.](source/figures/Grafico_18.png "Gráfico 17"){#fig:gf17 width=70%}

A pesar de que el modelo no establece una relación lineal entre las variables, sí permite hacer una apreciación de las features a partir de la cantidad la mejora marginal que se obtiene de la varianza explicada cuando se realiza una partición en el espacio de dicha variable exógena. En efecto, retorna un ranking en función de esa información aportada. En este caso, se observó (ver Gráfico @fig:gf17) que las variables que mayor información aportan son las relacionadas a la mensura de la propiedad ($b-ratio$, $tsqm2$, $bsqm$, $tsqm$, $bsqm2$ y $bed_bath_ratio$) así como también las variables de localización ($lat$ y $lon$).

![Residuos de CatBoost, sin Lag X.](source/figures/Grafico_19.png "Gráfico 18"){#fig:gf18 width=70%}

### CatBoost con variables autocorrelacionadas
Para evaluar si la creación de la matriz de adyacencia y la generación de las características ponderadas de los vecinos aportan valor en este tipo de modelos, se ajustó una nueva instancia del modelo donde lo único que cambió fue la inclusión de las variables _lag_.
Nuevamente, se repitió el espacio multidimensional de hiper-parámetros para encontrar el mejor ajuste posible. En esta ocasión, se entrenaron árboles de decisión de profundidad máxima de 4 hojas, tasa de aprendizaje de 0.01 y 1000 estimadores. Con esta configuración se obtuvo un $R^2$ en entrenamiento de 0.928 y un $R^2$ en el conjunto de testeo de 0.893, lo que implicó una leve caída respecto del modelo anterior, con menor sobreajuste al mismo tiempo, pero que queda dentro de la variabilidad propia del algoritmo, que es no determinístico[^16].

[^16]: CatBoost FAQ, CatBoost, https://catboost.ai/docs/concepts/faq.html, (consultada el 15 de septiembre de 2020)

![Importancia relativa de las características según CatBoost, con Lag X.](source/figures/Grafico_20.png "Gráfico 19"){#fig:gf19 width=70%}

Respecto de la importancia relativa de las variables exógenas, se observó (ver Gráfico @fig:gf19) una historia muy similar al modelo anterior, donde las mismas variables se encuentran entre las principales, excepto por el orden de importancia, lo cual sugiere que comparten algún grado de correlación entre las mismas. Respecto de las variables nuevas que se incluyeron en este modelo, las únicas que aparentaron ser relevantes fueron $lag\_b\_ratio$, $lag\_bed\_bath\_ratio$ y $lag\_tsqm2$, pero este tipo de resultados puede ser explicado simplemente por la correlación existente entre las variables originales y las autocorrelacionadas.

![Residuos de CatBoost, con Lag X.](source/figures/Grafico_21.png "Gráfico 20"){#fig:gf20 width=70%}

## Métodos de conjuntos simultáneos
### Random Forest sin variables autocorrelacionadas

Luego de separar aleatoriamente el conjunto de entrenamiento y el de validación, se procedió a ajustar el algoritmo de RandomForest bajo un esquema de búsqueda de optimización de hiper-parámetros con validación cruzada de tres pliegues donde se obtuvieron como mejores resultados, los siguientes valores para el algoritmo:
Profundidad máxima de cada árbol: sin límite.
- Mínimo de muestras por hoja: 1.
- Mínimo de muestras para realizar partición: 2.
- Cantidad de estimadores: 50.

Al mismo tiempo, se buscó minimizar los errores cuadráticos como criterio de aprendizaje. Los resultados obtenidos fueron un $R^2$ de 0.985 en el conjunto de entrenamiento, 0.894 promedio de $R^2$  en validación cruzada y 0.903 en el conjunto de datos de testeo.

![Importancia relativa de las características según RandomForest, sin Lag X.](source/figures/Grafico_22.png "Gráfico 21"){#fig:gf21 width=70%}

Debido a que tanto CatBoost como RandomForest están construidos a partir de árboles de regresión y clasificación, este último también computa la importancia relativa de las variables exógenas en función de la varianza explicada. En base a ese criterio, las características más relevantes para el modelo fueron sólo 5 variables: el ratio de metros construidos y metros totales ($b\_ratio$), el tamaño del terreno y su transformación cuadrática (_tsqm_ y _tsqm2_) y la latitud (_lat_) y longitud (_lon_).

![Residuos de RandomForest, sin Lag X.](source/figures/Grafico_23.png "Gráfico 22"){#fig:gf22 width=70%}

### Random Forest con variables autocorrelacionadas
A efectos de comprobar la misma hipótesis observada con el algoritmo de regresión de CatBoost, se optó por ajustar un modelo con las variables lag.Se procedió a repetir el espacio de posibilidades de hiper-parámetros y se obtuvieron los mismos resultados, excepto la cantidad total de estimadores que pasó de 50 a 500. Esto puede deberse a que al incrementar la cantidad de descriptores, se requiera de mayor varianza de los árboles para poder aportar más información.

![Importancia relativa de las características según RandomForest, con Lag X.](source/figures/Grafico_23.png "Gráfico 22"){#fig:gf22 width=70%}

En cuanto a la performance del modelo, se observó un claro ejemplo de sobreajuste a los datos ya que se obtuvo un $R^2$ de entrenamiento de 0.981, un $R^2$ de validación cruzada de 0.857 y un $R^2$ en el set de testeo de 0.876. Los principales motivos para estos resultados yacen en el hecho de que se duplicó la cantidad de features al modelo y se aumentó considerablemente la cantidad de estimadores. Además, las variables lag  que se agregaron están fuertemente correlacionadas con las originales, lo cual provoca aumento de la varianza de los estimadores.
Finalmente, se pudo observar que, en cuanto a la importancia relativa de las variables exógenas, no cambió mucho el panorama, sólo que se incluyeron entre las principales algunas de las variables lag, tales como: $lag\_b\_ratio$ y $lag\_tsqm$. De todas formas no son efectos necesariamente relevantes debido a la alta correlación que existe entre las variables lag y las originales.

![Residuos de RandomForest, con Lag X.](source/figures/Grafico_24.png "Gráfico 23"){#fig:gf23 width=70%}