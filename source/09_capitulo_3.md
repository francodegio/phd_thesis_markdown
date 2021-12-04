# Especificación y estimación de modelos paramétricos

## Especificación

### Modelo Base

Partiendo de la ecuación \ref{eqn:formastd}, procedemos a definir su forma funcional de la siguiente manera:
$y = Z\beta + \varepsilon$ \label{eq:linreg_matrix}
donde $y$ es un vector de dimensiones $H\times{1}$ de elementos $y-\ln{p_h}$ siendo $p_{h}$ el precio de cada inmueble, $Z$ es una matriz $H\times{C}$ de características del inmueble, $\beta$ es un vector $C\times{1}$ de los precios sombra de las características. En nuestro caso en particular, las características a considerar serán:

- Metros cuadrados de superficie total de la parcela, con el nombre $tsqm$.
- El cuadrado de Metros Cuadrados de superficie total, con el nombre $tsqm2$.
- Metros cuadrados construidos, con el nombre $bsqm$.
- El cuadrado del total de metros cuadrados construidos $bsqm2$.
- Cantidad de Dormitorios $beds$ , como variable continua discreta.
- Cantidad de Baños $baths$, como variable continua discreta.
- Cantidad de cocheras $garages$ , como variable continua discreta.
- Tipo de propiedad, compuesta por las dummies: $Casas$, $Departamentos$ y $Lotes$.
- Localidad del inmueble, compuesta por las dummies: $Chimbas$, $Pocito$, $Rawson$, $Rivadavia$ y $Santa Lucía$.

Con todas las variables construidas, se procedió a ajustar el modelo de regresión por Mínimos Cuadrados Ordinarios, en base a 731 observaciones disponibles en el dataset.

\newpage
\begin{table}
\caption[Modelo Base, regresión de 'precio'.]{\label{table:tabla_1} Regresión por MCO de 'precio', sin geolocalización y con variables contínuas.}
\begin{center}
\begin{tabular}{llll}
\hline
Model:              & OLS              & Adj. R-squared:     & 0.550       \\
Dependent Variable: & precio           & AIC:                & 23062.4828  \\
Date:               & 2021-11-22 18:47 & BIC:                & 23131.3990  \\
No. Observations:   & 731              & Log-Likelihood:     & -11516.     \\
Df Model:           & 14               & F-statistic:        & 64.75       \\
Df Residuals:       & 716              & Prob (F-statistic): & 6.15e-117   \\
R-squared:          & 0.559            & Scale:              & 2.8864e+12  \\
\hline
\end{tabular}
\begin{tabular}{lrrrr}
                                 &         Coef. &    Std.Err. &       t & P$> |$t$|$    \\
\hline
Intercept                        &  1001794.6909 & 489718.7083 &  2.0457 &      0.0412   \\
Departamentos                    &  -577266.1171 & 289089.8384 & -1.9968 &      0.0462   \\
Lotes                            &  1011929.7035 & 480487.5601 &  2.1060 &      0.0355   \\
Chimbas                          & -1913483.3892 & 267030.6478 & -7.1658 &      0.0000   \\
Pocito                           & -2085066.0239 & 369021.5508 & -5.6503 &      0.0000   \\
Rawson                           & -1540117.9699 & 210743.4251 & -7.3080 &      0.0000   \\
Rivadavia                        &  -606423.6150 & 191922.3262 & -3.1597 &      0.0016   \\
Santa Lucia                      & -1084575.4259 & 194445.3649 & -5.5778 &      0.0000   \\
tsqm                             &      256.1682 &     35.9391 &  7.1278 &      0.0000   \\
tsqm2                            &       -0.0028 &      0.0004 & -6.4585 &      0.0000   \\
bsqm                             &    16586.3408 &   4905.4892 &  3.3812 &      0.0008   \\
bsqm2                            &      -14.4830 &      9.9110 & -1.4613 &      0.1444   \\
beds                             &   -80205.9649 & 131241.1543 & -0.6111 &      0.5413   \\
baths                            &  1018250.0355 & 199791.8962 &  5.0966 &      0.0000   \\
garages                          &   552066.7752 & 131700.1101 &  4.1918 &      0.0000   \\
\end{tabular}
\begin{tabular}{llll}
\hline
Omnibus:       & 648.787 & Durbin-Watson:    & 1.878       \\
Prob(Omnibus): & 0.000   & Jarque-Bera (JB): & 33919.168   \\
Skew:          & 3.706   & Prob(JB):         & 0.000       \\
Kurtosis:      & 35.538  & Condition No.:    & 4398056683  \\
\hline
\end{tabular}
\begin{tabular}{l}
\footnotesize Fuente: Elaboración propia a partir de datos de compraensanjuan.com, 2019.
\end{tabular}
\end{center}
\end{table}


