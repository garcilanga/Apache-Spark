
# PySpark: Mostrar el contenido de un RDD

2018-03-25

En este artículo vamos a practicar con las acciones **collect()** y **forearch(_function_)** para ver distintas formas de imprimir o mostrar el contenido de un **RDD**.

La acción **collect()** retorna una lista de los elementos del RDD serializados.

La acción **foreach(_function_)** aplica una función _function_ a cada uno de los elementos del RDD.


## Redordemos

- Un **RDD** (Resilient Distributed Dataset) es una colección de elementos que pueden procesarse en paralelo.
- Sobre un RDD se pueden realizar operaciones de **transformación** y operaciones de **acción**.
- Las operaciones de **transformación** son perezosas, no se realizan mientras no se invoque una operación de **acción**.
- La transformación **textFile(_parameter_)** se utiliza para cargar ficheros de texto plano en un RDD.


## Nota importante

Si tras la instalación de Apache Spark y Jupyter Notebook no incluiste las variables de entorno en tu fichero ~/.bashrc, no olvides ejecutar las siguientes instrucciones antes de invocar _jupyter notebook_:
```
    export SPARK_HOME=/opt/spark
    export PATH=$SPARK_HOME/bin:$PATH
    export PYSPARK_DRIVER_PYTHON=jupyter
    export PYSPARK_DRIVER_PYTHON_OPTS='notebook'
```

```
    jupyter notebook
```
## Fuentes

[Tutorial Kart](https://www.tutorialkart.com)



```python
### Carga PySpark
import findspark
findspark.init()

### Carga algunas librerías adicionales que necesitaremos
import urllib.request
import datetime
```

## Descargar un libro en fichero de texto plano

En primer lugar necesitaremos un fichero de texto plano para poder trabajar con él. 

En la web http://www.gutemberg.org hay disponibles una gran cantidad de obras literarias de todos los tiempos e idiomas y en distintos formatos, así que descargaremos un libro desde esta plataforma web.

El libro ejegido es '**Don Quijote de la Mancha**', obra cumbre no solo en la literatura española, sino importantísima también en la literatura universal, una de las más reeditadas y traducidas del mundo.


```python
url = 'http://www.gutenberg.org/ebooks/2000.txt.utf-8'
filename = './don_quijote.txt'
urllib.request.urlretrieve(url, filename)
```




    ('./don_quijote.txt', <http.client.HTTPMessage at 0x7f4398114588>)



## Ejercicio 1:

Cargaremos el fichero de texto en un RDD, obtendremos el array de líneas correspondiente mediante la acción **collect()** y mostraremos con el comando print() sólo algunas lineas del principio (ya que el fichero competo tiene casi 38.000 líneas).

El resultado se mostrará en este mismo notebook.


```python
import sys
 
from pyspark import SparkContext, SparkConf

if __name__ == "__main__":
    ini = datetime.datetime.now()
 
    # Crea el contexto de Spark
    conf = SparkConf().setAppName("MyAppName")
    sc = SparkContext(conf=conf)
 
    # Carga el RDD
    rdd = sc.textFile(filename)
 
    # Collect the RDD to a list
    rowlist = rdd.collect()
 
    # Muestra los primeros elementos de la lista
    for row in rowlist[35:52]:
        print(row)
    
    # Cierra el contexto de Spark
    sc.stop()
    
    fin = datetime.datetime.now()
    print('\nTiempo transcurido: %s' % (fin - ini))
```

    
    El ingenioso hidalgo don Quijote de la Mancha
    
    
    TASA
    
    Yo, Juan Gallo de Andrada, escribano de Cámara del Rey nuestro señor, de
    los que residen en su Consejo, certifico y doy fe que, habiendo visto por
    los señores dél un libro intitulado El ingenioso hidalgo de la Mancha,
    compuesto por Miguel de Cervantes Saavedra, tasaron cada pliego del dicho
    libro a tres maravedís y medio; el cual tiene ochenta y tres pliegos, que
    al dicho precio monta el dicho libro docientos y noventa maravedís y medio,
    en que se ha de vender en papel; y dieron licencia para que a este precio
    se pueda vender, y mandaron que esta tasa se ponga al principio del dicho
    libro, y no se pueda vender sin ella. Y, para que dello conste, di la
    presente en Valladolid, a veinte días del mes de deciembre de mil y
    seiscientos y cuatro años.
    
    Tiempo transcurido: 0:00:04.744016


## Ejercicio 2:

Cargaremos el fichero de texto en un RDD, y recorreremos todas sus líneas mediante la acción **foreach(f)**. En cada iteración, se aplicará a cada elemento del RSS (línea de texto) una función _f_ cuyo cometido será ejecutar el comando print().

En este caso, el resultado se mostrará en el terminal desde donde ejecutamos jupyter notebook.


```python
import sys
 
from pyspark import SparkContext, SparkConf

if __name__ == "__main__":
    ini = datetime.datetime.now()

    # Crea el contexto de Spark
    conf = SparkConf().setAppName("MyAppName")
    sc = SparkContext(conf=conf)

    # Carga el RDD
    rdd = sc.textFile(filename)

    # Define una función para aplicar a cada elemento del RDD
    def myfunction(row): print(row)

    # Aplicala función myfunction(x) a cada elemento (línea de texto) del RDD
    rdd.foreach(myfunction)
 
    # Cierra el contexto de Spark
    sc.stop()
    
    fin = datetime.datetime.now()
    print('\nTiempo transcurido: %s' % (fin - ini))
```

    
    Tiempo transcurido: 0:00:02.054515


### Consideraciones


En el primer ejercicio, la acción **collect** tiene que serializar todos los elementos del RDD, generar un array con todos ellos y detornarlo. Después se recorre el array y se imprime por pantalla.

En el segundo ejercicio, la acción **foreach** recorre directamente los elementos del RDD y le aplica una función que los imprime.

En segundo ejercicio es más eficiente en términos de memoria y tiempo, ya que no necesita crear el array en memoria ni gastar el tiempo necesario para ello.



```python

```
