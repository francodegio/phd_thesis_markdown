# Análisis Exploratorio de Datos

Con la intención de conocer los datos sobre los cuales se ajustarán los modelos, se procede a realizar un análisis en detalle de cada una de las dimensiones del dataset. Las mismas son:

- Price
- Price US
- LotLandSize
- ConstructionSize
- Bedrooms
- Bathrooms
- CarParkingLots
- District
- Latitude
- Longitude
- PropertyType

y contiene 731 observaciones en total. A efectos de homogeneizar la unidad de medida de los valores de los inmuebles, todos los inmuebles se encuentran valuados en pesos a un tipo de cambio de 43 ARS por 1 USD, correspondiente al día 1 de Junio de 2019.


## Análisis de distribuciones
### Distribuciones de clases de variables categóricas

En el gráfico @fig:gf1 se puede apreciar la cantidad de avisos según tipo de inmueble. El tipo más popular corresponde a Lotes, con 438 unidades (60% de las observaciones), seguido de Casas con 194 filas (25,5% de las observaciones) y finalmente los Departamentos con 99 observaciones (14,5%).
Con respecto a la cantidad de avisos según la localidad del inmueble, en el gráfico @fig:gf2 se puede ver que tanto Capital como Santa Lucía tienen 179 observaciones (24,5%), seguido de Rivadavia con 159 (21,8%), Rawson con 126 (17,2%), Chimbas con 61 (8,3%) y finalmente Pocito con 27 observaciones, es decir, el 3.7%.