En base a los resultados obtenidos de la regresión (ver Tabla \ref{table:tabla_1}, podemos tomar nota de la significancia de las variables, así como también del ajuste general del modelo. Partiendo por éste el $R^2$ es de $0.559$($0.55$ ajustado), es decir, sólo explica dicho porcentaje de la variación de $y$. Respecto de las características incluidas en el modelo, se puede apreciar que hay dos de ellas que no son estadísticamente significativas al $95\%$: una de ellas es la cantidad de dormitorios $beds$, mientras que la otra es el cuadrado de la cantidad de metros cuadrados construidos $bsqm2$.

En ambos casos, esto podría sugerir que, en caso de que estas variables tengan información significativa -lo cual es bastante probable-, el tipo de relación no es lineal, por lo cual lo ideal sería tratar a beds como una variable categórica y crear las dummies correspondientes, mientras que la interpretación de $bsqm2$, podría sugerir que no es significativa respecto de la variable objetivo precio, pero sí quizás respecto de una transformación no lineal de la misma. En definitiva, en ambos casos nos encontraríamos con supuestos de no linealidad. Alternativamente, $bsqm2$ podría verse influenciada por la magnitud de la unidad de medida de precio que se cuenta expresada en pesos a Junio de 2019.


### Modelo Base con transformación logarítmica

Con la intención de capturar la variación porcentual en la variable dependiente, se puede realizar una transformación logarítmica sobre la misma. Realizar dicha transformación implica que la interpretación de los betas de la regresión pasan a estar medidos en puntos porcentuales de variación respecto de la variable objetivo, así como también introducimos no-linealidad en el modelo.
En efecto, en la Tabla \ref{table:tabla_2} se pueden observar los nuevos valores de los betas, así como también el hecho de que ahora $bsqm2$ es estadísticamente significativa con un $\alpha=5\%$, tal como se postuló en la sección anterior. Adicionalmente puede observarse que el $R^{2}$ ajustado aumentó a $0.632$.
Respecto de la interpretación de los coeficientes, por ejemplo, el beta asociado a la localidad de Rivadavia Rivadavia, en la regresión con la variable precio toma un valor de -60,6423.615, es decir en promedio 60,6423.615 menos que un inmueble de Capital. Mientras que en el segundo caso, toma un valor de -0.2021, es decir un 20% menos en promedio.
Nuevamente se puede observar que hay gran disparidad entre los coeficientes, lo cual tiene que ver con las magnitudes de medida de cada una de las variables. Esto también explica el valor extremo que toma el Número de Condición ($4398056683$).

\newpage
\begin{table}
\caption[Modelo Base, regresión de 'lprice'.]{\label{table:tabla_2}Regresión por MCO de 'lprice', sin geolocalización y con variables contínuas.}
\begin{center}
\begin{tabular}{llll}
\hline
Model:              & OLS              & Adj. R-squared:     & 0.632      \\
Dependent Variable: & lprice           & AIC:                & 1404.5282  \\
Date:               & 2021-11-22 17:57 & BIC:                & 1473.4444  \\
No. Observations:   & 731              & Log-Likelihood:     & -687.26    \\
Df Model:           & 14               & F-statistic:        & 90.70      \\
Df Residuals:       & 716              & Prob (F-statistic): & 5.41e-148  \\
R-squared:          & 0.639            & Scale:              & 0.39188    \\
\hline
\end{tabular}
\begin{tabular}{lrrrr}
                                 &   Coef. & Std.Err. &        t & P$> |$t$|$   \\
\hline
Intercept                        & 14.3944 &   0.1804 &  79.7719 &      0.0000  \\
Departamentos                    & -0.2928 &   0.1065 &  -2.7486 &      0.0061  \\
Lotes                            & -0.4387 &   0.1770 &  -2.4782 &      0.0134  \\
Chimbas                          & -1.1291 &   0.0984 & -11.4755 &      0.0000  \\
Pocito                           & -1.0620 &   0.1360 &  -7.8104 &      0.0000  \\
Rawson                           & -0.6674 &   0.0777 &  -8.5948 &      0.0000  \\
Rivadavia                        & -0.2021 &   0.0707 &  -2.8583 &      0.0044  \\
Santa Lucia                      & -0.5513 &   0.0716 &  -7.6954 &      0.0000  \\
tsqm                             &  0.0001 &   0.0000 &   9.3906 &      0.0000  \\
tsqm2                            & -0.0000 &   0.0000 &  -8.7855 &      0.0000  \\
bsqm                             &  0.0050 &   0.0018 &   2.7501 &      0.0061  \\
bsqm2                            & -0.0000 &   0.0000 &  -2.1373 &      0.0329  \\
beds                             &  0.0130 &   0.0484 &   0.2686 &      0.7883  \\
baths                            &  0.2031 &   0.0736 &   2.7588 &      0.0059  \\
garages                          &  0.1065 &   0.0485 &   2.1946 &      0.0285  \\
\end{tabular}
\begin{tabular}{llll}
\hline
Omnibus:       & 82.214 & Durbin-Watson:    & 1.701       \\
Prob(Omnibus): & 0.000  & Jarque-Bera (JB): & 627.788     \\
Skew:          & -0.075 & Prob(JB):         & 0.000       \\
Kurtosis:      & 7.537  & Condition No.:    & 4398056683  \\
\hline
\end{tabular}
\begin{tabular}{l}
\footnotesize Fuente: Elaboración propia a partir de datos de compraensanjuan.com, 2019.
\end{tabular}
\end{center}
\end{table}


## Modelos modificados
### Precio por metro cuadrado (pm2) como variable dependiente

Una alternativa bastante interesante a analizar, consiste en tratar de estimar el precio del metro cuadrado $pm2$ del inmueble, en vez del precio total del mismo, es decir:
$pm2 = \frac{precio}{tsqm}$ {#eq:pm2}

En la Tabla \ref{table:tabla_3} se pueden observar los resultados de utilizar pm2 como variable dependiente. En la misma se destaca que variables que previamente eran significativas al 95%, ahora no lo son, tal es el caso de $tsqm$, $tsqm2$ y $garages$, mientras que otras variables exógenas como $bsqm2$ o $beds$ ahora sí lo son, al 10% y al 5%, respectivamente.
El hecho de que  $tsqm$ y $tsqm2$ no sean estadísticamente significativas en este esquema tiene sentido ya que lo más probable es que no se trate de una relación lineal o cuadrática, ya que la misma variable $tsqm$ se utilizó para transformar el precio y construir $pm2$. Con este resultado se puede concluir que no se está filtrando información al modelo.

En cuanto al poder descriptivo del modelo, podemos ver una mejora considerable en tanto en el valor de $R^2$ como en $R^2 ajustado$, que aumentan de 0.559 y 0.55a 0.708 y 0.702, respectivamente; es decir, se observa una mejora de $26,5p.p.$ y de $27,6p.p.$ en cada caso.


\newpage
\begin{table}
\caption[Modelo modificado, regresión de 'pm2'.]{\label{table:tabla_3}Regresión por MCO de 'pm2', sin geolocalización y con variables contínuas.}
\begin{center}
\begin{tabular}{llll}
\hline
Model:              & OLS              & Adj. R-squared:     & 0.702       \\
Dependent Variable: & pm2              & AIC:                & 15213.7604  \\
Date:               & 2021-11-22 19:13 & BIC:                & 15282.6766  \\
No. Observations:   & 731              & Log-Likelihood:     & -7591.9     \\
Df Model:           & 14               & F-statistic:        & 123.8       \\
Df Residuals:       & 716              & Prob (F-statistic): & 2.62e-180   \\
R-squared:          & 0.708            & Scale:              & 6.2713e+07  \\
\hline
\end{tabular}
\end{center}

\begin{center}
\begin{tabular}{lrrrr}
\hline
                                 &       Coef. &  Std.Err. &       t & P$> |$t$|$   \\
\hline
Intercept                        &  21682.9033 & 2282.6749 &  9.4989 &      0.0000  \\
Departamentos                    &  18548.9113 & 1347.5044 & 13.7654 &      0.0000  \\
Lotes                            & -12486.8167 & 2239.6467 & -5.5754 &      0.0000  \\
Chimbas                          &  -9554.3840 & 1244.6822 & -7.6762 &      0.0000  \\
Pocito                           &  -8616.4696 & 1720.0818 & -5.0093 &      0.0000  \\
Rawson                           &  -8563.9327 &  982.3164 & -8.7181 &      0.0000  \\
Rivadavia                        &  -6225.0700 &  894.5876 & -6.9586 &      0.0000  \\
Santa Lucia                      &  -8207.5256 &  906.3480 & -9.0556 &      0.0000  \\
tsqm                             &     -0.1454 &    0.1675 & -0.8677 &      0.3858  \\
tsqm2                            &      0.0000 &    0.0000 &  0.7724 &      0.4401  \\
bsqm                             &    -64.5413 &   22.8654 & -2.8227 &      0.0049  \\
bsqm2                            &      0.0790 &    0.0462 &  1.7108 &      0.0876  \\
garages                          &    407.6286 &  613.8800 &  0.6640 &      0.5069  \\
baths                            &   5738.3234 &  931.2692 &  6.1618 &      0.0000  \\
beds                             &  -1870.8038 &  611.7407 & -3.0582 &      0.0023  \\
\hline
\end{tabular}
\end{center}

\begin{center}
\begin{tabular}{llll}
\hline
Omnibus:       & 461.609 & Durbin-Watson:    & 1.889       \\
Prob(Omnibus): & 0.000   & Jarque-Bera (JB): & 7929.654    \\
Skew:          & 2.531   & Prob(JB):         & 0.000       \\
Kurtosis:      & 18.321  & Condition No.:    & 4398056683  \\
\hline
\end{tabular}
\begin{tabular}{l}
\footnotesize Fuente: Elaboración propia a partir de datos de compraensanjuan.com, 2019.
\end{tabular}
\end{center}
\end{table}


### Logaritmo del precio por metro cuadrado lpm2 como variable dependiente.

En línea con la sección anterior, en ésta se busca modelar el logaritmo del precio por metro cuadrado -$lpm2$-  a partir de las mismas variables exógenas que en los casos anteriores. Como se propuso previamente, el hecho que las variables $tsqm$ y su cuadrado no sean estadísticamente significativas, da lugar a sospechar que puede deberse a una relación de no linealidad.
Tal como se observa en la \ref{table:tabla_4}, las variables $tsqm$ y $tsqm2$ pasan a ser estadísticamente significativas aunque con un coeficiente muy cercano a cero, debido a la unidad de medida de la misma, al punto que $tsqm2$ es redondeada a cero por la librería _statsmodels_. Si bien, se observa que la cantidad de dormitorios -$beds$- deja de ser estadísticamente significativa la razón es relativamente sencilla: aparentemente tanto la cantidad de metros -$tsqm$-, como su transformación cuadrática -$tsqm2$-, contienen la misma información que la variable dormitorios -$beds$.

\newpage
\begin{table}
\caption[Modelo modificado, regresión de 'lpm2'.]{\label{table:tabla_4}Regresión por MCO de 'lpm2', sin geolocalización y con variables contínuas.}
\begin{center}
\begin{tabular}{llll}
\hline
Model:              & OLS              & Adj. R-squared:     & 0.819      \\
Dependent Variable: & lpm2             & AIC:                & 1503.7073  \\
No. Observations:   & 731              & BIC:                & 1572.6235  \\
Df Model:           & 14               & Log-Likelihood:     & -736.85    \\
Df Residuals:       & 716              & F-statistic:        & 237.3      \\
R-squared:          & 0.823            & Prob (F-statistic): & 1.06e-257  \\
\hline
\end{tabular}
\begin{tabular}{lrrrr}
\hline
                                 &   Coef. & Std.Err. &        t & P$> |$t$|$   \\
\hline
Intercept                        &  9.6328 &   0.1931 &  49.8825 &      0.0000  \\
PropertyTypeDepartamentos] &  0.8936 &   0.1140 &   7.8388 &      0.0000  \\
PropertyTypeLotes]         & -1.5692 &   0.1895 &  -8.2823 &      0.0000  \\
Chimbas]           & -1.1213 &   0.1053 & -10.6489 &      0.0000  \\
Pocito]            & -1.4413 &   0.1455 &  -9.9045 &      0.0000  \\
Rawson]            & -0.9767 &   0.0831 & -11.7530 &      0.0000  \\
Rivadavia]         & -0.2931 &   0.0757 &  -3.8735 &      0.0001  \\
Santa Lucia]       & -1.0327 &   0.0767 & -13.4679 &      0.0000  \\
tsqm                             & -0.0001 &   0.0000 & -10.1971 &      0.0000  \\
tsqm2                            &  0.0000 &   0.0000 &   5.3335 &      0.0000  \\
bsqm                             & -0.0019 &   0.0019 &  -0.9649 &      0.3349  \\
bsqm2                            &  0.0000 &   0.0000 &   0.4166 &      0.6771  \\
garages                          &  0.0559 &   0.0519 &   1.0771 &      0.2818  \\
baths                            &  0.2353 &   0.0788 &   2.9864 &      0.0029  \\
beds                             & -0.0203 &   0.0518 &  -0.3930 &      0.6945  \\
\hline
\end{tabular}
\end{center}

