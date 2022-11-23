---
title: 'Modelo de crecimiento exponencial en ecología'
date: 2022-11-22
permalink: /posts/2022/11/exp-growth/
code-block-font-size: \large
tags:
  - exponencial
  - modelos
  - ecología
  - poblaciones
---

# Contar organismos puede ser complejo

Muchas veces el problema de contar organismos en una población puede ser de gran dificultad, en el caso de organismos que podemos identificar como unitarios puede resultar de una menor dificultad, pero en los organismos modulares, como corales, árboles u otros este problema puede ser de gran relevancia.
También al momento de contar organismos y querer proyectar la población hacia el futuro, podemos encontrarnos con el problema de que no todos los organismos son de una misma categoría, pudieran existir organismos que nunca llegarán a reproducirse u organismos que ya han pasado su etapa reproductiva.
Sin embargo, a pesar de todos estos puntos a tomar en consideración se han podido generar modelos que buscan describir la dinámica de las poblaciones. En este ejemplo hablaremos del modelo de crecimiento exponencial, un modelo independiente de la densidad de la población o denso-independiente. Para ilustrarlo pensaremos en paramecios. 

# Un modelo inicial

Teniendo en cuenta nuestra población de paramecios esta podría crecer debido a que estos se reproduzcan o a que lleguen paramecios de otra población, es decir de una tasa de natalidad ($B$) y de inmigración ($I$). Por otro lado, disminuiría nuestro número de paramecios si algunos de estos mueren o si emigran a otra población, lo que podríamos considerar una tasa de mortalidad ($D$) y una tasa de emigración ($E$), por lo que el número de individuos en un tiempo futuro, que podemos denotar como $t+1$, de una población estaría dado por lo siguiente:

$$N_{t+1} = N0 + B – D + I – E$$

Es decir, del número inicial de organismos, cuantos nacen, cuantos mueren, cuentos inmigran y emigran. Para obtener el número de individuos en el que cambia la población entre un tiempo y otro, podemos escribir lo siguiente:

$$\Delta N = B – D + I – E$$

Ahora supongamos que tenemos una población cerrada, es decir, no es posible emigrar de ella o que lleguen inmigrantes, por lo que nuestra ecuación queda reducida a:

$$\Delta N = B – D$$

Es decir, el número de individuos sólo depende de cuantos nacen y mueren.
Si consideramos que nuestra población de paramecios crece de forma continua, es decir que no lo hace en saltos temporales, podemos escribir la ecuación de forma diferencial:

$$ \frac{\delta N}{\delta t} = B - D$$

Donde podemos apreciar el cambio infinitesimal o muy pequeño del tamaño de la población, en un tiempo infinitesimal o muy pequeño.

Si nos centramos en las tasas de natalidad y mortalidad, vemos que ambas dependen del número de individuos de la población, por ejemplo, mientras más individuos tenga la población mayor cantidad de individuos nacerán, lo mismo ocurre con la mortalidad, por lo que podemos definir $B$ y $D$ cada una con una tasa de natalidad y mortalidad percápita:

$$B = bN0$$
y
$$D = dN0$$

Esto dejaría nuestra ecuación de la siguiente forma:

$$ \frac{\delta N}{\delta t} = (b-d)*N0$$

Finalmente, al factor $b – d$ podemos llamarlo $r$ o tasa de crecimiento poblacional instantánea.

$$ \frac{\delta N}{\delta t} = r*N0$$

Esta sería nuestra primera ecuación que nos indica un crecimiento exponencial de la población. Podemos notar que a valores de r superiores a 0 la población crece en el tiempo, a valores menores decrece y a valor igual a 0 se mantiene su tamaño.

Para obtener una ecuación que nos permita proyectar la población hacia el futuro podemos integrar la ecuación anterior siguiendo las reglas del cálculo, lo que nos daría como resultado lo siguiente:

$$N_{t} = N0e^{rt}$$

Esta ecuación nos permite proyectar a un tiempo futuro nuestra población de paramecios teniendo en cuenta cuantos paramecios hay en este momento y su tasa de crecimiento instantánea.

# Elementos importantes a considerar
Como ya hemos mencionado anteriormente el tamaño inicial de la población como la tasa de crecimiento determinan el número de individuos en el futuro cambiando estos parámetros podemos tener distintos tipos de curvas. Pero imagina un valor de $r$ ligeramente superior a 0 para nuestros paramecios, algo como 0.1 y proyectamos el tiempo en días e iniciando sólo con 10 paramecios. En 1 día tendríamos un nuevo paramecio, en 2 días tendríamos 2 nuevos paramecios, en 3 días, 3 nuevos y así hasta llegar al sexto día con 8 paramecios nuevos y un total de 18 ¿Bastante lento no? Si seguimos proyectando el tamaño poblacional en un mes de 30 días tendríamos 200 paramecios, pero en 100 días tendríamos más de 220 mil paramecios. Imagina que estos sólo se originaron de 10 paramecios iniciales, por lo que si una población de una especie creciera así rápidamente poblaría todo el mundo, algo que en la naturaleza no ocurre. Este modelo sin embargo tiene la gracia de indicarnos el potencial que podría tener una población de una especie de no haber restricciones en cuanto a recursos. Además, sólo lo podemos aplicar cuando se cumplen ciertos supuestos.

<img src="https://jorgecortes-m.github.io/images/exp_gro_cont.jpeg" alt="Grafico de crecimiento exponencial continuo">

1.- No hay inmigración o emigración <br>
2.- $b$ y $d$ son parámetros constantes, es decir no cambia la natalidad ni mortalidad. <br>
3.- No hay estructura genética, es decir todos los organismos tienen el mismo potencial reproductivo y la misma tasa de mortalidad asociada. <br>
4.- No hay estructura etaria o por tamaño, todos los organismos desde que nacen tienen la misma capacidad reproductiva y tasa de mortalidad asociada. <br>

A pesar de lo alejado a la realidad que puede ser este modelo, suele ser una pieza clave de la que nacen preguntas como ¿Por qué los organismos no crecen de esta forma? ¿Cuáles son las condiciones para que una población pueda crecer exponencialmente? Preguntas relevantes incluso en ambitos relacionados con la biología evolutiva.
Aunque quizás lo hayas leído en algún sitio, la población humana no crece exponencialmente, puede que en algún intervalo de tiempo haya sido así, pero los datos actuales apuntan a que no sigue esa tendencia, relacionado con la disminución de la tasa de crecimiento en las últimas décadas. Para más información pueden revisar [ourworldindata.org](https://ourworldindata.org/world-population-growth).

Si quieres adentrarte mucho más en este tema te recomiendo el libro “a primer for ecology” de Nicholas Gotelli donde se trabaja en detalle este tipo de modelos.

Si esta es la primera vez que te enfrentas a un modelo de esta índole, te dejo las siguientes preguntas ¿Qué crees que debería agregarse a este modelo? ¿Qué cosas no considera que podrían ser importantes? 