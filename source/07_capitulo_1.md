\setcounter{page}{1}
\pagenumbering{arabic}
\doublespacing
\setlength{\parindent}{0.5in}

# Aspectos metodológicos

## Justificación

Probablemente el lector o lectora notó en los últimos años que las palabras que le recomienda el teclado de su Smartphone son cada vez más precisas y personalizadas, o quizás le pasó de estar hablando de un tema con alguien para luego revisar su teléfono y encontrar publicidad relacionada al tema. Seguramente también notó que las recomendaciones de películas o series en su cuenta de Netflix son más acertadas o que si tuvo una “conversación” con Siri o Google Assistant, estas asistentes lograron interpretar lo que les dijo verbalmente. Esto, obviamente, no es casualidad, ya que todos los ejemplos mencionados tienen algún algoritmo –o varios- de aprendizaje automático de fondo que permiten realizar predicciones sobre acciones futuras en base a características observadas de su comportamiento.

En el día a día de los economistas puede requerirse el uso de modelos econométricos, ya sea para interpretar las relaciones entre distintas variables, así como también para realizar predicciones de alguna variable objetivo, utilizando como base algún modelo económico que permita encontrar una relación paramétrica, es decir, con relación funcional entre dichas variables. Si bien los ejemplos mencionados en el párrafo anterior no son de interés inmediato para un economista, los mecanismos utilizados para lograr los objetivos, sí. Éstos están construidos a partir de los mismos conceptos estadísticos que estudiamos, pero suelen tener una gran diferencia: no exponen una relación lineal o bien, son modelos no paramétricos.

El objetivo de este trabajo es exponer algunos métodos de aprendizaje estadístico alternativos a los vistos en el plan de estudio de la carrera, así como también los costos y beneficios de implementar una solución u otra, dadas las particularidades necesidades que puede tener una persona en el rol de economista. En efecto, la problemática que se resolverá consiste en ajustar un modelo que permita predecir el precio de un inmueble, utilizando los métodos clásicos, métodos de econometría espacial, así como también métodos de conjunto.


## Marco Teórico

Como punto de partida, entendemos como _aprendizaje automático_ o _estadístico_ –_Machine Learning_ en inglés– al estudio de algoritmos computacionales que mejoran automáticamente con la experiencia[^1]. En otras palabras, se busca desarrollar algoritmos que puedan encontrar relación y tendencias entre los datos (experiencia), es decir, aprender de los datos para realizar alguna predicción. Difiere de la programación clásica, en la cual se deben desarrollar algoritmos manualmente a partir de los datos para obtener las respuestas buscadas. Bajo este paradigma, los modelos estadísticos como la regresión lineal o la regresión logística, forman parte de esta área de estudio, así como también el resto de algoritmos que se desarrollarán más adelante.

En cuanto al caso de estudio, es necesario ajustar un modelo econométrico que permita estimar el precio de mercado de los inmuebles radicados en la provincia. A tales efectos, el modelo implementado se denomina Modelo de Precios Hedónicos, también conocido como regresión hedónica, a través del cual se estiman los valores de las características de un bien que indirectamente afectan su precio de mercado[^2]. Por otra parte, el modelo también puede ser utilizado para estimar la demanda de un bien. Dadas estas cualidades, y su interpretación relativamente sencilla, este método ha sido ampliamente utilizado no sólo en investigaciones relacionadas al mercado inmobiliario, sino también en investigaciones sobre consumidores y mercados, para el cálculo de índices de precios al consumidor, para la valuación de autos, entre otros[^3].

[^1]: @mitchell1997
[^2]: @Shanaka2010
[^3]: En referencia a los temas mencionados, se pueden resaltar los trabajos de los autores @HirschmanHolbrook1982, @Moulton1996, @BerryBednarz1975 y @CowlingCubbin1972, entre muchos otros.


### Origen e historia del modelo de precios hedónicos.

La creación de la metodología, según diversos autores[^4], suele ser atribuida a Andrew T. Court, quien desarrolló un índice de precios hedónicos para automóviles en 1939, en el cual utilizó el término _‘hedónico’_ para describir la ponderación de la importancia relativa de los varios componentes que componían a los vehículos. En su trabajo elabora problemas de no-linealidad y con cambios en los conjuntos de bienes que componen al automóvil.

Sin embargo, otros autores[^5] argumentan que el pionero en la metodología fue George C. Haas en el año 1922, aunque sin utilizar el término _‘hedónico’_ para describir la metodología. En el trabajo original, Haas utiliza la metodología para estimar los valores por acre ajustados por el año de venta, el tipo de carretera y el tipo de ciudad, basado en 160 transacciones realizadas en Minnesota.

El modelo eventualmente fue popularizado por Zvi Griliches a fines de los años 50s y principios de los 60s en diversos trabajos, lo que dio lugar a que los autores @Lancaster1966 y @Rosen1974 realizaran aportes –quizás los más importantes– a la base teórica del modelo. El primero estableció los fundamentos microeconómicos para analizar las características que componen los bienes y aplica dicho análisis a diversos productos. Rosen por otra parte, deriva funciones de subasta correspondientes a consumidores maximizadores de utilidad y funciones de oferta de productores maximizadores del beneficio. De esta manera, pone en evidencia un mercado implícito de características de los productos diferenciados y analiza el equilibrio del mercado. Estos precios implícitos de las características de los bienes sirven para que los productores y consumidores puedan optimizar sus elecciones de conjuntos de producción y de consumo, respectivamente.