\begin{center}
\begin{tabular}{llll}
\hline
Omnibus:       & 0.453 & Durbin-Watson:    & 1.539       \\
Prob(Omnibus): & 0.797 & Jarque-Bera (JB): & 0.318       \\
Skew:          & 0.021 & Prob(JB):         & 0.853       \\
Kurtosis:      & 3.093 & Condition No.:    & 4398056683  \\
\hline
\end{tabular}
\begin{tabular}{l}
\footnotesize Fuente: Elaboración propia a partir de datos de compraensanjuan.com, 2019.
\end{tabular}
\end{center}
\end{table}


### Estableciendo relación no lineal con variables discretas y utilizando $lpm2$ como variable dependiente.
En base a los resultados obtenidos y las mejoras observadas, en esta sección se procederá a combinar todas las transformaciones previamente realizadas, así como también una relación no lineal con las variables discretas como la cantidad de baños (baths), dormitorios (beds) y cocheras (garages). De esta manera, se generarán $n-1$ variables _dummy_ por cada variable discreta.

Debido a la gran cantidad de variables dicotómicas, cabe destacar que el intercepto contiene los efectos de las dummies bed0, bath0,  garages0, Capital y  Casas, que fueron eliminadas para evitar multicolinealidad.
Los resultados de dicho modelo se encuentran en la Tabla \ref{table:tabla_5}, donde se puede observar claramente que el $R^2$ aumentó a 0.843 (0.837). Debido a que la mayoría de las variables exógenas son estadísticamente significativas, se detalla a continuación las que no lo son:

