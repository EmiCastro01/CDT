# **Trabajo Práctico N°4**

**Integrantes**:  
- _Dario Castillo_
- _Emiliano Castro_
- _Gabriel Arrieta_
- _Priscila Martinez_
 
**CDT** 

**Facultad de Ciencias Exactas, Físicas y Naturales**  

**Materia**: _Redes de Computadoras_  
**Profesores**: _Santiago M. Henn_, _Facundo O. Cuneo_  
**Fecha**: XX/03/2026  

---

## Resumen
Este trabajo práctico se centró en el despliegue y comunicación a través de un servidor TCP multi-hilo, incorporando además una capa de seguridad mediante cifrado. Para ello inicialmente se desplegó un servidor multi-hilo y se verificó su funcionamiento enviando mensajes serializados en formato JSON mediante PacketSender. Luego, se desarrolló una aplicación de cliente propia, capaz de establecer conexión con un servidor y serializar la información a transmitir en formato JSON antes de enviarlos. Finalmente, se incorporó una capa de seguridad cifrando la carga útil de los mensajes con AES-CBC, implementando el cifrado del lado del cliente y el descifrado del lado del servidor.
---

## Introducción

La comunicación entre aplicaciones distribuidas requiere tanto de un protocolo de transporte que garantice la entrega confiable de los datos, como de un formato estructurado que permita la interpretación de la información transmitida. Para comprender esto, se desplegó un servidor TCP multi-hilo y se construyó una aplicación de cliente capaz de comunicarse con él, lo que permitió poner en práctica el proceso de serialización dentro de una comunicación real entre dos equipos. Por último, para comprender cómo se podía proteger esa información, se incorporó cifrado sobre la carga útil de los mensajes, implementando tanto el cifrado del lado del cliente como el descifrado del lado del servidor.

---
## Desarrollo | 
### Marco Teórico 
### Serialización en redes.

Es un proceso que ocurre en la capa de aplicación donde una estructura de datos (objeto, diccionario, clase, etc) se convierte en un formato común y acordado para transmitirse por la red. Este proceso es necesario porque de lo contrario, el intercambio de datos estructurados entre programas sería imposible, ya que cada lenguaje y sistema representa los objetos en memoria de forma distinta.

### Serialización binaria y no binaria.

Según el formato en que se realice esa conversión, se distinguen dos tipos de serialización:

- **Serialización binaria:** Convierte los datos en una secuencia de bytes compacta y no legible bajo un formato (ya sea Protocol Buffers, MessagePack, BSON, u otro). Como resultado, los mensajes son eficientes en tamaño y velocidad de procesamiento, lo que la hace ideal para sistemas donde el rendimiento y el ancho de banda son críticos. Sin embargo, tanto para deserializar como para inspeccionar los datos, se requiere la librería específica del formato utilizado. Se usa comúnmente en comunicación entre microservicios, donde la latencia y el volumen de datos son críticos. Por ejemplo, Google desarrolló Protocol Buffers para que sus servicios intercambien mensajes por segundo de forma eficiente.

- **Serialización no binaria:** Convierte los datos a texto plano estructurado y legible directamente en formato por ejemplo: JSON, XML, YAML, entre otros, lo cual facilita el desarrollo y depuración, ya que puede inspeccionarse a simple vista en logs o herramientas como Wireshark. Sin embargo, los mensajes tienen mayor tamaño, por ende su procesamiento es más costoso en sistemas de alto volumen. Se suele usar en APIs, por ejemplo, cuando una tienda online consulta el stock de un producto, el servidor responde con un JSON con el precio y disponibilidad
### Ventajas y desventajas

| Serialización | Ventajas | Desventajas |
|---|---|---|
| **Binaria** | - Eficiente para sistemas que manejan grandes volúmenes de datos simultáneos <br> <br> - Menor uso de memoria durante la transmisión | - Menor portabilidad en algunos casos, por ejemplo pickle solo funciona en Python <br> <br> - Si el esquema del formato cambia, puede romper la compatibilidad entre versiones |
| **No binaria** | - Compatible con cualquier lenguaje o plataforma sin configuración adicional <br> <br> - Al ser texto plano, es fácil de versionar y comparar cambios en herramientas como Git | - Puede volverse confuso y difícil de mantener en estructuras de datos muy complejas <br> <br> - Mayor consumo de CPU al parsear en sistemas con muchas solicitudes concurrentes |
