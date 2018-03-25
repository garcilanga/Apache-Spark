
# PySpark: Cargar ficheros de texto en un RDD

2018-03-25


En este artículo vamos a ver varias formas de cargar ficheros de texto en un RDD utilizando la operación de transformación **textFile**.

## Operación de transformación textFile

Mediante la operación de transformación **sc.textfile(_parameter_)** podemos cargar en un mismo RDD un fichero de texto, una lista de ficheros de texto, los ficheros de texto de un directorio, los ficheros de texto de una lista de directorios, ficheros de texto que cumplan un determinado patrón..., todo ello dependiendo del valor que le pasemos por parámetro (_parameter_).

## Descargar libros en ficheros de texto plano

Antes de nada, necesitamos disponer de varios ficheros de texto plano para poder trabajar con ellos. Aprovechamos esta ocasión para hablar del proyecto Gutemberg.

El **Proyecto Gutenberg** ofrece más de 56,000 eBooks gratuitos de obras literarias y científicas en distintos idiomas, para las que han expirado sus derechos de autor. Pueden ser leídos en línea o descargados en varios formatos.

Para nuestros ejercicios descargaremos varias joyas de la literatura Española desde esta plataforma web, cuya url de página principal es http://www.gutemberg.org


## Redordemos

- Un **RDD** (Resilient Distributed Dataset) es una colección de elementos que pueden procesarse en paralelo.
- Sobre un RDD se pueden realizar operaciones de **transformación** y operaciones de **acción**.
- Las operaciones de **transformación** son perezosas, no se realizan mientras no se invoque una operación de **acción**.
- La transformación **textFile(_parameter_)** se utiliza para cargar ficheros de texto plano en un RDD.
- La acción **collect()** retorna una lista de los elementos del RDD serializados.
- La acción **foreach(_function_)** aplica una función _function_ a cada uno de los elementos del RDD.


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
import os
import urllib.request
import datetime
```

## Descargar varios libros en ficheros de texto plano

Vamos a crear dos bibliotecas (subdirectorios) cada una de ellas con varios libros (ficheros de texto):
- Biblioteca 1:
    - Don Quijote
    - El lazarillo de Tormes
    - Zalacaín el aventurero
    - El Buscón
    - El libro del buen amor
- Biblioteca 2:
    - La Celestina
    - Don Juan Tenorio
    - Novelas ejemplares
    - La regenta
    - Platero y yo


```python
### Asigna las listas de libros para descargar

download_list_1 = [
    [ 'http://www.gutenberg.org/ebooks/2000.txt.utf-8', 'don_quijote.txt' ],
    [ 'http://www.gutenberg.org/ebooks/320.txt.utf-8', 'el_lazarillo_de_tormes.txt' ],
    [ 'http://www.gutenberg.org/ebooks/13264.txt.utf-8', 'zalacain_el_aventurero.txt' ],
    [ 'http://www.gutenberg.org/ebooks/32315.txt.utf-8', 'el_buscon.txt' ],
    [ 'http://www.gutenberg.org/ebooks/16625.txt.utf-8', 'el_libro_del_buen_amor.txt' ],
]

download_list_2 = [
    [ 'http://www.gutenberg.org/ebooks/1619.txt.utf-8', 'la_celestina.txt' ],
    [ 'http://www.gutenberg.org/ebooks/5201.txt.utf-8', 'don_juan_tenorio.txt' ],
    [ 'http://www.gutenberg.org/files/55916/55916-0.txt', 'novelas_ejemplares.txt' ],
    [ 'http://www.gutenberg.org/ebooks/17073.txt.utf-8', 'la_regenta.txt' ],
    [ 'http://www.gutenberg.org/ebooks/9980.txt.utf-8', 'platero_y_yo.txt' ], 
]
```


```python
### Crea los dos directorios (bibliotecas) de descarga
path1 = './biblioteca1'
if not os.path.exists(path1):
    os.mkdir(path1)

path2 = './biblioteca2'
if not os.path.exists(path2):
    os.mkdir(path2)
```


```python
### Descarga la lista de libros de la primera biblioteca
for url, filename in download_list_1:
    filename = os.path.join(path1, filename)
    print('Desargando %s en %s' % (url, filename))
    urllib.request.urlretrieve(url, filename)
