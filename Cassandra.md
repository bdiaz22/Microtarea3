# Cassandra
Es una base de datos NoSQL (Not-Only SQL) distribuida y de código abierto (Open Source). Esta base de datos es perfecta si es que se necesita escalabilidad y disponibilidad en los datos sin tener que comprometer el rendimiento de estos.

## Características de Cassandra
Cassandra ha sido diseñada para cumplir varios requirimientos de arquitecturas que otras bases de datos no podían hacer.
- **Aprueba de Fallas:**  Cassandra brindra confianza en que  su arquitectura no haya ningún error cuando existan varios nodos conectados en un cluster. Si es que llegase a fallar un nodo, el cluster debería de poder continuar con sus operaciones que estaba realizando.
- **Alta Escalabilidad:** Debe de soportar una cantidad masiva de nodos y debería de ser posible añadir un nuevo nodo sin la necesidad de detener los procesos realizados por el cluster.
- **Alta Dsitribuidad y Alta disponibilidad:** La información puede ser repartida en los nodos del cluster y ser accedida por cualquiera de ellos. De tal forma que si llegase a fallar algún nodo, la arquitecturar seguirá funcionando.
- **Mayor Rendimiento:** La lectura y escritura de los datos es la más óptima, ya que puede ser usada en tiempo real.

## Arquitectura de Cassandra

- Cassandra está diseñada de tal forma que no hay una jerarquía de nodos maestros y esclavos.
- Tiene una architectura de tipo anillo, es decir las conexiones entre los nodos tiene la forma de un anillo.
- Los datos son distribuidos automáticamnete entre los nodos.
- Los datos son almacenados en memoria y escritos en disco.
- Los datos se replican en los nodos para brindar redundancia.
- Los valores hash de las llaves se utilizan para distribuir los datos entre los nodos del clúster. 
![Imagen](cassandra.png)

## Proceso de Escritura de datos en Cassandra
Los pasos para realizar una escritura de datos en Cassandra son los siguientes:
- Los datos se escriben en un registro de confirmación en el disco. 
- Luego, los datos se envían a un nodo responsable en función del valor hash. 
- Los nodos escriben los datos en una tabla en memoria llamada memtable.
- En el Memtable se hace el proceso de escritura de datos en un sstable en la memoria. Sstable son las siglas de Sorted String table. Esta tabla contiene datos consolidados de todas las actualizaciones de la tabla.
- Desde el sstable, los datos se van a actualizar en la tabla real.
- Si el nodo responsable está inactivo, los datos se escribirán en otro nodo identificado como tempnode. El tempnode retendrá los datos temporalmente hasta que el nodo responsable se encuentre activo.
![Imagen](cassandra2.png)

## Proceso de Lectura de Datos en Cassandra
La lectura de datos se realiza de forma paralela en todos los nodos. Si un nodo está inactivo, los datos se leen de la réplica de los datos. Para realizar la lectura de datos en Cassandra se dan diferentes preferencias las cuales son las siguientes: 
- Los datos en el mismo nodo tienen preferencia en primer lugar y se consideran datos locales.
- Los datos en el mismo rack tienen una segunda preferencia y se consideran rack locales.
- Los datos del mismo centro de datos reciben una tercera preferencia y se consideran locales del centro de datos.
- A los datos de un centro de datos diferente se les da la menor preferencia. 
![Imagen](cassandra3.png)

## Particiones 
En Cassandra, la distribución de los datos se realiza a través de particiones, que luego se almacenan en diferentes nodos del clúster.

- Todos los datos de una sola partición siempre residen en un solo nodo. Los datos de una partición nunca se distribuyen entre varios nodos. Esto es importante, ya que no tendremos que visitar varios nodos para obtener datos de partición.
- Cuando se realizan consultas de los datos en Cassandra, la consulta debe especificar el valor de la llave de partición. Ya que el valor hash de la llave se asigna a un nodo en el clúster.
- Cada partición contiene varias filas de datos (que se agrupan por una llave de partición) 
![Imagen](cassandra4.png)
## Replica
La replicación se refiere al número de copias que se mantienen para cada fila en uin conjunto de datos. La replicación proporciona redundancia de datos para tener tolerancia a fallas. 
En esta operación existe un factor de replicación el cual realiza las copias dependiendo el valor de ese factor. Un ejemplo sería tener como factor de replicación el valor de 3 lo que implica que se tienen 3 copias de datos en el sistema. Entonces, si existiera el caso en el que 2 máquinas se encuentren inactivas, todavía se puede acceder a los datos desde la tercera copia. 
Por defecto, el factor de replicación predeterminado es 1. Este factor significa que solo se tiene una copia de los datos, por lo que si el nodo que tiene los datos falla, este los perderá.

Cassandra permite la replicación basada en nodos, racks y centros de datos. La replicación en los centros de datos garantiza la disponibilidad de los datos incluso cuando un centro de datos no funciona. 

