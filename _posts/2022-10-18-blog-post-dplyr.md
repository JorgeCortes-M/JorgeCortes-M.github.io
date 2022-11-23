---
title: 'Paquete Dplyr en R y su uso para el manejo de datos'
date: 2022-10-10
permalink: /posts/2022/10/Dplyr-R/
code-block-font-size: \large
tags:
  - R
  - RStudio
  - Data Analysis
  - dplyr
---

# ¿Análisis de datos? dplyr es tu aliado

Una de las herramientas más usadas para el análisis de datos es R, este software cuenta con funciones nativas que nos pueden ayudar a la hora de limpiar o querer extraer alguna información de nuestros conjuntos de datos. Sin embargo, muchas de estas operaciones pueden ser tediosas de escribir, para demostrarlo usaremos un set de datos de Kaggle sobre [población humana](https://www.kaggle.com/datasets/iamsouravbanerjee/world-population-dataset). Luego de descargar el archivo .csv procederemos a cargarlo en RStudio.

```R
library(readr)
popData <- read_csv(file = "./world_pop/world_population.csv")
```


Imaginemos que deseamos el ranking y la abreviatura de Chile, para el caso del código nativo de R, esto se haría así:
```R
popData[which(popData$Country == "Chile"), names(popData) %in% c("Rank", "CCA3")]
```

Mientras que con dplyr haríamos esto:
```R
popData %>%
filter(Country=="Chile")%>%
select(Rank,CCA3)
```
En el paquete dplyr existen diversas funciones que nos ayudarán a manipular nuestros dataframes, las cuales podemos dividir en funciones para resumir la información (summarise, count), para agrupar casos (group_by, rowwise, ungroup), Manipular casos (filter, distinct, slice, slice_sample, slice_min, slice_head, arrange, add_row), Manipular variables (pull, select, relocate, across, c_across, mutate, transmute, rename). Revisemos algunas de estas funciones.

El primer elemento que nos aporta dplyr y no mencioné antes es la tubería o pipe (%>%) podemos concatenar los resultados de una función y aplicarle otra directamente.

```R
head(popData) # Sin uso de pipe

popData %>%
  head() # Con uso de pipe
```
## Select()

Ahora si, vamos a las funciones de Dplyr, partiremos por select(), esta nos permite seleccionar columnas de nuestro data frame indicando su nombre. Veamos, seleccionemos los continentes de nuestro data set. 
```R
popData %>%
  select(Continent)
```
Esto nos genera un dataframe de una única columna con los continentes. <img src="https://jorgecortes-m.github.io/images/select_continent_blog_1.JPG" alt="Data frame ends_with">

También podemos seleccionar más de una columna y renombrar la columna Growth Rate. 
```R
popData %>%
  select(Country, Continent, Growth_Rate = 'Growth Rate')
```
Ahora tenemos un data frame con tres columnas. <img src="https://jorgecortes-m.github.io/images/select_mult_col_blog_1.JPG" alt="Data frame ends_with">

También podemos cambiar el nombre a una columna llamando a la función rename de dplyr, de esta forma:

```R
popData %>%
  select(Country, Continent, 'Growth Rate') %>%
  rename(Growth_Rate = 'Growth Rate')
```
Como ves no hay una sola forma de llevar a cabo estas tareas y la utilización de la tubería %>% facilita mucho usar una función tras otra.

Otra cosa que podemos hacer con select es utilizar funciones ayudantes o helper, en este caso usamos ends_with para seleccionar todas las columnas que terminen con "Population":

```R
popData %>%
  select(ends_with("Population"))
```

Así obtenemos el siguiente dataframe <img src="https://jorgecortes-m.github.io/images/select_ends_with.JPG" alt="Data frame ends_with">

## Relocate()

La segunda función que veremos brevemente es relocate, la cual como su nombre dice nos permite cambiar el orden de una columna en el data frame.
Podemos por ejemplo indicar que la columna Country quede como la última utilizando el parámetro .after. 

```R
popData %>%
  select(Country, Continent, 'Growth Rate') %>%
  rename(Growth_Rate = 'Growth Rate') %>%
  relocate(Country, .after = last_col())
```

Obteniendo el siguiente dataframe.
<img src="https://jorgecortes-m.github.io/images/relocate_after.JPG" alt="Data frame relocate after">

Podríamos dejarla antes de la última columna cambiando after por before. 

```R
popData %>%
  select(Country, Continent, 'Growth Rate') %>%
  rename(Growth_Rate = 'Growth Rate') %>%
  relocate(Country, .before = last_col())
```
Obteniendo el siguiente dataframe.
<img src="https://jorgecortes-m.github.io/images/relocate_before.JPG" alt="Data frame relocate before">

Incluso podemos hacer referencia a otra columna para posicionar la columna objetivo, pongamos Country luego de Continent.

```R
popData %>%
  select(Country, Continent, 'Growth Rate') %>%
  rename(Growth_Rate = 'Growth Rate') %>%
  relocate(Country, .after = Continent)
```
Obteniedo el mismo dataframe que obtuvimos en el ejemplo anterior.
<img src="https://jorgecortes-m.github.io/images/relocate_before.JPG" alt="Data frame relocate before">

## Mutate()

Siguiendo con las operaciones en columnas tenemos la función mutate, la cual nos permite crear una nueva columna. Por ejemplo una nueva columna podría ser una proyección de la población en 2023, al multiplicar la tasa de crecimiento por la población de cada país en 2022. Podríamos obtener también una columna con la diferencia en número de población entre 2022 y 2020 o incluso calcular el logaritmo de la población de 2022. Podemos hacer esto por separado o incluso en una sola línea de código.

```R
popData %>%
  mutate(Proy_2023 = `2022 Population` * `Growth Rate`) %>%
  mutate(Dif_betw_22_20 = `2022 Population` - `2020 Population`) %>%
  mutate(log_2022 = log(`2022 Population`)) %>%
  select(Proy_2023, Dif_betw_22_20, log_2022) #Caso de múltiples líneas

popData %>%
  mutate(Proy_2023 = `2022 Population` * `Growth Rate`,
         Dif_betw_22_20 = `2022 Population` - `2020 Population`,
         log_2022 = log(`2022 Population`))%>%
  select(Proy_2023, Dif_betw_22_20, log_2022) #Caso de una sola línea
```
Obteniendo en ambos casos el siguiente dataframe.
<img src="https://jorgecortes-m.github.io/images/mutate.JPG" alt="Dataframe con funcion mutate">

Si notas, el nombre de la columna va antes de la operación que la define, además combinamos lo obtenido a través de mutate con la función select() para observar las nuevas columnas creadas.

## Funciones que manipulan filas

Ahora revisaremos las funciones que manipulan casos o filas. Quizás una de las más importantes es la función filter.

## Filter()

Esta función como lo indica su nombre nos permite filtrar nuestro data set para obtener solamente las observaciones que deseemos, por ejemplo podríamos obtener la información únicamente de Chile, al filtrar Country y buscar Chile.

```R
popData %>%
  filter(Country == "Chile")
```
Obtendríamos el siguiente dataframe.
<img src="https://jorgecortes-m.github.io/images/filter_unparam.JPG" alt="Dataframe con funcion filter y un parametro">

Antes de continuar es importante mencionar que esta función va de la mano con los operadores lógicos o booleanos, los cuales puedes [ver por acá](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf).

Podemos filtrar usando estos operadores e incluso combinarlos para obtener filtros mas precisos. Primero filtraremos por el ranking, escogiendo a aquellos que esten bajo el rank 100 y además sean paises de América del sur. podemos hacer eso mismo al utilizar el operador lógico AND, representado por este "&" carácter.

```R
popData %>%
  filter(Rank < 100, Continent == "South America")

popData %>%
  filter(Rank < 100 & Continent == "South America")
```

Obteniendo el siguiente dataframe.
<img src="https://jorgecortes-m.github.io/images/filter_and.JPG" alt="Dataframe con funcion filter operador AND">

Podríamos querer aquellos paises que estén bajo el rank 100 , pero que no sean de América del sur, para eso usaríamos el operador NOT, representado por !=.

```R
popData %>%
  filter(Rank < 100 & Continent != "South America")
```
Obteniendo el siguiente dataframe.
<img src="https://jorgecortes-m.github.io/images/filter_not.JPG" alt="Dataframe con funcion filter operador NOT">

Incluso podríamos filtrar para obtener nuevamente los países bajo el rank 100 o aquellos que sean de América del sur con el operador OR.

```R
popData %>%
  filter(Rank < 100 | Continent == "South America")
```

Que nos da un datframe similar al anterior pero que además contiene a todos los paises de américa del sur.
<img src="https://jorgecortes-m.github.io/images/filter_not.JPG" alt="Dataframe con funcion filter operador NOT">

## Arrange()

La segunda y última función de esta categoría que revisaremos es arrange, la cual nos permite ordenar las filas con respecto a un valor. Por ejemplo ordenemos estos datos en función de la población en 2022. 

```R
popData %>%
  arrange(`2022 Population`)
```
<img src="https://jorgecortes-m.github.io/images/arrange_1asc.JPG" alt="Dataframe con arrange simple">

Como vemos lo ordena de menor a mayor, esto lo podemos cambiar a forma descendente usando la función desc. 

```R
popData %>%
  arrange(desc(`2022 Population`))
```
<img src="https://jorgecortes-m.github.io/images/arrange_2desc.JPG" alt="Dataframe con arrange desc">

Y así podríamos ordenar este data frame en función de cualquier variable, incluso de aquellas que podamos crear con mutate, calculemos nuevamente la diferencia entre la población de 2022 y 2020 y ordenemos en relación a ese calculo. 

```R
popData %>%
  mutate(dif_2020_2022 = `2022 Population`- `2020 Population`)%>%
  arrange(desc(dif_2020_2022))
```
<img src="https://jorgecortes-m.github.io/images/arrange_3mutate.JPG" alt="Dataframe usando mutate">

Como vemos India ahora esta en primer lugar seguido de Nigeria y China ya no esta en los 10 primeros lugares.

## Funciones para organizar información

Por ultimo revisaremos en conjunto una función para agrupar group_by y por otro lado dos funciones para resumir información, count y summarise.

## Group_by() y Summarise()

Group_by por su parte nos permite agrupar filas con respecto a los valores de una columna en particular, por ejemplo agrupemos este data set en función de los continentes.

```R
popData %>%
  group_by(Continent)
```

<img src="https://jorgecortes-m.github.io/images/group_by_solo.JPG" alt="Dataframe usando sólo group_by">

En principio no vemos mucho cambio, pero si acoplamos esta función con summarise, la cual nos permite resumir información y creamos una columna que nos indique el total de elementos de cada grupo creado, vemos como group_by puede ser una gran función para ayudarnos a resumir información, ya que cambia como el data set interactúa con otras funciones de dplyr.

```R
popData %>%
  group_by(Continent) %>%
  summarise(CountperContin = length(Continent))
```
<img src="https://jorgecortes-m.github.io/images/group_by_summarise.JPG" alt="Dataframe usando group_by y summarise">

Otra cosa que podemos hacer es crear una nueva variable con mutate y luego agrupar en torno a esa variable y obtener algún resumen de la información. Acá clasificaremos los paises otorgandoles un 1 cuando su tasa de crecimiento sea mayor o igual a 1 y un 0 cuando sea menor a 1, luego agruparemos por esta variable y contaremos cuantos paises hay en cada clase.

```R
popData %>%
  mutate(class_gr = ifelse(`Growth Rate`>=1, 1, 0))%>%
  group_by(class_gr) %>%
  summarise(n = length(class_gr))
```

<img src="https://jorgecortes-m.github.io/images/group_by_mutate.JPG" alt="Dataframe usando group_by y mutate">

## Count()

Por último el primer ejercicio que realizamos en esta sección, podríamos haberlo hecho con count() de esta forma, mucho más directo, count nos sirve para contar los casos de una variable o columna de interés.

```R
popData %>%
  count(Continent)
```
<img src="https://jorgecortes-m.github.io/images/count_1.JPG" alt="Dataframe usando count">

 Incluso, aunque no muy usado, podemos evaluar alguna condición dentro de count, por ejemplo evaluemos la condición de paises donde la población de 2022 es es mayor que 100000.

 ```R
popData %>%
  count(`2022 Population` > 100000)
```

<img src="https://jorgecortes-m.github.io/images/count_2.JPG" alt="Dataframe usando count para condicional">

Vemos como nos entrega un resultado tanto para cuando es verdadero como para cuando esta condición es falsa.

Con esto hemos revisado algunas de las principales funciones de dplyr lo que te permitiría seguir explorando este increíble paquete. Es posible hacer mucho más, pero sin duda estas funciones te ayudarán a dar tus primeros pasos.