La base teórica de Rosen deja como resultado una metodología de dos etapas para determinar los precios hedónicos: la primera consiste en estimar la ecuación hedónica; la segunda, se encarga de determinar los precios implícitos de las características a través de derivadas parciales del precio, respecto de tal característica. En este modelo, el ingreso del consumidor se toma en cuenta a la hora de definir la restricción presupuestaria, lo que implica que la intención marginal de pagar por cierto atributo puede verse modificada por un cambio en el ingreso[^6].

[^4]: @Bartik1987, Goodman1998 y otros.
[^5]: @ColwellDilmore1999 .
[^6]: Esto podría dar lugar al efecto ingreso o al efecto sustitución desarrollado en la Teoría del Consumidor.

Las diferencias en los enfoques de Rosen y Lancaster ponen en manifiesto dos alternativas de análisis que permiten una mejor estimación dependiendo del tipo de mercado y producto. De esta forma, el análisis de Lancaster se basa en la idea que la utilidad de un bien depende de sus características y que dichos bienes pueden ser concentrados en grupos basados en ellas. La utilidad del consumidor se origina de las distintas características que los bienes poseen, no sólo en las diferentes cantidades de bienes. Esto permite que el enfoque se ajuste mejor a bienes de consumo. Finalmente, el modelo asume una relación lineal entre el precio de los bienes y las características contenidas en esos bienes. 

El modelo de Rosen sostiene que existe un rango de bienes, pero que los consumidores no adquieren los atributos preferidos comprando una combinación de bienes, sino que cada bien es elegido de un espectro de ‘marcas’ y es consumido discretamente. Consecuentemente, el modelo suele ajustarse mejor a la demanda de bienes durables. En este caso, el modelo asume una relación no lineal entre el precio de los bienes y sus atributos, ya que el precio implícito no es una constante, sino una función de las cantidades del atributo ‘comprado’ y del resto de los atributos.


### Ecuaciones del modelo
Cualquiera sea el caso del modelo especificado, el método de estimación del modelo de precios hedónicos se realiza a través del análisis de regresión múltiple, ya sea a través de Mínimos Cuadrados Ordinarios (MCO) o del método Máxima Verosimilitud. En ambos casos, se busca encontrar un vector de parámetros que mejor explique los valores de las variables explicativas. Estas variables suelen ser las características de los bienes o transformaciones matemáticas –como las dummies, o las variables instrumentales–, lo que permite contemplar relaciones de no-linealidad, interacción de variables u otras situaciones complejas. Esta información permite construir un índice de precios, el cual puede ser utilizado para comparar distintas ciudades (sección cruzada) o una misma ciudad en el tiempo (serie de tiempo).

La forma estándar de estimación de la regresión toma la forma:
$$
\begin{aligned}\label{eqn:formastd}
    P = f(I,N,L,C,t)
\end{aligned}
$$
donde $P$ es el precio del inmueble (o el valor del alquiler), $I$ son los atributos relativos al inmueble, $N$ son las características del barrio, $L$ son variables particulares del mercado, $C$ son las condiciones contractuales y $t$ es un indicador del tiempo.

Dada la complejidad en la forma de medición de algunas variables y la disponibilidad de datos relacionados, es poco común ver todas las variables utilizadas en un mismo trabajo. Según @Malpezzi2003, las variables que habitualmente se utilizan son:

- Número y tipo de ambientes (baños, dormitorios, etc.)
- Tipo de inmueble (departamento, casa, casa quinta, terreno, etc.)
- Antigüedad
- Tipo de construcción (en seco, Steel frame, tradicional)
- Características estructurales (sótano, cochera, chimenea, piscina, etc.)
- Servicios disponibles (gas, electricidad, agua, cloacas)
- Instalaciones climáticas (calefacción, piso radiante, A/C, etc.)
- Superficie del terreno
- Superficie construida

En cuanto a la forma funcional, la estimación de la regresión de la ecuación puede ser lineal, semi logarítmica o log-log, siendo la segunda la más utilizada. Este método presenta la ventaja de que los coeficientes de las variables estimadas representan las proporciones del precio que son directamente atribuibles a las características. Por su parte, la implementación del método log-log permite estimar las elasticidades de cada una de las características de los inmuebles.

Con el desarrollo del modelo y su aplicación al mercado inmobiliario, distintos autores[^7] han incluido variables que toman en cuenta la dimensión espacial en presencia de dependencia espacial y autocorrelación espacial. A tales efectos, una de las formas más sencillas de incluir aspectos espaciales en el modelo es incluir una variable de distancia entre la propiedad y el distrito comercial. Sin embargo, el análisis espacial suele ser considerado un arma de doble filo: permite obtener resultados más robustos y explicativos, pero al mismo tiempo, complejiza el análisis considerablemente.

[^7]: Se pueden resaltar: @Jackson1979, @Dubin1988, @Anselin1988, @BasuThibodeau1998, entre otros.


### Aplicaciones en el mercado inmobiliario
El modelo de precios hedónicos suele ser una herramienta conveniente en un contexto de estimación de precios de viviendas en distintas situaciones, tales como la generación de un índice de precios ajustado por la calidad de la construcción, la generación de una herramienta de tasación automática o masiva y para explicar las variaciones de precios o determinar el impacto en los precios de las viviendas a través de ciertas características.

