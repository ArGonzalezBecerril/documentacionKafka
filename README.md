olid](https://image.ibb.co/bSHK4b/apache_kafka.png)](https://github.com/ArturoGonzalezBecerril/kafka-Instalacion-cluster)

# Que es?
-  Es un sistema de mensajería basado en el modelo publicador/susbcriptor, persistente, escalable, replicado, tolerante a fallos.


# Comandos para la administración de Kafka

## Servicios Kafka y Zookeeper

Iniciar el servidor zookeeper
```scala
eglobal@arturo$ ./bin/zookeeper-server-start.sh config/zookeeper.properties
// Inicia el servidor zookeeper con la configuración de zookeeper.properties
```
Iniciar el servicio kafka-server
```scala
eglobal@arturo$ ./bin/kafka-server-start.sh config/server.properties
// Inicia el servicio kafka con la configuración de server.properties
```
## Topics(Temas)
Crear un nuevo Topic:
```scala
eglobal@arturo$ kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic NombreTopic
// Crea un topic con una particion de 3 y un factor de replicación de 1
```
Listar  todos los Topics(Temas)
```scala
eglobal@arturo$ kafka-topics --list --zookeeper localhost:2181
```
Particiones en Kafka
```scala
eglobal@arturo$ kafka-topics --zookeeper localhost:2181 --alter --topic Test --partitions 4
//Modifica el numero de particiones de Test a 4
```
Eliminar un Topic
```scala
eglobal@arturo$ kafka-topics --zookeeper localhost:2181 --delete --topic Test
```
Mostrar los detalles de todos los Topics
```scala
eglobal@arturo$ kafka-topics --zookeeper localhost:2181/kafka-cluster --describe
```
Mostrar las particiones replicadas de cada topic
```scala
eglobal@arturo$ kafka-topics --zookeeper localhost:2181/kafka-cluster --describe --under-replicated-partitions
```
## Configuración de Productores(Producer)
```scala
eglobal@arturo$ kafka-console-producer --broker-list localhost:9092 --topic Test < "hola desde kafka"
//Productor -> Envia un msg, y lo publica en el topic Test
```
#### Productor en Python
```ssh
eglobal@arturo$ sudo apt-get install python-pip
eglobal@arturo$ pip install kafka-python
eglobal@arturo$ touch producer.py
```
##### Código
```python
from kafka import KafkaProducer
producer = KafkaProducer(bootstrap_servers='localhost:9092')
for _ in range(2):
    producer.send('foobar', b'Producer msg ejemplo')
```
**Nota:** Cualquier lenguaje que tenga el API de implementación de Kafka puede fungir como Consumer o Producer

## Consumidores(Consumer)
```ssh
eglobal@arturo$ bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test
```
Consume un Msg de __consumer_offsets
```scala
eglobal@arturo$ kafka-console-consumer --bootstrap-server localhost:9092 --topic __consumer_offsets --formatter 'kafka.coordinator.GroupMetadataManager$OffsetsMessageFormatter' --max-messages 1
```
Consume 1 solo mensaje
```scala
eglobal@arturo$ kafka-console-consumer --zookeeper localhost:2181 --topic Test  --max-messages 1
```
#### Operaciones sobre los Consumer
Listar todos los grupos de Consumer
```scala
eglobal@arturo$ kafka-consumer-groups --new-consumer --list --bootstrap-server localhost:9092
```
## Aspectos de Configuración
Mostrar los cambios que se han realizado sobre un Topic en especifico.
```scala
eglobal@arturo$ kafka-configs --zookeeper localhost:2181 --describe --entity-type topics --entity-name Test
```
Metricas de Rendimiento
```scala
eglobal@arturo$ kafka-producer-perf-test --topic position-reports --throughput 10000 --record-size 300 --num-records 20000 --producer-props bootstrap.servers="localhost:9092"
```
## Parámetros de Configuración mas destacados
```scala
//****Retención de Mensajes
#En milisegundos
log.retention.ms=1680000
#En minutos
log.retention.minutes=3600
#En horas
log.retention.hours=2
***********************************
//Por defecto kafka desactiva, el poder eliminar un topic
#Cambiar a true si desea eliminar un topic desde linea de comandos
delete.topic.enable=true
***********************************

```
## Arquitectura de Kafka
[![N|Solid](https://image.ibb.co/n6PTVG/kafka_Arquitectura.png)](https://github.com/ArturoGonzalezBecerril/kafka-Instalacion-cluster)

Descripción
- Magic byte - Se usa para la versión del Protocolo.(1 byte)
- Mtype -Es un byte simple funge como indicador  insert(0x1), update(0x2), delete(0x3)
- SCMID -Id que se usa para los esquemas en Avro

## Conceptos
- **Mensaje:** Información que se intercambian entre Productor y Consumidor.
- **Topic:** Categorización de un Mensaje.
- **Particion:** Estructura de almacenamiento de mensajes.
- **Productor:** Entidad generadora de un mensaje.
- **Consumidor:** Entidad procesadora de mensajes.

Licencia
----
GNU