```

    Desargando http://www.gutenberg.org/ebooks/2000.txt.utf-8 en ./biblioteca1/don_quijote.txt
    Desargando http://www.gutenberg.org/ebooks/320.txt.utf-8 en ./biblioteca1/el_lazarillo_de_tormes.txt
    Desargando http://www.gutenberg.org/ebooks/13264.txt.utf-8 en ./biblioteca1/zalacain_el_aventurero.txt
    Desargando http://www.gutenberg.org/ebooks/32315.txt.utf-8 en ./biblioteca1/el_buscon.txt
    Desargando http://www.gutenberg.org/ebooks/16625.txt.utf-8 en ./biblioteca1/el_libro_del_buen_amor.txt



```python
### Descarga la lista de libros de la segunda biblioteca
for url, filename in download_list_2:
    filename = os.path.join(path2, filename)
    print('Desargando %s en %s' % (url, filename))
    urllib.request.urlretrieve(url, filename)
```

    Desargando http://www.gutenberg.org/ebooks/1619.txt.utf-8 en ./biblioteca2/la_celestina.txt
    Desargando http://www.gutenberg.org/ebooks/5201.txt.utf-8 en ./biblioteca2/don_juan_tenorio.txt
    Desargando http://www.gutenberg.org/files/55916/55916-0.txt en ./biblioteca2/novelas_ejemplares.txt
    Desargando http://www.gutenberg.org/ebooks/17073.txt.utf-8 en ./biblioteca2/la_regenta.txt
    Desargando http://www.gutenberg.org/ebooks/9980.txt.utf-8 en ./biblioteca2/platero_y_yo.txt


## Ejercicio 1: Cargar un fichero de texto en un RDD

En el primer ejercicio cargaremos un único fichero de texto en un RDD. Para ello, pasaremos como parámetro a la operación de transformación **textFile** una cadena de texto con la ruta del fichero de texto a cargar.

```
file_name = './biblioteca1/don_quijote.txt'
rdd = sc.textFile(file_name)
```
Mostraremos el número de filas totales del texto así como algunas de las primeras filas.


```python
import sys

from pyspark import SparkContext, SparkConf

if __name__ == "__main__":
    ini = datetime.datetime.now()

    # Crea el contexto de Spark
    conf = SparkConf().setAppName("MyAppName")
    sc = SparkContext(conf=conf)

    # Asigna el nombre y ruta del fichero de texto a cargar
    file_name = './biblioteca1/don_quijote.txt'

    # Carga el RDD
    rdd = sc.textFile(file_name)

    # Aplica la operación de acción collect al RDD para obtener la lista completa de líneas de texto
    lrdd = rdd.collect()

    # Muestra algunas líneas
    for line in lrdd[35:52]:
        print(line)
    print('\n- Número total de filas: %s' % len(lrdd))

    # Cierra el contexto de Spark
    sc.stop()
    
    fin = datetime.datetime.now()
    print('- Tiempo transcurido: %s' % (fin - ini))
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
    
    - Número total de filas: 37861
    - Tiempo transcurido: 0:00:05.321762


## Ejercicio 2: Cargar una lista de ficheros de texto en un úncio RDD

Ahora cargaremos en el mismo RDD el contenido de una lista de ficheros de texto. Para ello, pasaremos como parámetro a la operación de transformación **textFile** una cadena de texto con las rutas de los ficheros separadas por coma.

```
str_file_name_list = './biblioteca1/don_quijote.txt,./biblioteca1/la_celestina.txt,./biblioteca2/la_regenta.txt'
rdd = sc.textFile(str_file_name_list)
```

Mostraremos el número de filas totales del texto así como algunas de las primeras filas.