Dependiendo de las necesidades, los objetivos del modelo y la disponibilidad de datos, existen diversas formas de encarar la estimación. Una primera consideración es si se utilizan datos de operaciones realizadas o si se realiza en base al stock de inmuebles disponibles. La primera tiene la ventaja que, al tratarse de operaciones efectivizadas, se tiene certeza de los valores a los cuales se concretaron dichas transacciones. Además, dicha información suele ser aportada por instituciones colegiadas de profesionales o cámaras empresarias del sector, por lo cual suele haber más datos de características individuales disponibles, pero una cantidad menor de observaciones, esparcidas en el tiempo. Si se trata de inmuebles que forman parte del stock disponible, suelen haber más observaciones, pero con menos datos individuales y, por otra parte, no se tiene certeza del precio. Esto se debe a que no se tiene noción si los inmuebles publicados ya fueron comercializados o no, ni el precio real al cual se concretó la operación. Además, aparece un riesgo de sesgo de selección de observaciones, dado que existe una correlación entre el precio del inmueble y los datos disponibles sobre sus características: los inmuebles de mayor valor suelen disponer de más detalles de sus características que los de menor valor.

Ya sea que se cuente con datos de transacciones efectivizadas o con el stock de inmuebles, existen tres métodos de estimación del modelo, cada uno con sus ventajas y sus desventajas, que permiten que sean utilizados en distintas ocasiones con mayor grado de éxito, las cuales serán explicadas a continuación.


#### Método de la variable dummy del tiempo
Es el método estándar de estimación del modelo de precios hedónicos y habitualmente utiliza la forma semilogarítmica para estimar el peso relativo de cada variable explicativa. Dicha metodología consiste en generar una variable dummy que capte el tiempo de acuerdo a la fecha en la que se realizó la transacción, a efectos de actualizar los valores históricos de las transacciones realizadas. La forma funcional es la siguiente:

$$
\begin{aligned}\label{eqn:timedummy}
    y = Z\beta + D\delta + \varepsilon
\end{aligned}
$$

donde $y$ es un vector de dimensiones $H\times{1}$ de elementos $y=\ln{p_{h}}$  siendo $p_{h}$ el precio de cada inmueble, $Z$ es una matriz $H\times{C}$ de características del inmueble, $\beta$ es un vector $C\times{1}$ de los precios sombra de las características, $D$ es una matriz $H\times{T-1}$ de variables dummies de períodos, $\delta$ es un vector $T-1\times{1}$ de precios de períodos y $\varepsilon$  el vector de errores aleatorios. $H$, $C$ y $T$ representan el número de inmuebles, el número de características y los períodos de tiempo del conjunto de datos. El formato permite incorporar funciones e interacciones de características, entre las cuales se destacan el cuadrado de los metros cuadrados o el producto entre dormitorios y baños.

Si el objetivo es construir un índice de precios ajustado por la calidad del inmueble, entonces el interés principal recae en los parámetros  que miden los efectos fijos en los logaritmos de los niveles de precios luego de controlar los efectos de las diferencias en los atributos de los inmuebles. De esta manera, el índice de precios para el período t se obtiene simplemente exponiendo el valor estimado de $\delta_{t}$, tal que:
$$
\begin{aligned}\label{eqn:priceindex}
    \hat{P_{t}}=\exp[\hat{\delta_{t}} + \frac{1}{2}{\hat{\sigma_{t}}^2}]
\end{aligned}
$$

Por otra parte, debido a que uno de los determinantes clave del precio de un inmueble es la ubicación, las estimaciones se pueden mejorar considerablemente si se la incluye. La forma que solía utilizarse en el pasado era incorporar una dummy basada en el código postal del inmueble. Actualmente, debido a la disponibilidad de datos de geolocalización y su fácil aplicación, se puede modelar la dependencia espacial lo que permite resultados más robustos y explicativos.

Este método tiene como principal ventaja su simplicidad, dado que permite estimar la variación de precios en el tiempo de manera sencilla e intuitiva, utilizando la forma funcional semilogarítmica. Por supuesto, no está exento de limitaciones, entre las que se pueden destacar el hecho de que la inclusión de un año extra de observaciones afecta el índice de los períodos anteriores o la falta de flexibilidad del modelo. Una forma de aumentar la flexibilidad consiste en interactuar las variables características del inmueble con las variables del tiempo, ya que la demanda por ciertas particularidades puede variar entre períodos. La inclusión de tales interacciones, por su parte, reduce los grados de libertad de la estimación.

Debido a que la inclusión de datos de nuevos períodos afecta el índice de los períodos anteriores, este método ha sido implementado con poca popularidad en un contexto de estimación de precios de viviendas. Lo mencionado anteriormente implica una violación de la fijación temporal, lo que significa que todos los resultados tienen que volver a ser computados cuando un período nuevo es agregado a la base de datos.


#### Métodos de imputación

Los métodos de imputación utilizan fórmulas estándares de índices de precios, como los de Laspeyres y Paasche, que son ampliamente utilizados en la construcción de índices de precios al consumidor. Ambas fórmulas miden el cambio en el precio de un conjunto dado de bienes en el tiempo. Esto requiere que el precio de cada bien en la canasta se encuentre disponible para cada período, lo cual no ocurre habitualmente en el mercado inmobiliario. Esto se debe a que es muy poco probable que una misma parcela se encuentre a la venta y se negocie en dos períodos distintos, por lo tanto, no es posible computar los índices de precios de Laspeyres o Paasche basados en transacciones reales. Ante tal impedimento, se recurre a la _imputación_  del precio de dicho bien en los períodos en los cuales no se vendió el mismo, lo cual se realiza estimando el precio $\hat{P_{th}}$ en base a los estimadores obtenidos con la regresión hedónica.

La utilización de este método agrega una nueva dimensión al problema del índice de precios, dado que otorga cierta discreción a quien construye el índice. Si el precio de una casa $h$ no se encuentra disponible en el período $t$, no queda otra que imputar el precio estimado; pero si el precio sí se encuentra disponible para tal período, existe aún la tentación a imputar el precio ya que permite reducir el sesgo de variables omitidas.

