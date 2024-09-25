# Examen parcial

Indicaciones generales:

-   Usted tiene el período de la clase para resolver el examen parcial.

-   La entrega del parcial, al igual que las tareas, es por medio de su
    cuenta de github, pegando el link en el portal de MiU.

-   Pueden hacer uso del material del curso e internet (stackoverflow,
    etc.). Sin embargo, si encontramos algún indicio de copia, se
    anulará el exámen para los estudiantes involucrados. Por lo tanto,
    aconsejamos no compartir las agregaciones que generen.

## Sección 0: Preguntas de temas vistos en clase (20pts)

-   Responda las siguientes preguntas de temas que fueron tocados en
    clase.

1.  ¿Qué es una ufunc y por qué debemos de utilizarlas cuando
    programamos trabajando datos?

-   Son funciones vectorizadas como sapply, que aplican operaciones
    sobre vectores o matrices eficientemente sin bucles. Sirven para
    trabajar con grandes volúmenes de datos.

1.  Es una técnica en programación numérica que amplía los objetos que
    son de menor dimensión para que sean compatibles con los de mayor
    dimensión. Describa cuál es la técnica y de un breve ejemplo en R.

-   Broadcasting permite operar entre arrays de diferentes dimensiones

``` r
A <- matrix(1:4, nrow=2, ncol=2)  
B <- 2  # Escalar
A + B  # Broadcasting permite sumar un escalar a una matriz
```

    ##      [,1] [,2]
    ## [1,]    3    5
    ## [2,]    4    6

1.  ¿Qué es el axioma de elegibilidad y por qué es útil al momento de
    hacer análisis de datos?

-   Es la capacidad de seleccionar subconjuntos de datos basados en
    condiciones, esencial para análisis de datos, por ejemplo, en
    filtrado usando dplyr en R

1.  Cuál es la relación entre la granularidad y la agregación de datos?
    Mencione un breve ejemplo. Luego, exploque cuál es la granularidad o
    agregación requerida para poder generar un reporte como el
    siguiente:

| Pais | Usuarios |
|------|----------|
| US   | 1,934    |
| UK   | 2,133    |
| DE   | 1,234    |
| FR   | 4,332    |
| ROW  | 943      |

-   La granularidad define el detalle de los datos, mientras que la
    agregación combina datos detallados. Para generar un reporte por
    país, se usa group_by y summarize, como este ejemplo de r

reporte_pais \<- data %\>% group_by(Pais) %\>% summarize(Usuarios =
n_distinct(Usuario))

## Sección I: Preguntas teóricas. (50pts)

-   Existen 10 preguntas directas en este Rmarkdown, de las cuales usted
    deberá responder 5. Las 5 a responder estarán determinadas por un
    muestreo aleatorio basado en su número de carné.

-   Ingrese su número de carné en `set.seed()` y corra el chunk de R
    para determinar cuáles preguntas debe responder.

``` r
set.seed(20210619) 
v<- 1:10
preguntas <-sort(sample(v, size = 5, replace = FALSE ))

paste0("Mis preguntas a resolver son: ",paste0(preguntas,collapse = ", "))
```

    ## [1] "Mis preguntas a resolver son: 5, 6, 8, 9, 10"

### Listado de preguntas teóricas

1.  ¿Cuál es la forma correcta de cargar un archivo de texto donde el
    delimitador es `:`?

-   Para cargar un archivo de texto con delimitador : en R, usa
    read.table() 0 read.”tipo de archivo” y especifica sep = ‘:’ para
    indicar el delimitador:

Ejemplo

``` r
# data <- read.table('archivo.txt', sep = ':', header = TRUE)
```

1.  ¿Qué es un vector y en qué se diferencia en una lista en R?

-   Un vector contiene elementos del mismo tipo sean numéricos,
    caracteres o otros diferentes, mientras que una lista puede contener
    diferentes tipos de datos Ejemplo:

``` r
v <- c(1, 2, 3)  # Vector con numeros
l <- list(1, 'texto', TRUE)  # Lista con diferentes tipos

v
```

    ## [1] 1 2 3

``` r
l
```

    ## [[1]]
    ## [1] 1
    ## 
    ## [[2]]
    ## [1] "texto"
    ## 
    ## [[3]]
    ## [1] TRUE

1.  Si en un dataframe, a una variable de tipo `factor` le agrego un
    nuevo elemento que *no se encuentra en los niveles existentes*,
    ¿cuál sería el resultado esperado y por qué?
    -   El nuevo elemento
    -   `NA`

-   `NA` -\> Esto ocurre porque los factores en R solo aceptan valores
    que ya están predefinidos en sus niveles.

1.  En SQL, ¿para qué utilizamos el keyword `HAVING`?

-   Se usa para filtrar los resultados de agregaciones como SUM(),
    COUNT(), etc., que no pueden ser filtrados con WHERE

1.  Si quiero obtener como resultado las filas de la tabla A que no se
    encuentran en la tabla B, ¿cómo debería de completar la siguiente
    sentencia de SQL?

    -   SELECT \* FROM A Left Join B ON A.KEY = B.KEY WHERE B.Key IS
        NULL

Extra: ¿Cuántos posibles exámenes de 5 preguntas se pueden realizar
utilizando como banco las diez acá presentadas? (responder con código de
R.)

``` r
choose(10, 5)
```

    ## [1] 252

## Sección II Preguntas prácticas. (30pts)