```python
import sys

from pyspark import SparkContext, SparkConf

if __name__ == "__main__":
    ini = datetime.datetime.now()

    # Crea el contexto de Spark
    conf = SparkConf().setAppName("MyAppName")
    sc = SparkContext(conf=conf)

    # Asigna los nombres y rutas de los ficheros de texto a cargar
    str_file_name_list = './biblioteca2/la_celestina.txt,./biblioteca1/don_quijote.txt,./biblioteca2/la_regenta.txt'

    # Carga el RDD
    rdd = sc.textFile(str_file_name_list)

    # Aplica la operación de acción collect al RDD para obtener la lista completa de líneas de texto
    lrdd = rdd.collect()

    # Muestra algunas líneas
    for line in lrdd[:25]:
        print(line)
    print('\n- Número total de filas: %s' % len(lrdd))

    # Cierra el contexto de Spark
    sc.stop()
    
    fin = datetime.datetime.now()
    print('- Tiempo transcurido: %s' % (fin - ini))
```

    **This is a COPYRIGHTED Project Gutenberg Etext, Details Below**
    The Project Gutenberg Etext of La Celestina by Fernando de Rojas
    
    Copyright 1998 R. S. Rudder
    
    Please take a look at the important information in this header.
    We encourage you to keep this file on your own disk, keeping an
    electronic path open for the next readers.  Do not remove this.
    
    
    **Welcome To The World of Free Plain Vanilla Electronic Texts**
    
    **Etexts Readable By Both Humans and By Computers, Since 1971**
    
    *These Etexts Prepared By Hundreds of Volunteers and Donations*
    
    Information on contacting Project Gutenberg to get Etexts, and
    further information is included below.  We need your donations.
    
    
    LA CELESTINA [In Spanish]
    
    by Fernando de Rojas
    
    (Edicion y Notas de Robert S. Rudder)
    
    - Número total de filas: 90972
    - Tiempo transcurido: 0:00:01.094952


## Ejercicio 3: Cargar  en un úncio RDD los ficheros de texto de un directorio


Para cargar en un único RDD el contenido de todos los ficheros de texto de un directorio, deberemos pasar como parámetro a la operación de transformación **textFile** una cadena de texto con la ruta del directorio que contiene los ficheros de texto a cargar.

```
directory_name = './biblioteca1'
rdd = sc.textFile(directory_name)
```

Mostraremos el número de filas totales del texto así como algunas de las primeras filas.