- En el caso de las variables $garages1$, $garages3$, $garages4$, $garages6$ y $garages8$ no se puede rechazar la hipótesis nula de que sean estadísticamente distintos de cero, a diferencia de $garages2$ que sí lo es, con un coeficiente de $0.3974 (0.1397 s.e.)$ y un valor $t=2.8457$. Esto podría indicar la posibilidad de una interacción entre garages y el tipo de inmueble $PropertyType$ y/o la localidad $$.
- $bsqm2$ tampoco es significativa, lo que sugiere que la relación entre $bsqm$ y el logaritmo del precio por metro cuadrado es simplemente lineal.
- $Departamentos$ no resultó ser estadísticamente significativa y esto puede deberse a que comparta información (es decir, que tenga correlación) con otras variables, como pueden ser $b_ratio$, $bed1$ y $bath1$.


\newpage
\begin{table}
\caption[Modelo modificado, regresión de 'lpm2' y dummies discretas.]{\label{table:tabla_5}Regresión por MCO de 'lpm2', sin geolocalización y con variables dummies.}
\begin{center}
\begin{tabular}{llll}
\hline
Model:              & OLS              & Adj. R-squared:     & 0.837      \\
Dependent Variable: & lpm2             & AIC:                & 1437.6488  \\
Date:               & 2021-11-22 19:37 & BIC:                & 1561.6980  \\
No. Observations:   & 731              & Log-Likelihood:     & -691.82    \\
Df Model:           & 26               & F-statistic:        & 145.7      \\
Df Residuals:       & 704              & Prob (F-statistic): & 6.38e-263  \\
R-squared:          & 0.843            & Scale:              & 0.40356    \\
\hline
\end{tabular}
\begin{tabular}{lrrrr}
\hline
                                 &   Coef. & Std.Err. &        t & P$> |$t$|$   \\