Para poder implementar el método, definimos $\hat{p_{th}}(z_{sh})$ como el precio estimado en el período $t$ de un inmueble vendido en el período $s$, lo cual se realiza reemplazando las características de la casa $h$ vendida en el período $s$ dentro de los parámetros estimados del modelo hedónico del período $t$, tal que:
$$
\begin{aligned}\label{eqn:estima}
    \hat{p_{th}}(z_{sh}) = \exp(\sum_{c=1}^c \hat{\beta_{ct}Z_{csh}})
\end{aligned}
$$

A dicha estimación corresponde corregir el sesgo[^8], lo cual realizamos agregando la mitad de la varianza del error de la ecuación de la regresión hedónica, lo que nos deja:
$$
\begin{aligned}\label{eqn:estimb}
    \hat{p_{th}}(z_{sh}) = \exp(\sum_{c=1}^c \hat{\beta_{ct}Z_{csh}}+\frac{1}{2}{\sigma_{t}^2})
\end{aligned}
$$

Con el resultado obtenido, se procede a la estimación de la fórmula de índice de precios, en este caso utilizaremos Laspeyres, que se puede definir como:
$$
\begin{aligned}\label{eqn:estimc}
    P_{st}^L = \sum_{h=1}^{H_{s}}\Bigg\{w_{sh}\left[\frac{p_{th}}{p_{sh}}\right]\Bigg\} = \frac{\sum_{h=1}^{H_{s}} p_{th}}{\sum_{h=1}^{H_{s}} p_{sh}}
\end{aligned}
$$

donde $w_{th}$ son las ponderaciones de observaciones imputadas definidas por:
$$
\begin{aligned}\label{eqn:estimd}
    w_{th} = \frac{p_{th}}{\sum_{m=1}^{H_{t}} p_{tm}}
\end{aligned}
$$
Ahora simplemente resta incorporar $\hat{p}_{th}(z_{sh})$ en la fórmula de $P_{st}^L$, lo que resulta en:

$$
\begin{aligned}\label{eqn:estime}
    L_1 : P_{st}^{L_{1}} = \sum_{h=1}^{H_{s}}\Bigg\{w_{sh}\left[\frac{\hat{p}_{th}(z_{sh})}{p_{sh}}\right]\Bigg\} = \frac{\sum_{h=1}^{H_{s}}\hat{p}_{th}(z_{sh})}{\sum_{h=1}^{H_{s}}p_{sh}}
\end{aligned}
$$

$$
\begin{aligned}\label{eqn:estimf}
    L_2 : P_{st}^{L_{1}} = \sum_{h=1}^{H_{s}}w_{sh}\left[\frac{p_{th}}{s_{th}} \right] + \sum_{h=1}^{H_{s}}\Bigg\{w_{sh}\left[\frac{\hat{p}_{th}(z_{sh})}{p_{sh}}\right]\Bigg\}
    = \frac{\left[\sum_{h=1}^{H_{s}}p_{th} + \sum_{h=H_{st}+1}^{H_{s}}\hat{p}_{th}(z_{sh})\right]}{\sum_{h=1}^{H_{s}}p_{sh}}
\end{aligned}
$$

[^8]: Esto se debe a que estamos estimando $p_{th}(z_{sh})$ y no $ln{p_{th} (z_{sh})}$.

$L_1$ y $L_2$ representan dos formas distintas de estimar el índice, con resultados distintos. La diferencia surge en las observaciones que se toman: $L_1$ imputa todos los precios al período $t$, mientras que $L_2$ sólo imputa los precios de los inmuebles que no estaban disponibles en el período $s$ y $t$. Existen dos alternativas más: una de ellas consiste en imputar todos los precios para los períodos $s$ y $t$; la otra sería imputar todos los precios para ambos períodos excepto los de los inmuebles que tengan ambos datos[^9]. Si se utiliza cualquiera de los últimos dos métodos mencionados, se recurre al caso de _doble imputación_, dado que en ambos se reemplazan valores reales de los períodos $s$ y $t$ por valores estimados. Esta metodología, si bien parece contradictoria o no deseada, puede ser útil en ciertas ocasiones ya que permite reducir el sesgo de variables omitidas. Supongamos que hay una observación la cual tiene precio estimado mayor al real, tal que:
$\hat{p}_{th}(z_sh) > p_{sh}$

[^9]: Para ver una representación algebraica de dichas alternativas, ver @Hill2011

Esto puede ser porque el comprador obtuvo una ganga o porque el inmueble tiene malos indicadores de variables omitidas (como puede ser polución o crimen si no estuvieran contempladas en el modelo original). Si fuera cierto el segundo caso,  $\hat{p}_{th}(z_sh)$ va a sobreestimar el precio real del inmueble, ya que  $\hat{p}_{th}(z_sh)/p_{sh}$ va a tener un sesgo positivo. Por el contrario, si utilizamos las características del inmueble para estimar el precio en el período $s$ y en el período $t$, la variación del precio en el tiempo de dicha observación no estará sesgada, ya que ambas estimaciones tendrán incorporadas las variables omitidas.

Esta metodología de estimación del modelo es un tanto más flexible que el basado en dummies de tiempo, ya que permite imputar datos en períodos en los que no se disponen los mismos, aumentando la cantidad total de observaciones. Por supuesto que la metodología no está libre de críticas: por un lado, otorga demasiada discrecionalidad a quien construye el índice, por otra parte, requiere la estimación del modelo por cada período individualmente, lo que no permite realizar interacciones intertemporales entre las variables.


