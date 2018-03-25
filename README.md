# Apache Spark

Apache Spark es un framework open source para **procesamiento de datos masivos mediante computación distribuida**.

Su diseño se basa en tres pilares fundamentales: **velocidad, facilidad de uso y capacidades avanzadas de analítica**.

Porporciona APIs en **Java, Scala, Python y R**.

También soporta un importante conjunto de herramientas de alto nivel entre las que se incluyen:
- **Spark SQL**, para el procesamiento de datos estructurados basado en SQL,
- **MLlib**, para implementar aprendizaje automático (machine learning), 
- **GraphX**, para el procesamiento de grafos y
- **Spark Streaming**, para procesamiento continuo (stream processing).

**Spark resuelve algunas de las limitaciones inherentes de Hadoop y MapReduce**. Spark puede utilizarse junto con Hadoop, pero no es un requisito indispensable. Spark extiende el modelo MapReduce, haciéndolo más rápido y posibilitando más escenarios de análisis como, por ejemplo, consultas interactivas y procesamiento de flujos en tiempo real. Esto es posible gracias a que Spark utiliza un clúster de cómputo en memoria (in-memory).

## Spark en entornos de producción

Para explotar al máximo todas sus posibilidades, Apache Spark requiere de un cluster manager y un sistema de almacenamiento distribuido.

Para la gestión del cluster, Spark soporta las opciones siguientes:
- Spark Standalone (Cluster Spark Nativo).
- Hadoop YARN.
- Apache Mesos.

Para el almacenamiento distribuido, Spark presenta interfaces hacia una gran variedad de plataformas:
- Hadoop Distributed File System (HDFS)
- MapR File System (MapR-FS)
- Cassandra
- OpenStack Swift
- Amazon S3
- Kudu
- Soporta incluso soluciones personalizadas.

## Spark en entornos de prueba o desarollo

Spark también soporta un modo local pesudo-distribuido, normalmente utilizado solo para pruebas o en entornos de desarrollo donde el almacenamiento distribuido no es obligatorio y se puede usar el sistema de archivos local. En un escenarion como este, Spark se ejecuta en una única máquina con un executor por cada core de CPU.

Los artículos de esta serie estarán orientados a una instalación de Apache Spark en modo local, y entre ellos analizaremos algunos temas como los siguientes:

1. [Instalar Apache Spark.](https://github.com/garcilanga/Apache-Spark/blob/master/Instalar%20Apache%20Spark.md)
2. [Instalar Jupyter Notebook y usarlo con Python y Spark.](https://github.com/garcilanga/Apache-Spark/blob/master/Instalar%20Jupyter%20Notebook%20y%20usarlo%20con%20Python%20y%20Spark.md)
3. [Ejercicio: Cálculo del número Pi.](https://github.com/garcilanga/Apache-Spark/blob/master/PySpark-C%C3%A1lculo%20del%20n%C3%BAmero%20PI.md)
4. Ejercicio: contar y buscar palabras en archivos de texto. 
5. ...

### Referencias

[Apache Spark](https://es.wikipedia.org/wiki/Apache_Spark)

[Un Vistazo a Apache Spark Streaming](https://sg.com.mx/revista/50/un-vistazo-apache-spark-streaming)