\hline
Intercept                        &  7.1121 &   0.1727 &  41.1929 &      0.0000   \\
garages1                         &  0.0899 &   0.1060 &   0.8482 &      0.3966   \\
garages2                         &  0.3974 &   0.1397 &   2.8457 &      0.0046   \\
garages3                         & -0.0320 &   0.2395 &  -0.1338 &      0.8936   \\
garages4                         &  1.0171 &   0.6911 &   1.4717 &      0.1416   \\
garages6                         &  0.5298 &   0.6773 &   0.7822 &      0.4344   \\
garages8                         &  0.9300 &   0.7789 &   1.1940 &      0.2329   \\
baths1                           &  0.8429 &   0.1458 &   5.7795 &      0.0000   \\
baths2                           &  1.1393 &   0.1422 &   8.0121 &      0.0000   \\
baths3                           &  1.4565 &   0.1803 &   8.0793 &      0.0000   \\
baths4                           &  1.3351 &   0.3223 &   4.1423 &      0.0000   \\
baths5                           &  1.4589 &   0.5930 &   2.4603 &      0.0141   \\
beds1                            &  1.3075 &   0.1262 &  10.3562 &      0.0000   \\
beds2                            &  1.4396 &   0.1095 &  13.1425 &      0.0000   \\
beds3                            &  1.2795 &   0.1039 &  12.3140 &      0.0000   \\
beds4                            &  1.2425 &   0.1360 &   9.1388 &      0.0000   \\
beds5                            &  0.9637 &   0.1988 &   4.8476 &      0.0000   \\
Departamentos                    & -0.0625 &   0.1682 &  -0.3719 &      0.7101   \\
Lotes                            &  0.8794 &   0.1698 &   5.1782 &      0.0000   \\
Chimbas                          & -1.0453 &   0.1015 & -10.3018 &      0.0000   \\
Pocito                           & -1.3657 &   0.1394 &  -9.8001 &      0.0000   \\
Rawson                           & -0.9187 &   0.0809 & -11.3544 &      0.0000   \\
Rivadavia                        & -0.2452 &   0.0748 &  -3.2763 &      0.0011   \\
Santa Lucia                      & -0.9298 &   0.0755 & -12.3075 &      0.0000   \\
tsqm                             & -0.0001 &   0.0000 & -10.3293 &      0.0000   \\
tsqm2                            &  0.0000 &   0.0000 &   5.2424 &      0.0000   \\
bsqm                             & -0.0044 &   0.0020 &  -2.1585 &      0.0312   \\
bsqm2                            &  0.0000 &   0.0000 &   0.5386 &      0.5904   \\
b\_ratio                         &  1.6997 &   0.1983 &   8.5730 &      0.0000   \\
\hline
\end{tabular}
\begin{tabular}{llll}
\hline
Omnibus:       & 1.272  & Durbin-Watson:    & 1.485              \\
Prob(Omnibus): & 0.529  & Jarque-Bera (JB): & 1.142              \\
Skew:          & -0.024 & Prob(JB):         & 0.565              \\
Kurtosis:      & 3.188  & Condition No.:    & 10024944593774462  \\
\hline
\end{tabular}
\begin{tabular}{l}
\footnotesize Fuente: Elaboración propia a partir de datos de compraensanjuan.com, 2019.
\end{tabular}
\end{center}
\end{table}