#### Método de características
Del mismo modo que el método de imputación, esta metodología requiere calcular el modelo hedónico para cada período por separado y requiere del uso de fórmulas de índices de precio estándares. La particularidad de esta metodología es que se construye un inmueble “tipo”, en base a los promedios, para cada período y luego se le imputan los precios de cada período, como función de sus características utilizando los precios sombras derivados del modelo hedónico. El inmueble “tipo” o “promedio” puede ser la media o la mediana. La diferencia de precios se obtiene, consecuentemente, dividiendo el precio imputado del inmueble para los dos períodos, lo que lo convierte en un modelo de doble imputación.

Cabe destacar que el inmueble tipo o promedio, puede ser tomado en cualquiera de los dos períodos de análisis y no requiere que sea construido con variables discretas, es decir, si tomamos la cantidad promedio de baños, dormitorios, metros cuadrados, metros construidos, etc., podemos utilizar unidades no enteras (p. ej.:  2,42 dormitorios, 1.65 baños, etc.). Una vez calculado el precio estimado para el inmueble de esas características en cada período, se construye el índice de precios utilizando nuevamente Laspeyres o Paasche, como se demuestra a continuación:
$$
\begin{aligned}\label{eqn:laspeyres}
    L: P_{st}^{L} = \frac{\hat{p}_{t}(\bar{z}_s)}{\hat{p}_{s}(\bar{z}_s)} = \exp\left[\sum_{c=1}^C (\hat{\beta}_{ct}-\hat{\beta}_{cs})\bar{z}_{cs}\right]
\end{aligned}
$$
con $\bar{z}_{cs} = \frac{1}{H_s}\sum_{h=1}^{H_s}z_{csh}$ 
$$
\begin{aligned}\label{eqn:paasche}
    P: P_{st}^{P} = \tilde{P}_{st}^{P} = \frac{\hat{p}_{t}(\bar{z}_s)}{\hat{p}_{s}(\bar{z}_s)} = \exp\left[\sum_{c=1}^C (\hat{\beta}_{ct}-\hat{\beta}_{cs})\bar{z}_{ct}\right]
\end{aligned}
$$
con $\bar{z}_{ct} = \frac{1}{H_s}\sum_{h=1}^{H_s}z_{cth}$

donde $H_s$ y $H_t$ son el número total de inmuebles observados en cada período, $\bar{z}_{cs}$ y $\bar{z}_{ct}$ son los vectores de características de los inmuebles promedio de cada período. Para extender el análisis se puede calcular la media geométrica de ambos índices y obtener un híbrido de doble imputación, que es análogo al índice de Fisher, tal que:
$$
\begin{aligned}\label{eqn:fisher}
    F: P_{st}^{F} = \sqrt{P_{st}^{L}\times{P_{st}^{P}}} = \exp\left[\frac{1}{2}\sum_{c=1}^C (\hat{\beta}_{ct}-\hat{\beta}_{cs})(\bar{z}_{cs}+\bar{z}_{ct})\right]
\end{aligned}
$$
Debido a este método realiza imputación de valores, sufre de los mismos defectos y virtudes que aquel. Otro de los inconvenientes que se presenta con el uso de esta metodología es cuando se incluyen ajustes por dependencia espacial. Esto se debe a una sencilla razón: ¿cómo determinamos la _“ubicación promedio”_ de los inmuebles? A pesar de esto y debido a la sencillez instrumental del método, éste es el más utilizado para computar índices de precios. Otra ventaja que es menester señalar de la metodología es su utilidad para comparar índices de precios entre diferentes regiones. 


### Dependencia espacial y datos geoespaciales
En la industria inmobiliaria –principalmente en países angloparlantes– existe una frase un tanto cómica que resume las tres reglas del real estate: “ubicación, ubicación y ubicación”. Como uno puede imaginarse, dicha variable es una de las más importantes a la hora de determinar el precio de un inmueble. Debido a este motivo, es lógico suponer que existe dependencia espacial en los precios de los inmuebles[^10], ya que muchos de los determinantes del precio de los inmuebles suelen ser compartidos por las propiedades vecinas. Las parcelas de los barrios suelen ser edificadas simultáneamente, lo que resulta en diseños y características similares; inclusive las propiedades vecinas disfrutan de las mismas comodidades locales. 

[^10]:  @PaceGilley1997, @BasuThibodeau1998, @Bourassa2002 son algunos ejemplos de estudios empíricos que alertan la existencia de dependencia espacial en los residuos.

En econometría es habitual suponer la independencia de las observaciones para poder realizar un análisis de inferencia estadístico. Una de las formas de explicitar matemáticamente esto es suponer que los valores observados $y_i$, para un individuo $i$, son estadísticamente independientes de otros valores $y_j$, para el individuo $j$, tal que $E(y_iy_j) = E(y_i)E(y_j) = 0$, donde $E(\cdot)$ es el operador de la esperanza. La existencia de correlación espacial implica una violación los supuestos de Gauss-Márkov[^11], en este caso $E(y_iy_j)\neq0$, lo que nos da como resultados estimadores ineficientes – en el mejor de los casos- o estimadores sesgados, ya que los regresores pueden estar correlacionados entre ellos o con respecto al residuo. Por lo tanto, la inclusión explícita de la dependencia espacial ayuda a eliminar el problema de variables locales omitidas.

[^11]: Principalmente las condiciones de no colinealidad, exogeneidad y homocedasticidad.