```python
import sys

from pyspark import SparkContext, SparkConf

if __name__ == "__main__":
    ini = datetime.datetime.now()

    # Crea el contexto de Spark
    conf = SparkConf().setAppName("MyAppName")
    sc = SparkContext(conf=conf)

    # Asigna el nombre y ruta del directorio
    directory_name = './biblioteca1'

    # Carga el RDD
    rdd = sc.textFile(directory_name)

    # Aplica la operación de acción collect al RDD para obtener la lista completa de líneas de texto
    lrdd = rdd.collect()

    # Muestra algunas líneas
    for line in lrdd[:25]:
        print(line)
    print('\n- Número total de filas: %s' % len(lrdd))

    # Cierra el contexto de Spark
    sc.stop()
    
    fin = datetime.datetime.now()
    print('- Tiempo transcurido: %s' % (fin - ini))
```

    Project Gutenberg's Clásicos Castellanos: Libro de Buen Amor, by Juan Ruiz
    
    This eBook is for the use of anyone anywhere at no cost and with
    almost no restrictions whatsoever.  You may copy it, give it away or
    re-use it under the terms of the Project Gutenberg License included
    with this eBook or online at www.gutenberg.net
    
    
    Title: Clásicos Castellanos: Libro de Buen Amor
    
    Author: Juan Ruiz
    
    Editor: Julio Cejador y Frauca
    
    Release Date: August 30, 2005 [EBook #16625]
    
    Language: Spanish
    
    
    *** START OF THIS PROJECT GUTENBERG EBOOK LIBRO DE BUEN AMOR ***
    
    
    
    
    Produced by Stan Goodman, PM Spanish, Pilar Somoza and the
    
    - Número total de filas: 77256
    - Tiempo transcurido: 0:00:00.598481


## Ejercicio 4: Cargar  en un úncio RDD los ficheros de texto de varios directorios

Para cargar en un único RDD el contenido de todos los ficheros de texto de varios directorios, deberemos pasar como parámetro a la operación de transformación **textFile** una cadena de texto con las rutas de los directorios que contienen los ficheros de texto a cargar, separadas por comas.

```
str_lst_directory_name = './biblioteca1,./biblioteca2'
rdd = sc.textFile(str_lst_directory_name)
```

Mostraremos el número de filas totales del texto así como algunas de las primeras filas.


```python
import sys

from pyspark import SparkContext, SparkConf

if __name__ == "__main__":
    ini = datetime.datetime.now()

    # Crea el contexto de Spark
    conf = SparkConf().setAppName("MyAppName")
    sc = SparkContext(conf=conf)

    # Asigna la lista de nombres y rutas de los directorios
    str_lst_directory_name = './biblioteca1,./biblioteca2'

    # Carga el RDD
    rdd = sc.textFile(str_lst_directory_name)

    # Aplica la operación de acción collect al RDD para obtener la lista completa de líneas de texto
    lrdd = rdd.collect()

    # Muestra algunas líneas
    for line in lrdd[:25]:
        print(line)
    print('\n- Número total de filas: %s' % len(lrdd))
    
    # Cierra el contexto de Spark
    sc.stop()
    
    fin = datetime.datetime.now()
    print('- Tiempo transcurido: %s' % (fin - ini))
```

    Project Gutenberg's Clásicos Castellanos: Libro de Buen Amor, by Juan Ruiz
    
    This eBook is for the use of anyone anywhere at no cost and with
    almost no restrictions whatsoever.  You may copy it, give it away or
    re-use it under the terms of the Project Gutenberg License included
    with this eBook or online at www.gutenberg.net
    
    
    Title: Clásicos Castellanos: Libro de Buen Amor
    
    Author: Juan Ruiz
    
    Editor: Julio Cejador y Frauca
    
    Release Date: August 30, 2005 [EBook #16625]
    
    Language: Spanish
    
    
    *** START OF THIS PROJECT GUTENBERG EBOOK LIBRO DE BUEN AMOR ***
    
    
    
    
    Produced by Stan Goodman, PM Spanish, Pilar Somoza and the
    
    - Número total de filas: 149391
    - Tiempo transcurido: 0:00:01.095239


## Ejercicio 5: Cargar  en un úncio RDD ficheros de texto según patrones de nombre y ruta

Por último, cargaremos en un único RDD el contenido de los ficheros cuyos nombres cumplan un patrón determinado. Para ello deberemos pasar como parámetro a la operación de transformación **textFile** una cadena de texto con los patrones de  de directorios y ficheros, separadas por comas.

Por ejemplo, podemos elegir los ficheros de la biblioteca 1 cuyos nombres empiecen por 'el\_' y los ficheros de la biblioteca 2 cuyos nombres empiecen por 'la\_':

```
str_lst_pattern = './biblioteca1/el_*,./biblioteca2/la_*'
rdd = sc.textFile(str_lst_pattern)
```

Otro ejemplo puede ser elegir los ficheros de cualquiera de las librerías que empiecen por 'don\_'

```
str_lst_pattern = './biblioteca[1-9]/don_*'
rdd = sc.textFile(str_lst_pattern)
```

Mostraremos el número de filas totales del texto así como algunas de las primeras filas.


```python
import sys

from pyspark import SparkContext, SparkConf

if __name__ == "__main__":
    ini = datetime.datetime.now()

    # Crea el contexto de Spark
    conf = SparkConf().setAppName("MyAppName")
    sc = SparkContext(conf=conf)

    # Asigna los patrones de directorios y ficheros
    #str_lst_pattern = './biblioteca1/el_*,./biblioteca2/la_*'
    str_lst_pattern = './biblioteca[1-9]/don_*'

    # Carga el RDD
    rdd = sc.textFile(str_lst_pattern)

    # Aplica la operación de acción collect al RDD para obtener la lista completa de líneas de texto
    lrdd = rdd.collect()

    # Muestra algunas líneas
    for line in lrdd[:25]:
        print(line)
    print('\n- Número total de filas: %s' % len(lrdd))
    
    # Cierra el contexto de Spark
    sc.stop()
    
    fin = datetime.datetime.now()
    print('- Tiempo transcurido: %s' % (fin - ini))
```

    The Project Gutenberg EBook of Don Juan Tenorio, by José Zorrilla
    
    This eBook is for the use of anyone anywhere at no cost and with
    almost no restrictions whatsoever.  You may copy it, give it away or
    re-use it under the terms of the Project Gutenberg License included
    with this eBook or online at www.gutenberg.org
    
    ** This is a COPYRIGHTED Project Gutenberg eBook, Details Below **
    **     Please follow the copyright guidelines in this file.     **
    
    Title: Don Juan Tenorio
    
    Author: José Zorrilla
    
    Translator: N. K. Mayberry
                A. S. Kline
    
    Posting Date: August 23, 2012 [EBook #5201]
    Release Date: March, 2004
    First Posted: June 2, 2002
    Last Updated: January 14, 2011
    
    Language: Spanish
    
    
    
    - Número total de filas: 45301
    - Tiempo transcurido: 0:00:00.585970


## Conclusiones

Como hemos podido comprobar, la versatilidad de la operación de transformación **textFile** nos permite abordar diferentes escenarios de carga de ficheros, desde ficheros individuales hasta listas de ficheros, ficheros de uno o varios directorios e incluso ficheros cuyos nombes y rutas cumplen con determinados patrones.


```python

```
