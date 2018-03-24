# Instalar Apache Spark

En este artículo veremos cómo instalar Apache Spark y utilizarlo con Python.

Como punto de partida consideraremos que tenemos una máquina con S.O. Ubuntu 16.04 (Xenial) en la que ya se encuentran instalados Java JDK 1.7 ó posterior así como Python y su gestor de paquetes pip.

En primer lugar descargaremos la última versión de Apache Spark desde su página oficial. Podemos lograrlo haciendo click directamente sobre el enlace de desarga, o por línea de comandos mediante el comando wget y la url de dicho enlace.

La url de la página principal de descargas de Apache Spark es:

[http://spark.apache.org/downloads.html](http://spark.apache.org/downloads.html)

y la url del paquete de la versión actual de Apache Spark (a fecha 2018-03-18):

[http://apache.rediris.es/spark/spark-2.3.0/spark-2.3.0-bin-hadoop2.7.tgz](http://apache.rediris.es/spark/spark-2.3.0/spark-2.3.0-bin-hadoop2.7.tgz)

![Página de descarga](images/spark-download-page.png)

Después de descargar el archivo con la última versión de Spark, lo descomprimiremos y moveremos el directorio resultante a su ubicación final. Por último crearemos un enlace simbólico para simplificar el acceso y abstraernos del número de versión; de este modo podremos tener instaladas distintas versiones y utilizar la que más nos convenga en cada momento.

```
# Descargar la última versión de Apache Spark
wget http://apache.rediris.es/spark/spark-2.3.0/spark-2.3.0-bin-hadoop2.7.tgz

# Descomprimir el fichero
tar -xzf spark-2.3.0-bin-hadoop2.7.tgz

# Mover el directorio de instalación a su ubicación definitiva
sudo mv spark-2.3.0-bin-hadoop2.7.tgz /opt/spark-2.3.0

# Crear un link (enlace simbólico) a la instalación de Spark
sudo ln -s /opt/spark-2.3.0 /opt/spark
```

![Instalación](images/spark-terminal-install.png)

A continuación debemos indicar a nuestro intérprete de comandos bash dónde encontrar la aplicación, y para ello crearemos la variable de entorno SPARK_HOME y modificamos PATH:
```
export SPARK_HOME=/opt/spark
export PATH=$SPARK_HOME/bin:$PATH
```

Podemos escribir estas variables de entorno cada vez que abramos una nueva consola o terminal, o evitar tener que hacerlo añadiéndolas a nuestro archivo ~./bashrc para tenerlas siempre disponibles (en este caso hay que reiniciar la consola despues de modificar el fichero).

```
# Editar ~/.bashrc
sudo nano ~/.bashrc
```

Una vez que hemos indicado al intérpreta de comandos bash dónde puede encontrar la instalación de apache Spark, sólo nos queda abrir la consola y ponernos a trabajar...

```
# Abir la consola de Python para Spark
pyspark
```

![Consola](images/spark-console.png)

Como podemos ver en la imagen anterior, durante el arranque de la aplicacion se nos informa, entre otras cosas, de las versiones de Spark y de Python.

Para salir de la consola utilizaremos el comando exit().
```
exit()
```

## Referencias

- [Get Started with PySpark and Jupyter Notebook in 3 Minutes](https://blog.sicara.com/get-started-pyspark-jupyter-guide-tutorial-ae2fe84f594f)
- [Apache Spark in Python: Beginner's Guide](https://www.datacamp.com/community/tutorials/apache-spark-python)