Existen dos formas de incluir la variable _ubicación_ en el modelo: la primera es generando una dummy de cada barrio, lo cual requiere tener información precisa de los límites de los mismos y la construcción de tantas dummies como barrios se contemplen; la segunda es a través de las coordenadas de latitud y longitud. Cualquiera sea el caso, se requiere la creación de una matriz $W$ de ponderaciones espaciales, la cual tendrá dimensiones $H\times{H}$ donde $H$ es la cantidad de observaciones incluidas en la regresión. A efectos de simplificar los valores y el peso computacional, se suele utilizar el algoritmo de triangulación de Delaunay, que simplifica la matriz a ceros y unos. Dicho algoritmo utiliza los datos de longitud y latitud de cada inmueble y crea un conjunto de triángulos en los que no existen más puntos que los vértices del mismo en su circunferencia circunscrita. Dos inmuebles se consideran vecinos si ambos son vértices del mismo triángulo y toman valor 1 en la matriz, esto es, si los inmuebles $j$ y $k$ son vecinos, entonces las posiciones $jk$ y $kj$ toman el valor 1 en la matriz, de lo contrario toman el valor cero. Además, la diagonal de ubicaciones $jj$ y $kk$ toman valor 0 por defecto. Un atajo que se realiza en la práctica, tiene que ver con la creación de los polígonos de Voronoi a partir de los puntos observados. Con estos polígonos se procede a considerar vecinos en base a un criterio de contigüidad como Rook, Bishop o Queen, que responden al tipo de contigüidad que existen en las casillas de ajedrez, en base al tipo de movimiento de dichas piezas (Torre, Alfil o Reina).

Una vez creada la matriz de ponderaciones espaciales $W$, la dependencia espacial puede ser capturada en un modelo hedónico simultáneamente autorregresivo (SAR), que puede ser definido de la siguiente forma:
$$
\begin{aligned}\label{eqn:SAR}
    y=\beta X+u .
    u=\lambda Wu+\varepsilon .
\end{aligned}
$$
donde  es un vector $H\times{1}$ de errores aleatorios. Como siempre, se estima el valor del vector  de dimensiones $C\times{1}$ y, en este caso, se estima el escalar $\lambda$, el cual mide la influencia porcentual local promedio de las observaciones vecinas para cada observación. Es decir, mide el porcentaje de la variación en $u_h$ que es causado por las influencias locales de los vecinos de $h$. Lo atractivo del modelo es que sólo requiere construir $W$ y estimar los parámetros $\beta$ y $\lambda$, lo cual se realiza utilizando el método de máxima verosimilitud –en vez de Mínimos Cuadrados Ordinarios–. 

Existen otras alternativas al modelo SAR, como por ejemplo la creada por @LeSage1999 y denominada “modelo simultáneo autoregresivo-regresivo”, que captura la dependencia espacial de la siguiente forma:

$$ y = \rho Wy + X\beta +\varepsilon $$

donde los parámetros a estimar son $\rho$ y $\beta$ con el método de máxima verosimilitud, nuevamente. 

Una alternativa al método de triangulación de Delaunay es utilizar el de los _k vecinos más cercanos_, el cual consiste en tomar una observación y seleccionar la cantidad $k$ de vecinos en el espacio que se encuentren a menor distancia. Este enfoque fija siempre la cantidad $k$ de vecinos, mientras que Delaunay sólo considera vecinos a los que se encuentran dentro del área de circunferencia circunscripta, por lo que cada observación puede obtener una cantidad distinta de vecinos. Aquí yace la principal desventaja del método de los $k$ vecinos más cercanos, donde al forzar la cantidad de vecinos, los _outliers_ -observaciones atípicas- tienen una ponderación mayor de la esperada. Aun así, es un método de fácil estimación que produce estimadores considerablemente mejores que si no se tuviera en cuenta la dimensión geoespacial. Habitualmente se lo suele tomar como punto de partida para conocer los efectos de los atributos de las observaciones y luego ajustar su eficacia con métodos más robustos.

Si bien existen múltiples métodos que buscan interpretar la dependencia espacial que tienen los precios de los inmuebles, éstos no suelen ser utilizados en índices de precios, ya que la mayoría de los índices suelen ser construidos a través del método de características. Recordemos que esta metodología crea un inmueble promedio y estima la variación del precio en el tiempo, pero cuando se trata de ubicación, el concepto de promedio no tiene mucho sentido: se generaría un punto que poco tiene que ver con el precio _promedio_ de la zona donde caiga; más aún, podría caer en el agua, si se tratara de una ciudad desarrollada sobre un puerto, una bahía o sobre ambos lados de un río.

### Métodos no paramétricos
Los métodos no paramétricos se caracterizan por no asumir ninguna relación funcional del modelo hedónico, por lo que evitan cualquier problema de especificación errónea, es decir, no suponen una relación lineal o cuadrática o de cualquier tipo entre las variables descriptivas y las dependientes. Esto las convierte en una gran herramienta para grandes conjuntos de datos, ya que en tales situaciones obtienen menores errores para predicciones fuera de la muestra.
Existe un universo de modelos no paramétricos que han tomado relevancia en los últimos años con el _boom_ de la disponibilidad de datos de gran escala _–Big Data–_ y el desarrollo de la disciplina de Ciencia de Datos. Entre ellos se destacan los árboles de decisión, _Random Forest_, _Gradient Boosting_ y las _Redes Neuronales_, estas últimas conocidas como _Deep Learning_.

Al no tener una forma funcional establecida, estos métodos gozan de una mayor flexibilidad para realizar las estimaciones. Al mismo tiempo, dicha particularidad no les permite estimar los precios sombras de las características. Por este motivo es poco común encontrarse con índices de precios de viviendas construidos a partir de esta metodología, ya que el objetivo principal de los índices de precios es estimar la variación de los mismos en el tiempo, sean los precios de los inmuebles o los precios sombra de las características. Debido a esto, esta metodología suele utilizarse para construir índices de precios espaciales, en vez de índices de precios temporales, lo que lo convierte en una gran herramienta para comparar precios entre regiones en un mismo periodo, así como también para predecir el precio de los inmuebles, habitualmente visto en herramientas de tasación online.