## Modelo Spatial Lag o SAR (Simultaneous Autoregressive-Regressive)

Como se anticipó previamente, el modelo de Spatial Lag, considera que la información de las observaciones vecinas aporta información a la hora de construir el precio del inmueble en cuestión. Esto significa que el precio del inmueble _i_ se construye, en parte, gracias a las características del inmueble _j_ –es decir $WX_j$–, gracias al precio de dicho inmueble –es decir $WY_j$–, o bien en los residuos del modelo –es decir $W_u$–, donde $W$ es la matriz de pesos espaciales. Cabe destacar que dicha matriz es calculada en base a la distancia entre una observación y sus vecinos, normalizada a 1, ya sea dividiendo las filas para que tomen dicho valor o las columnas.
Nótese que el motivo por el cual se incorpora la matriz de pesos espaciales al cálculo, tiene que ver con la limitación que se encuentra a la hora de estimar las interacciones de los parámetros de las observaciones: no es posible calcular todas las interacciones para cada uno de los parámetros ya que existen $n^2$ parámetros para $n$ observaciones. De esta manera, el problema se restringe a un único parámetro de dependencia espacial ($\lambda$) y se calcula sólo a partir de las observaciones vecinas.

### Matriz de pesos espaciales
De acuerdo a la especificación modelo, la forma de incorporación del concepto de vecindad de dos observaciones es representada por la matriz de pesos espaciales. Para poder estimarla, se procedió a la creación de polígonos de Voronoi y posteriormente se calculó la contigüidad a partir del criterio de Reina (Queen), ya que sólo se consideró relevante que sean o no vecinas las distintas observaciones. Una vez estimada y normalizada la matriz de pesos espaciales, se procedió al ajuste del modelo SAR por el método de máxima verosimilitud.

### SAR por método de Máxima Verosimilitud
De acuerdo al ajuste del modelo realizado con esta metodología, se puede observar en la Tabla \ref{table:table_6} que la cantidad de dormitorios presenta una interacción no lineal estadísticamente significativa, ya que la variable beds_sq obtuvo un coeficiente de $-0.3653(s.e. 0.0526)$, mientras que la variable beds obtuvo un beta de $0.5669 (s.e. 0.1786)$. Con respecto a la cantidad de cocheras, si bien son estadísticamente significativas, el tipo de relación que se observa es lineal, ya que sólo fue significativa garages con un coeficiente de $0.1711 (s.e. 0.0526)$, a diferencia de $garages_2$ que no presentó un coeficiente significativo. 

En cuanto a las variables de mensura, se puede observar una relación cuadrática con respecto a la cantidad de metros cuadrados totales al cuadrado, $tsqm_2$, con coeficiente $0.2499 (s.e. 0.0498)$ y coeficiente $-0.6147 (s.e. 0.0498)$ para la cantidad de metros cuadrados totales, es decir, $tsqm$. Con respecto a la cantidad de metros cuadrados construidos, se observa una relación lineal con un coeficiente significativo para $bsqm$ de valor $-0.2896(s.e. 0.1129)$, pero con coeficiente cuadrático no significativo. También se observó un coeficiente estadísticamente significativo para la variable de proporción de construcción $b_ratio$ con un $\beta$ de $0.5501 (s.e. 0.0401)$.

Finalmente, de acuerdo a las restricciones impuestas, podemos decir que existe autocorrelación espacial positiva respecto de los precios de las otras observaciones. Dicho efecto está capturado por $W\_lpm2$ y presenta un coeficiente de $0.4868 (s.e. 0.0193)$. Este modelo nos permite estimar un $pseudo-R^2$ de $0.8923$ y un $pseudo-R^2 espacial$ de $0.8465$. 



\newpage
\begin{table}
\caption[Modelo SAR.]{\label{table:table_6}Modelo SAR con matriz de pesos espaciales de contiguidad de Queen.}
\begin{center}
\begin{tabular}{llll}
\hline
Model:              & OLS              & Presudo R-squared:         & 0.8923    \\
Dependent Variable: & lpm2             & Spatial Pseudo R-squared:  & 0.8465    \\
Mean dependent var  & 8.1251           & AIC:                       & 1173.428  \\
S.D. dependent var  & 1.5757           & Log-Likelihood:            & -572.714  \\
Df Model:           & 14               & Schwarz criterion:         & 1237.750  \\
Df Residuals:       & 717              & S.E of regression:         & 0.517     \\
Sigma-square ML     & 0.267            &                            &           \\
\hline
\end{tabular}
\begin{tabular}{lrrrr}
\hline
                                 &   Coef. & Std.Err. &        z & P$> |$t$|$ \\