![Cantidad de avisos según tipo de inmueble.](source/figures/Grafico_1.png "Gráfico 1"){#fig:gf1 width=70%}
![Cantidad de avisos según localidad del inmueble.](source/figures/Grafico_2.png "Gráfico 2"){#fig:gf2 width=70%}


### Distribuciones de variables discretas

Con respecto a la cantidad de baños, el valor máximo observado fue de 5 unidades, que ocurrió en una única observación (0.13%). De la misma forma, 4 baños fue observado sólo en 4 inmuebles, es decir 0.54% de las propiedades. Por el contrario, debido a que la mayoría de los inmuebles son lotes, el 438 (60%) inmuebles no poseen baños, mientras que los inmuebles que sí poseen, muestran una relación decreciente:

- 25% ó 185 observaciones tienen sólo 1 baño,
- 11% ó 81 observaciones tienen 2 baños y,
- 3% ó 22 observaciones tienen 3 baños.

![Cantidad de avisos según número de baños.](source/figures/Grafico_3.png "Gráfico 3"){#fig:gf3 width=70%}

Para analizar más de cerca la distribución de los baños, se realizó una tabla dinámica donde se agrupó las observaciones por tipo y cantidad de baños para contar la cantidad de observaciones en cada par de valores. El resultado se puede observar en el gráfico @fig:gf4, donde queda expreso que del total de inmuebles que poseen un baño, 50,8% corresponden a Casas (94 unidades) y 49,2% (91 observaciones) corresponden a Departamentos. La diferencia se ve ampliada cuando nos enfocamos en inmuebles con 2 baños, donde el 92,5% de los mismos son Casas, es decir, 75 inmuebles, mientras que el resto (6 observaciones) corresponden a Departamentos. Finalmente, si bien no se observaron departamentos con 3 baños, sí se relevaron 2 departamentos con 4 baños, misma cantidad que casas.

![Distribución de baños según tipo de inmueble.](source/figures/Grafico_4.png "Gráfico 4"){#fig:gf4 width=70%}

El histograma de los dormitorios se puede observar en el gráfico @fig:gf5. En el mismo se utilizó el tipo de inmueble para resaltar la distribución entre los mismos. Dejando de lado los Lotes por razones obvias, se puede apreciar que en departamentos, la cantidad más frecuente es un dormitorio (42 observaciones), mientras que las casas frecuentan tener tres unidades (102 observaciones). De los inmuebles con un dormitorio, el 89% fueron departamentos, en cuanto a los inmuebles con dos dormitorios, la mayoría son casas (44 unidades, 70%) mientras que sólo se observaron 19 departamentos (30%). Por otra parte, se observó que el 85% de los inmuebles con 4 dormitorios son casas (29 observaciones), mientras que el 15% restante fueron 5 unidades de departamentos. Finalmente sólo 14 inmuebles presentaron 5 dormitorios, en todos los casos fueron casas.

![Cantidad de avisos según número de Dormitorios.](source/figures/Grafico_5.png "Gráfico 5"){#fig:gf5 width=70%}
![Distribución de dormitorios según tipo de inmueble.](source/figures/Grafico_6.png "Gráfico 6"){#fig:gf6 width=70%}

En cuanto a las cocheras, se observaron tres valores extremos con una única observación en cada caso. Se trata de inmuebles con cocheras para 4, 6 y 8 vehículos. Por otra parte, debido a la gran cantidad de lotes presentes en el conjunto de datos, la mayoría de los inmuebles no tiene cochera. Del total de inmuebles que no tienen cochera, los lotes representan el 86,5% mientras que los departamentos representan el 10% (51 inmuebles) y las casas sólo el 3,5% (17 unidades). Si se analiza desde el punto de vista del tipo de inmueble, estos 17 inmuebles representan el 8,7% de las casas, mientras que los 51 departamentos corresponden al 51,5% de departamentos.


![Distribución de cocheras según tipo de inmueble.](source/figures/Grafico_7.png "Gráfico 7"){#fig:gf7 width=70%}

Siguiendo la línea de análisis a partir del tipo de inmueble, el 57,7% de las casas tiene 1 cochera (112 unidades), el 27,3% (53 unidades) tiene cochera para 2 vehículos y el 4,6% tiene para 3 autos (9 observaciones). Por otra parte, del total de inmuebles con cochera para 1 vehículo, 70,4% son casas, mientras que el resto son departamentos (47 unidades). En cuanto a los departamentos que sí tienen cocheras, el 47,5% (47 observaciones) tiene cochera para 1 vehículo, mientras que sólo el 1% (1 inmueble) tiene para 2 vehículos.


## Análisis y transformaciones de la(s) variable(s) objetivo

Dado que el objetivo es modelar el precio de los inmuebles, es de esperarse que la distribución sea normal y asimétrica. En el gráfico @fig:gf8 puede observarse el histograma y la función de kernel ajustada sobre los datos, ambos codificados en colores según el tipo de inmueble. Debido a esta forma característica, se optó por realizarle una transformación logarítmica. De esta manera, al momento de ajustar las regresiones, se puede interpretar la variación de la variables exógenas como la variación porcentual. Al mismo tiempo, permiten diferenciar mejor los rangos de valores y a que se asemeje una distribución normal simétrica. Esta distribución puede verse en el gráfico @fig:gf9.

![Distribución de precios de los inmuebles según tipo.](source/figures/Grafico_8.png "Gráfico 8"){#fig:gf8 width=100%}
![Distribución del logaritmo de precios de los inmuebles según tipo.](source/figures/Grafico_9.png "Gráfico 9"){#fig:gf9 width=100%}

Alternativamente, la variable objetivo puede plantearse como el precio por metro cuadrado. De esta manera, se puede observar cómo varía el precio de un inmueble a medida que se agregan más metros cuadrados a un inmueble, al mismo tiempo que permite utilizar una unidad de medida más agnóstica. En efecto, una vez creada dicha variable -pm2-, podemos ver que tiene una distribución similar a la variable precio. Con lo cual también se optó por realizar la transformación logarítmica. Dichas distribuciones pueden observarse en los gráficos @fig:gf10 y @fig:gf11.

![Distribución de valores de precio por metro cuadrado de inmuebles, según tipo.](source/figures/Grafico_10.png "Gráfico 10"){#fig:gf10 width=100%}
![Distribución del logaritmo de precio por metro cuadrado de los inmuebles, según tipo.](source/figures/Grafico_11.png "Gráfico 11"){#fig:gf11 width=100%}


## Análisis de correlaciones

Debido a la diversidad de intereses que se persiguieron en el estudio, se realizó una gran cantidad de transformaciones a las variables dependientes e independientes. Para poder realizar una apreciación si dichas transformaciones tenían sentido, se realizó una matriz de correlación entre las variables exógenas y las cuatro variables endógenas.
En el gráfico @fig:gf12 se puede observar el grado de correlación lineal entre las variables. Algunas variables se presentan duplicadas, en el sentido que se utilizaron la variables originales, por ejemplo, las discretas y por otra parte, sus dummies correspondientes. El objetivo de dicha transformación se explica en el Capítulo III, en la sección de Modelos modificados. 
Por su parte, las variables de latitud ($lat$) y longitud ($long$) muestran una correlación con las variables objetivo, aunque en este caso estamos capturando sólo una porción de dicha relación. En la matriz se establece una relación positiva con la latitud, es decir, mientras más al norte, más caras las propiedades y, al mismo tiempo, una correlación negativa con la longitud, en otras palabras, mientras más al oeste se ubique el inmueble, mayor será su precio. Dicho esto, es de esperarse que dicha relación sea no lineal, tal como se representará gráficamente con los mapas y polígonos en la sección de análisis geoespacial.

![Matriz de correlación de variables endógenas y exógenas.](source/figures/Grafico_12.png "Gráfico 12"){#fig:gf12 width=100%}


## Ratios
Debido a que se observó una interacción entre las variables de metros construidos y metros totales, así como también con la cantidad de baños y de dormitorios, se optó por realizar dos variables que capturen dichas relaciones: $b\_ratio$ y $bed\_bath\_ratio$, respectivamente. 
En el gráfico @fig:gf13 se puede apreciar que a medida que aumenta la cantidad de dormitorios -eje x-, aumenta la cantidad de baños, los cuales están representados en colores. Al mismo tiempo, más del 40% de los inmuebles tienen 3 dormitorios.

![Proporción de baños según cantidad de dormitorios.](source/figures/Grafico_13.png "Gráfico 13"){#fig:gf13 width=70%}


## Dependencia espacial
A efectos de poder visualizar la distribución de precios en el espacio, se optó por realizar un gráfico de dispersión utilizando como base el mapa de la ciudad de San Juan y sus alrededores. De esta manera, el gráfico @fig:gf14 representa los precios de los inmuebles en el espacio geográfico en escala logarítmica para facilitar la interpretación de las magnitudes de la misma. 
En el mismo se puede observar claramente el aumento de precios por metro cuadrado en las zonas céntricas, como es habitual en casi todas las ciudades. Asimismo, puede apreciarse rápidamente que en la zona al oeste de la capital, los precios son mayores que hacia el este.
Para poder estimar los modelos que contemplan dependencia espacial, fue necesaria la construcción de la matriz de adyacencia utilizando la triangulación de Delaunay. En el gráfico @fig:gf15 se pueden apreciar los polígonos de Voronoi (en celeste) y el grafo de adyacencia de las observaciones (en negro). En el grafo, cada nodo representa una observación y se conecta a través de aristas o bordes a sus puntos vecinos. Los polígonos, por su parte, son construidos partiendo las aristas en el punto medio entre dos nodos. Para visualizar este efecto, en el gráfico @fig:gf16 se representa cada polígono (asociado a cada observación) con el color correspondiente a su precio por metro cuadrado, en escala logarítmica.

<!-- Cuidado!! Los gráficos tienen una enumeración distinta a partir de acá -->
![Mapa de dispersión coloreado en función del logaritmo de precio por metro cuadrado. Como puede observarse, no se trata de una distribución aleatoria, sino que se puede observar que hacia el centro del Gran San Juan, los precios aumentan.](source/figures/Grafico_15_mod.png "Gráfico 14. Mapa de dispersión de precios."){#fig:gf14 width=100%}
![Polígonos de Voronoi. En la figura se pueden observar los puntos originales representadaos como nodos (vértices), y su relación de vecino con los polígonos adyacentes que se encuentran represetados por bordes o aristas.](source/figures/Grafico_16_mod.png "Gráfico 15. Grafo y polígonos de Voronoi."){#fig:gf15 width=100%}
![Mapa coroplético del logaritmo de precio por metro cuadrado. En esta representación, cada observación es extendida a un polígono de Voronoi para generalizar el área de influecia.](source/figures/Grafico_17_mod.png "Gráfico 16. Mapa coroplético."){#fig:gf16 width=100%}