#### Árboles de clasificación y regresión (CARTs)
Los árboles de decisión arrancan desde un paradigma de las ciencias de la computación bastante sencillo, a partir del cual realizan particiones binarias en el espacio multidimensional de las variables exógenas. En otras palabras, van generando rectángulos en el espacio multidimensional dónde se encuentran los datos y ajustan un modelo sencillo en cada uno de estos, tal como una constante. En el caso de la regresión, a cada subespacio generado le asignan el valor medio de los datos. Cada partición _-nodo-_ tiene una descripción sencilla como $X_1=t_1$ a partir de la cual el algoritmo genera dos regiones -_ramas_- $R_1$ y $R_2$, una para valores menores o iguales a $t_1$ y otra para los valores mayores. De esta manera, el algoritmo predice la constante $c_1$ para la región $R_1$ y $c_2$ para la región $R_2$. En el caso de una regresión, los valores de ci están dados por la media para la región $R_i$. 

Formalmente, considerando un conjunto de datos con $p$ variables independientes y $N$ observaciones, es decir $(x_i,y_i)$ con $i=1, 2, …, N$ y $x_i=(x_{i1}, x_{i2}, …, x_{ip})$, se generan $M$ particiones $R_1, R_2, …, R_M$ y se asignan las constantes $c_m$ para cada región, tal que:
$$
\begin{aligned}\label{eqn:carta}
    \hat{f}(X)=\sum_{m=1}^M c_mI(x_i\in R_m)
\end{aligned}
$$
Utilizando el criterio de minimización de la suma de errores cuadráticos $\sum(y_i-f(x_i))^2$, la mejor estimación de $\hat{c}_m$ viene dada por el promedio $y_i$ en la región de $R_m$:
$$
\begin{aligned}\label{eqn:cartb}
    \hat{c}_m = ave(y_i|x_i\in R_m),
\end{aligned}
$$
donde $ave$ es el promedio de $y_i$ para las $x_i$ pertenecientes a $R_m$.

Por supuesto, la clave del algoritmo está en encontrar los puntos donde realizar los cortes de las $p$ variables. Para conseguirlo, se busca minimizar la suma de los errores cuadráticos de ambas regiones generadas por un corte en $x$. Matemáticamente se puede definir un corte $s$ en la variable $j$, que genera dos regiones:

$R_1(j,s)=\{X|X_j\leq s\}$ t $R_2(j,s)=\{X|X_j>s\}$.

Para encontrar $s$, se busca el valor que resuelve:
$$
\begin{aligned}\label{eqn:cartc}
    \min{j,s}\left[\min_{c_1}\sum_{x_i\in R_1(j,s)}(y_i-c_1)^2+\min_{c_2}\sum_{x_i\in R_2(j,s)}(y_i-c_2)^2\right]
\end{aligned}
$$
donde para cada par $(j,s)$ las minimizaciones de $c_1$ y $c_2$ son resueltas por:

$\hat{c}_1 = ave(y_i|x_i\in R_1(j,s)$ y $\hat{c}_2 = ave(y_i|x_i\in R_2(j,s)$.

El problema inherente de este algoritmo, es que el árbol de regresión puede crecer hasta el punto en el cual cada observación coincide con la cantidad de _hojas_ o nodos terminales, lo cual genera un sobreajuste a los datos de entrenamiento y pérdida de generalización del modelo a datos no observados. Esto se debe a que si el modelo tiene i hojas en un conjunto de $i$ observaciones, el error cuadrático del modelo, será 0, ya que éste podrá predecir siempre el mismo valor con el que fue entrenado, generando un $R_2=1$.

Hasta dónde dejar crecer el árbol es cuestión de cada problemática específica que se esté analizando. Existen dos formas de evitar el sobreajuste: una consiste en establecer una condición para la generación de _ramas_ o nodos, mientras que la otra se conoce como _poda_. La primera es un tanto arriesgada, ya que, si existen ramas valiosas en otra variable, pero no se cumple con la condición establecida para realizar la partición, el algoritmo no podrá encontrar dicha rama. La segunda consiste en permitir el sobreajuste y luego recortar las hojas y ramas que provocan el sobreajuste a los datos de entrenamiento. 

Una alternativa usual, consiste en encontrar la cantidad de ramas y hojas del árbol de regresión generando $n$ cantidad de modelos, de complejidad variada y evaluando la capacidad de generalización del mismo utilizando _validación cruzada_.

Esta técnica consiste en subdividir los datos de entrenamiento en distintos grupos con los cuales se entrenará y evaluará el comportamiento del modelo. Es una técnica habitualmente utilizada en la ciencia de datos con el objetivo de encontrar los _hiper-parámetros_ que optimizan el modelo. Es decir, aquellos parámetros del modelo -por ejemplo, la cantidad de hojas y ramas de un árbol de regresión- que debe definir el investigador a su criterio y que no se pueden definir con las funciones de costo asociadas al modelo.

#### Técnicas de conjuntos (ensemble methods)
Las técnicas de conjunto consisten en ajustar distintos modelos en virtud de minimizar el error de estimación y combatir la elevada varianza de los modelos complejos, es decir, el sobreajuste. Éstas se pueden agrupar en dos categorías:

- Técnicas de conjunto simultáneas o paralelas.
- Técnicas de conjunto secuenciales o aditivas.

La primera consiste en ajustar una gran cantidad de modelos complejos (de elevada varianza, es decir, que sobreajustan a los datos) y promediar los resultados de dichos modelos. El mecanismo utilizado para generar resultados robustos se conoce como _Bagging_ o _**B**ootstrap_ _**Agg**regat**ing**_ y su funcionamiento es bastante sencillo: dado un set de datos $Z=(z_1. z_2, ..., z_N)$ donde $z_i = (x_i,y_i)$, se realiza una selección aleatoria de $N$ observaciones $z_i$ con reposición. De esta manera, obtendremos una cantidad $B$ de conjuntos de datos creados a partir del set original, donde cada uno de ellos tendrá Ncantidad de observaciones. Esta metodología se conoce como _Bootstrap_ y permite aumentar sintéticamente el tamaño de los conjuntos de datos. Una vez generados los $B$ conjuntos de datos, se realiza un ajuste del modelo complejo a cada uno de estos conjuntos. Finalmente, los resultados de éstos se promedian para poder estimar las medidas de error totales -o el $R^2$. 

Una aplicación especial de esta metodología se conoce como _Random Forest_, donde además de lo descrito, cada árbol de decisión sólo puede elegir el mejor nodo de un subconjunto de los vectores de características, elegido aleatoriamente. Cabe destacar que todos los algoritmos que pertenecen a esta familia de conjuntos, buscan que cada uno de los árboles que lo componen sean lo más diferentes entre sí, a efectos de que el promedio de los valores estimados por cada uno de ellos, se aproxime al valor observado de $y_i$.

La segunda familia de algoritmos de conjuntos, conocida como _Gradient Boosting_, parte de un modelo sencillo y sesgado $f_1(X)$para posteriormente ajustar otro modelo $f_2(X)$ ponderando las observaciones en base a los errores de estimación de cada una de ellas, de acuerdo a una función de pérdida definida. Existen varias implementaciones entre las que se destacan _AdaBoost_, _XGBoost_, _LightGBM_ y _CatBoost_, que principalmente varían sobre la metodología que utilizan para ponderar los errores de las observaciones, así como también en los mecanismos de uso de recursos computacionales. Genéricamente, se los puede definir como:
$$
\begin{aligned}\label{eqn:boosting}
    G(X)=sign(\sum_{m=1}^M\alpha_mG_m(x))
\end{aligned}
$$
donde $\alpha_1, \alpha_2, ..., \alpha_M$ son los pesos estimados por el algoritmo y ponderan la contribución del modelo $G_m(X)$ a la estimación final. Este mecanismo se conoce como “comité”, ya que cada modelo $G_m(X)$ tendrá un voto asociado a su relevancia, de acuerdo a su capacidad de minimizar el error de estimación. Para calcular los $\alpha_m$ del regresor final, cada paso secuencial del algoritmo genera un peso $w_1, w_2, ..., w_N$ a cada una de las observaciones $(x_i, y_i)$ en base al error de estimación de $y_i$. Inicialmente cada observación tiene un peso tal que $w_i=\frac{1}{N}$ y luego se van ponderando las observaciones con mayor error de la manera previamente descrita[^12].


[^12]: Para ver las funciones de pérdida de AdaBoost y sus particularidades, ver @HastieTibshirani2009, capítulo 10.


## Objetivos

### Objetivo General
Analizar y modelar un mismo problema económico desde distintos enfoques estadísticos y de aprendizaje automático a efectos de representar las ventajas y desventajas de cada una de ellas.


### Objetivos expecíficos

1. Desarrollar un marco conceptual para interpretar las implicancias matemáticas y estadísticas de cada enfoque.
2. Realizar un análisis descriptivo del conjunto de datos para exponer el problema en cuestión.
3. Ajustar distintos modelos de regresión lineal multivariada a los datos.
4. Desarrollar distintos modelos de dependencia espacial para analizar la aleatoriedad de la distribución de nuestra variable objetivo.
5. Entrenar modelos de machine learning con el conjunto de datos.
6. Comparar los resultados.


## Metodología

Debido a la accesibilidad y disponibilidad de datos, las estimaciones sobre los precios de los inmuebles se realizarán sobre el stock de inmuebles del mercado, es decir, sobre la oferta actual que existe en la provincia, en vez de las transacciones realizadas. Las distintas instituciones referentes de la industria son un tanto discretas a la hora de compartir información por una sencilla razón: existe una gran disparidad de valores entre el avalúo fiscal y el precio real de mercado, por lo que no quieren dejar expuestos a los clientes que realizaron una transacción inmobiliaria por un monto artificialmente bajo.
A la hora de elegir entre transacciones realizadas y stock inmobiliario existe un _trade-off_ inevitable: certeza de precios vs. representatividad de la oferta inmobiliaria. Las transacciones efectivas proveen certeza del precio por el cual se vendieron los inmuebles, pero pueden ser no representativas del total de inmuebles que se comercializan. Esto puede surgir en situaciones en las que operaciones de ciertos tipos de inmuebles o de ciertos montos no se realicen a través de inmobiliarias, por ejemplo.
En cuanto a la base de datos que se utilizará para estimar el modelo, la misma parte de un relevamiento de inmuebles disponibles a la venta en distintas plataformas de clasificados online que se utilizan en la provincia.
Respecto del método de estimación del modelo, se utilizará el de características, acompañado del modelo SAR ya que en un primer momento nos interesa conocer los precios por zonas y tipos de inmuebles. Finalmente se mostrará el uso de modelos no paramétricos de conjunto, particularmente _Random Forest_ y _Gradient-Boosting_ en su implementación a través de _CatBoost_. La construcción del índice de precios servirá para ajustar dichos valores en el tiempo. La especificación de los modelos paramétricos se realizará en el Capítulo III, mientras que en el Capítulo IV se encontrará la de los métodos de conjuntos.
