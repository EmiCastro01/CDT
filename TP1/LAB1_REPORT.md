# **Trabajo Práctico N°1**

**Integrantes**:  
- _Dario Castillo_
- _Emiliano Castro_
- _Gabriel Arrieta_
- _Priscila Martinez_
 
**CTD**  

**Facultad de Ciencias Exactas, Físicas y Naturales**  

**Materia**: _Redes de Computadoras_  
**Profesores**: _Santiago M. Henn_, _Facundo O. Cuneo_  
**Fecha**: XX/03/2026  

---

## Resumen

Este trabajo práctico se divide en dos partes. En la primera se simuló el envío y recepción de paquetes IP encapsulados en frames Ethernet a través de una red con múltiples subredes, donde cada integrante asumió un rol concreto, host o default gateway. Además, el recorrido de los paquetes involucró tres routers intermedios, lo cual permitió observar el comportamiento del ruteo en una topología real. En la segunda parte se analizó la integridad de los datos transmitidos, trabajando con mecanismos de detección de errores sobre un canal de comunicación no confiable donde un intermediario podía alterar los mensajes transmitidos. Esto permitió analizar las capacidades y limitaciones de distintos algoritmos de detección de errores.

---

## Introducción

Cuando dos dispositivos se comunican a través de una red, los datos no viajan de forma directa ni garantizada. En su recorrido pasan por múltiples dispositivos intermedios, cada uno tomando decisiones locales sobre a dónde reenviar la información, modificando las cabeceras del paquete en cada salto sin alterar su contenido. Al mismo tiempo, el canal puede introducir alteraciones que comprometan la integridad de los datos transmitidos, haciendo necesario contar con mecanismos que permitan detectarlas

Este trabajo práctico aborda ambos aspectos. Inicialmente se estudia el mecanismo de ruteo y encapsulación, analizando cómo las direcciones MAC e IP cumplen roles distintos durante el recorrido de un paquete. Luego se trabaja con algoritmos de detección de errores, evaluando su comportamiento frente a mensajes potencialmente alterados. 


---

## Desarrollo | Primera parte

---
## Desarrollo | Segunda parte

## Referencias

[1] [Stallings - Comunicaciones y Redes de Computadores 7ed]