\hline
Intercept                        &  4.1704 &   0.1581 &  26.3753 &    0.0000  \\
beds                             &  0.5669 &   0.1786 &   3.1739 &    0.0015  \\
baths                            &  0.2896 &   0.2196 &   1.3183 &    0.1873  \\
garages                          &  0.1711 &   0.0526 &   3.2479 &    0.0011  \\
tsqm                             & -0.2868 &   0.1129 &  -2.5409 &    0.0110  \\
bsqm                             & -0.6147 &   0.0502 & -12.2406 &    0.0000  \\
tsqm2                            &  0.2499 &   0.0498 &   5.0085 &    0.0000  \\
bsqm2                            &  0.0328 &   0.0715 &   0.4585 &    0.6465  \\
b\_ratio                         &  0.5501 &   0.0401 &  13.7072 &    0.0000  \\
bed\_bath\_ratio                 &  0.1134 &   0.0780 &   1.4535 &    0.1460  \\
beds\_sq                         & -0.3653 &   0.1100 &  -3.3196 &    0.0009  \\
baths\_sq                        & -0.1214 &   0.1074 &  -1.1307 &    0.2581  \\
garages\_sq                      & -0.0385 &   0.0403 &  -0.9566 &    0.3387  \\
W\_lpm2                          &  0.4868 &   0.0193 &  25.1787 &    0.0000  \\
\hline
\end{tabular}
\begin{tabular}{l}
\footnotesize Fuente: Elaboración propia a partir de datos de compraensanjuan.com, 2019.
\end{tabular}
\end{center}
\end{table}


