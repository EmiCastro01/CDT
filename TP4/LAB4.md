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
---
### Despliegue de servidor TCP multi-hilo con Packet Sender
Para esta actividad, se utilizaron dos computadoras conectadas a la misma red local, una actuando como cliente y otra como servidor, donde:
En la computadora elegida como servidor se ejecutó el script propuesto de Python server.py, el cual implementa un servidor TCP multi-hilo que escucha en el puerto 5000 y es capaz de atender múltiples clientes de forma concurrente, asignando un hilo independiente a cada conexión. 
En la computadora cliente, en cambio, se configuró Packet Sender para interactuar con el servidor de forma externa. Para ello, se especificó la dirección IP de destino del servidor (192.168.0.16), el puerto (5000), se seleccionó TCP como protocolo de comunicación y se activó la opción Persistent TCP para mantener la conexión continua sin necesidad de abrir y cerrar el socket por cada mensaje a enviar. El payload se serializó manualmente en el campo ASCII utilizando el estándar JSON bajo el formato  `{"group": "<nombreGrupo>", "payload": "<Mensaje>"}`. Y al presionar Send, se tradujo automáticamente el string en su equivalente hexadecimal para transmitirse como un flujo de bytes a través de la red. 

<img width="528" height="148" alt="image" src="https://github.com/user-attachments/assets/eef4fba4-e7eb-461e-9730-51ca87b928ab" />

Una vez enviado el primer mensaje, se abrió una pestaña dedicada a la sesión activa y fue posible continuar enviando mensajes al servidor a través del mismo socket ya establecido. 

<img width="528" height="141" alt="image" src="https://github.com/user-attachments/assets/e4636b73-7f4b-4af7-85f2-fa5b88cd8a65" />

En el servidor se registró la conexión desde la IP del cliente (192.168.0.15), se procesó el flujo de bytes recibido, se deserializó el JSON y se mostraron los mensajes de forma limpia y ordenada. Finalmente, al cerrar la sesión desde el cliente, el hilo del servidor detectó la interrupción, liberó los recursos del socket y finalizó su ejecución mostrando un mensaje de despedida.

<img width="528" height="102" alt="image" src="https://github.com/user-attachments/assets/755a11d7-a9a3-48c9-9bf4-72ed3ed402ad" />

### Implementación de aplicación cliente

En este punto, se implementó un cliente en python diseñado para interactuar con el servidor TCP desplegado en el punto anterior. Para ello se extendió el script client.py propuesto, con una interfaz interactiva por consola que permite:   

- **Configuración de conexión:** al ejecutarse el script, solicita la dirección IP y el puerto del servidor y valida que el formato de la IP sea correcto y que el puerto se encuentre dentro del rango válido (1-65535) antes de intentar conectarse. En caso de producirse algún fallo (ya sea por timeout, conexión rechazada o formato inválido), se informa el motivo con un mensaje y el programa finaliza.                                 

- **Selección de modo de envío:** una vez que la conexión se establece, el menú permite elegir entre dos modalidades, ya sea mandar un mensaje único, donde se solicita el nombre del grupo y el payload, se transmite el mensaje, y luego retorna al menú principal o mantener la conexión TCP abierta de forma persistente, para ello, se solicita el nombre del grupo al inicio y permite enviar payloads de forma sucesiva utilizando el mismo socket y permite escribiendo la palabra exit volver al menú principal.

Independientemente del modo seleccionado, luego de establecer conexión con el servidor, el payload ingresado se estructura primero en un diccionario, se serializa mediante un JSON con el formato {"group": "...", "payload": "..."} , y finalmente se codifica como un flujo de bytes en UTF-8 antes de ser enviada por el socket. Esto garantiza compatibilidad con el servidor, que espera recibir y decodificar los datos en ese formato.

Para verificar el funcionamiento, se ejecutó en una computadora el servidor y en otra dentro de la misma red, se ejecutó el script del cliente y se estableció conexión con el servidor por 192.168.0.15:5000. Luego se seleccionó el modo de múltiples mensajes, se definió el nombre de grupo y se enviaron payloads de forma continua.

<img width="808" height="478" alt="Screenshot 2026-06-17 at 18-18-19 TP1-Redes de Computadoras - Documentos de Google" src="https://github.com/user-attachments/assets/33b3464d-43ef-4b37-b1ca-4f59d5fa2c7a" />


Por último, cuando se estableció la conexión, el servidor detectó la IP del cliente, mostró el mensaje de bienvenida y recibió el flujo de datos, verificó que contaran con las claves de diccionario correctas (group  y  payload), los deserializó e imprimió su contenido en pantalla.  

<img width="825" height="174" alt="Screenshot 2026-06-17 at 18-19-23 TP1-Redes de Computadoras - Documentos de Google" src="https://github.com/user-attachments/assets/82102db9-f17f-4566-9feb-0955fb6467fb" />