-   Conteste las siguientes preguntas utilizando sus conocimientos de R.
    Adjunte el código que utilizó para llegar a sus conclusiones en un
    chunk del markdown.

A. De los clientes que están en más de un país,¿cuál cree que es el más
rentable y por qué?

B. Estrategia de negocio ha decidido que ya no operará en aquellos
territorios cuyas pérdidas sean “considerables”. Bajo su criterio,
¿cuáles son estos territorios y por qué ya no debemos operar ahí?

### I. Preguntas teóricas

## A

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
parcial_anonimo <- readRDS("parcial_anonimo.rds")

ventas_por_cliente_pais <- parcial_anonimo %>%
  group_by(Cliente, Pais) %>%
  summarize(Ventas_Totales = sum(Venta, na.rm = TRUE))  
```

    ## `summarise()` has grouped output by 'Cliente'. You can override using the
    ## `.groups` argument.

``` r
clientes_multinacionales <- ventas_por_cliente_pais %>%
  group_by(Cliente) %>%
  filter(n_distinct(Pais) > 1)  

ventas_totales_multinacionales <- clientes_multinacionales %>%
  group_by(Cliente) %>%
  summarize(Ventas_Totales_Globales = sum(Ventas_Totales))  

cliente_mas_rentable <- ventas_totales_multinacionales %>%
  arrange(desc(Ventas_Totales_Globales)) %>%
  slice(1)  

print(cliente_mas_rentable)
```

    ## # A tibble: 1 × 2
    ##   Cliente  Ventas_Totales_Globales
    ##   <chr>                      <dbl>
    ## 1 a17a7558                  19818.

``` r
cat("El cliente mas rentable es", cliente_mas_rentable$Cliente,
    "con ventas totales globales de", cliente_mas_rentable$Ventas_Totales_Globales, 
    "porque opera en múltiples países y genera el mayor volumen de ventas totales entre todos los clientes multinacionales.\n")
```

    ## El cliente mas rentable es a17a7558 con ventas totales globales de 19817.7 porque opera en múltiples países y genera el mayor volumen de ventas totales entre todos los clientes multinacionales.

-   El cliente mas rentable es a17a7558 con ventas totales globales de
    19817.7 porque opera en múltiples países y genera el mayor volumen
    de ventas totales entre todos los clientes multinacionales.

## B

``` r
library(dplyr)
parcial_anonimo <- readRDS("parcial_anonimo.rds")



ventas_por_territorio <- parcial_anonimo %>%
  group_by(Territorio) %>%
  summarize(Ventas_Totales = sum(Venta, na.rm = TRUE))

# Definimos un umbral para ventas consideradas considerablemente bajas
# seleccionamos los territorios con ventas totales en el percentil 10 más bajo
umbral_ventas_bajas <- quantile(ventas_por_territorio$Ventas_Totales, 0.10)  

territorios_con_ventas_bajas <- ventas_por_territorio %>%
  filter(Ventas_Totales <= umbral_ventas_bajas)


# Añadir una columna para indicar si el territorio tiene ventas bajas
ventas_por_territorio <- ventas_por_territorio %>%
  mutate(Ventas_Bajas = ifelse(Ventas_Totales <= umbral_ventas_bajas, "Sí", "No"))


print(territorios_con_ventas_bajas)
```

    ## # A tibble: 11 × 2
    ##    Territorio Ventas_Totales
    ##    <chr>               <dbl>
    ##  1 0320288f            845. 
    ##  2 0bfe69a0            384. 
    ##  3 13b223c9             49.9
    ##  4 368301e2            121. 
    ##  5 3e0d75d0            647. 
    ##  6 4163fa3f            580. 
    ##  7 456278b8            493. 
    ##  8 79428560            132  
    ##  9 aed8e579            747. 
    ## 10 e034e3c8            247. 
    ## 11 e6fd9da9             18.2

``` r
cat("Los territorios con pérdidas considerables o ventas muy bajas son:\n")
```

    ## Los territorios con pérdidas considerables o ventas muy bajas son:

``` r
print(territorios_con_ventas_bajas$Territorio)
```

    ##  [1] "0320288f" "0bfe69a0" "13b223c9" "368301e2" "3e0d75d0" "4163fa3f"
    ##  [7] "456278b8" "79428560" "aed8e579" "e034e3c8" "e6fd9da9"

``` r
cat("\nNo debemos operar en estos territorios porque generan ventas totales que están entre el 10% más bajo de todos los territorios.\n")
```

    ## 
    ## No debemos operar en estos territorios porque generan ventas totales que están entre el 10% más bajo de todos los territorios.

``` r
cat("Continuar operando en estas áreas podría no ser rentable debido a los costos fijos y operativos asociados que pueden superar los ingresos generados.\n")
```

    ## Continuar operando en estas áreas podría no ser rentable debido a los costos fijos y operativos asociados que pueden superar los ingresos generados.

-   Los territorios con pérdidas considerables o ventas muy bajas son:
    \[1\] “0320288f” “0bfe69a0” “13b223c9” “368301e2” “3e0d75d0”
    “4163fa3f” \[7\] “456278b8” “79428560” “aed8e579” “e034e3c8”
    “e6fd9da9”

No debemos operar en estos territorios porque generan ventas totales que
están entre el 10% más bajo de todos los territorios. Continuar operando
en estas áreas podría no ser rentable debido a los costos fijos y
operativos asociados que pueden superar los ingresos generados.