### Modelo SLX
Esta versión es considerada la más simple y propone que el precio de un inmueble depende no sólo de sus características, sino también de las características de los inmuebles vecinos. La forma matemática es bastante sencilla y se define como:
$\log{pm2}_i = \beta_0 + \beta\times{X_i} + \rho W\times{X_j} + \varepsilon$ {#eq:eq_slx}

El vector de parámetros $\rho$ es el que tendremos que evaluar si es significativo o no, con las mismas pruebas de significancia establecidos para la estimación de los betas. De esta manera, se evita establecer un modelo endógeno como el observado en el modelo SAR, lo que nos da flexibilidad a la hora de realizar predicciones, tiende a reducir la capacidad descriptiva de la varianza observada en la variable objetivo.

Como se puede observar en la Tabla \ref{table:tabla_7}, incluir información de las características de los inmuebles vecinos no aporta mucho a la descripción de la variación de la variable objetivo. El $R^2$ pasó de $0.843 (0.837)$ a $0.875 (0.865)$ incluyendo la dimensión espacial. No sólo eso, sino que se puede apreciar la penalización sobre el $R^2-Ajustado$, ya que si bien el modelo había explicado $0.5 p.p.$ más, en base a la cantidad de variables no significativas incluidas, el $R^2$ Ajustado aumentó sólo $0.28 p.p.$ 

Entre las nuevas variables incluidas, podemos observar que las únicas estadísticamente significativas para un α de 5%  son: $tsqm$, $tsqm2$, $bsqm$, $b\_ratio$, $Chimbas$, $Pocito$, $Rawson$, $Rivadavia$, $car\_2$, $bath\_3$, $Lotes$, $lag\_car\_3$, $lag\_car\_4$, $lag\_bed\_5$, $lag\_bath\_3$, $lag\_bath\_5$, $lag\_Lotes$, $lag\_tsqm$, $lag\_Rawson$, $lag\_Santa Lucia$ y $lag\_tsqm2$. El resto de las variables no son estadísticamente significativas.


\newpage
\begin{table}
\caption[Modelo SLX.]{\label{table:tabla_7}Modelo SAR con matriz de pesos espaciales de contiguidad de Queen.}
\begin{center}
\begin{tabular}{llll}
\hline
Model:              & OLS              & Adj. R-squared:     & 0.865      \\
Dependent Variable: & y                & AIC:                & 1328.8715  \\
No. Observations:   & 731              & BIC:                & 1581.5642  \\
Df Model:           & 54               & Log-Likelihood:     & -609.44    \\
Df Residuals:       & 676              & F-statistic:        & 87.53      \\
R-squared:          & 0.875            & Prob (F-statistic): & 9.47e-268  \\ 
\hline
\end{tabular}
\begin{tabular}{lrrrr}
\hline
                      &   Coef. & Std.Err. &        t & P$> |$t$|$  \\
\hline
Intercept             & 14.3454 &   2.0783 &   6.9025 &      0.0000 \\
bsqm                  & -0.3289 &   0.1404 &  -2.3431 &      0.0194 \\
Departamentos         & -0.0326 &   0.1606 &  -0.2030 &      0.8392 \\
Lotes                 & -2.3713 &   1.0400 &  -2.2801 &      0.0229 \\
Chimbas               & -0.6160 &   0.1993 &  -3.0917 &      0.0021 \\
Pocito                & -0.7010 &   0.2444 &  -2.8675 &      0.0043 \\
Rawson                & -0.2926 &   0.1474 &  -1.9849 &      0.0476 \\
Rivadavia             & -0.3222 &   0.1465 &  -2.1998 &      0.0282 \\
Santa Lucia           & -0.1418 &   0.1511 &  -0.9385 &      0.3483 \\
car\_2                &  0.2347 &   0.1015 &   2.3113 &      0.0211 \\
car\_3                &  0.0185 &   0.2089 &   0.0884 &      0.9296 \\
car\_4                &  0.8340 &   0.6267 &   1.3308 &      0.1837 \\
car\_6                &  0.6994 &   0.6184 &   1.1310 &      0.2584 \\
car\_0                & -0.1656 &   0.0999 &  -1.6576 &      0.0979 \\
car\_8                &  0.5178 &   0.7226 &   0.7165 &      0.4739 \\
bath\_2               &  0.6697 &   0.3760 &   1.7810 &      0.0754 \\
bath\_3               &  1.4979 &   0.6535 &   2.2920 &      0.0222 \\
bath\_4               &  1.7573 &   1.0432 &   1.6846 &      0.0925 \\
bath\_5               &  1.8335 &   1.1847 &   1.5476 &      0.1222 \\
bed\_2                & -0.4402 &   0.5099 &  -0.8633 &      0.3883 \\
bed\_3                & -0.8439 &   0.7074 &  -1.1930 &      0.2333 \\
bed\_4                & -1.1551 &   0.8804 &  -1.3119 &      0.1900 \\
bed\_5                & -1.5304 &   1.0217 &  -1.4980 &      0.1346 \\
tsqm                  & -0.6292 &   0.0578 & -10.8875 &      0.0000 \\
tsqm2                 &  0.2714 &   0.0571 &   4.7558 &      0.0000 \\
bsqm2                 &  0.0340 &   0.0902 &   0.3764 &      0.7067 \\
b\_ratio              &  0.5557 &   0.0685 &   8.1153 &      0.0000 \\
bed\_bath\_ratio      & -0.4280 &   0.3466 &  -1.2348 &      0.2173 \\
lag\_bsqm             &  0.2011 &   0.3718 &   0.5407 &      0.5889 \\
lag\_Departamentos    &  0.4222 &   0.3460 &   1.2204 &      0.2228 \\
lag\_Lotes            & -5.3339 &   2.6435 &  -2.0178 &      0.0440 \\
\end{tabular}
\end{center}
\end{table}

\newpage
\begin{table}
\begin{center}
\begin{tabular}{lrrrr}
\hline
lag\_Chimbas          & -0.3095 &   0.2416 &  -1.2813 &      0.2005 \\
lag\_Rawson           & -0.5984 &   0.1793 &  -3.3369 &      0.0009 \\
lag\_Rivadavia        &  0.1296 &   0.1725 &   0.7515 &      0.4526 \\
lag\_Santa Lucia      & -0.7262 &   0.1803 &  -4.0283 &      0.0001 \\
lag\_car\_2           & -0.0332 &   0.2459 &  -0.1349 &      0.8927 \\
lag\_car\_3           & -1.1801 &   0.4796 &  -2.4604 &      0.0141 \\
lag\_car\_4           &  3.9414 &   1.8787 &   2.0979 &      0.0363 \\
lag\_car\_6           & -0.9383 &   1.4885 &  -0.6304 &      0.5287 \\
lag\_car\_0           & -0.1251 &   0.2492 &  -0.5021 &      0.6158 \\
lag\_car\_8           & -1.4180 &   2.2695 &  -0.6248 &      0.5323 \\
lag\_bath\_2          &  1.7141 &   0.9111 &   1.8814 &      0.0603 \\
lag\_bath\_3          &  3.4931 &   1.6432 &   2.1258 &      0.0339 \\
lag\_bath\_4          &  5.0116 &   2.6347 &   1.9022 &      0.0576 \\
lag\_bath\_5          &  6.2346 &   3.0167 &   2.0667 &      0.0391 \\
lag\_bed\_2           & -2.2368 &   1.2754 &  -1.7538 &      0.0799 \\
lag\_bed\_3           & -3.3610 &   1.7693 &  -1.8997 &      0.0579 \\
lag\_bed\_4           & -4.0964 &   2.2018 &  -1.8605 &      0.0633 \\
lag\_bed\_5           & -6.2597 &   2.5554 &  -2.4496 &      0.0146 \\
lag\_tsqm             & -0.4907 &   0.1192 &  -4.1183 &      0.0000 \\
lag\_tsqm2            &  0.4642 &   0.1236 &   3.7557 &      0.0002 \\
lag\_bsqm2            &  0.0028 &   0.2560 &   0.0109 &      0.9913 \\
lag\_b\_ratio         & -0.1458 &   0.1505 &  -0.9689 &      0.3330 \\
lag\_bed\_bath\_ratio & -1.6478 &   0.8656 &  -1.9036 &      0.0574 \\
\hline
\end{tabular}
\begin{tabular}{l}
\footnotesize Fuente: Elaboración propia a partir de datos de compraensanjuan.com, 2019.
\end{tabular}
\end{center}
\end{table}
\normalsize

  