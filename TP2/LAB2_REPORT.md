# **Trabajo Práctico N°2** 
**Integrantes**:  
- _Dario Castillo_
- _Emiliano Castro_
- _Gabriel Arrieta_
- _Priscila Martinez_
 
**CDT** 

**Facultad de Ciencias Exactas, Físicas y Naturales**  

**Materia**: _Redes de Computadoras_  
**Profesores**: _Santiago M. Henn_, _Facundo O. Cuneo_  

---

## Resumen

Este trabajo práctico se divide en dos partes. En la primera, el objetivo fue construir un cable de red Cat5/Cat5e bajo el estándar T568A/B, aprendiendo el proceso de crimpado y verificando su correcto funcionamiento tanto de forma visual como eléctrica mediante un tester, intercambiando además el cable con otro grupo para una revisión cruzada. En la segunda parte, el objetivo fue tomar contacto con equipamiento físico de red, para ello, se configuró un switch, se definieron direcciones IP para armar una red con otros grupos y se verificó la conectividad mediante pings. Como práctica adicional, se utilizó netcat para comunicar mensajes con los compañeros.

---

## Introducción

Para que una red sea confiable, es indispensable contar con una capa física correctamente implementada y verificada. Para comprender eso, se aplicó el estándar T568A/B para la confección de cables directos. Con los cables ya verificados, se trabajó con switches gestionables para configurar una red entre los distintos grupos y probar que la comunicación entre los hosts fuera exitosa. Esta práctica busca integrar tanto la correcta construcción del medio físico, como la administración del switch que opera a través de dicho medio.

---
## Desarrollo | Primera parte: Armado y verificación de cables Cat5/Cat5e bajo estándar T568A/B
## Marco Teórico 
Para entender el proceso de armado de un cable de red, es necesario conocer algunos conceptos importantes sobre los estándares, componentes y herramientas involucradas.

- **Estándar T568A/B:** T568A y T568B son los dos códigos de color utilizados para el cableado de conectores RJ-45 de ocho posiciones, ambos aceptados por la norma ANSI/TIA/EIA. Este estándar define el orden en que se ubican los conductores dentro del conector, asegurando que dos dispositivos distintos puedan comunicarse correctamente. La única diferencia entre ellos es que los pares naranja y verde están intercambiados, sin que esto afecte las características de transmisión. En Norteamérica se prefiere T568B, y en cualquier caso, ambos extremos del cable deben seguir el mismo estándar para obtener un cable directo.

<p align=center>
<img width="470" height="200" alt="imagen0" src="https://github.com/user-attachments/assets/9933073c-c435-43fa-8c60-f017a2338d76" />
</p>

- **Conectores RJ-45/jacks:** Son piezas de plástico transparente con ocho posiciones numeradas donde se insertan los conductores del cable UTP. Respetar el orden de los pines según el estándar elegido es importante para obtener óptimos resultados..

<p align=center>
<img width="470" height="186" alt="imagen2" src="https://github.com/user-attachments/assets/71b90087-e4e6-43d8-ac77-ea83808b5e28" />
</p>

- **Crimpeadora:** Es la herramienta utilizada para el armado del cable y tiene múltiples funciones repartidas a lo largo de toda la herramienta, como se puede ver en la figura. Cerca del mango (A) se encuentran las muescas para pelar cables de distintos grosores. En la zona central (B) están las muescas para crimpar terminales sin aislamiento, que son las que se utilizan para presionar el conector RJ-45 sobre los conductores y establecer la conexión eléctrica. Hacia la punta (C) se ubican las muescas codificadas por color para terminales con manguito aislante. Algunas crimpeadoras también incluyen un cortador de cable integrado.

<p align=center>
<img width="470" height="186" alt="imagen3" src="https://github.com/user-attachments/assets/2e42f2b3-7f51-4b3a-a71c-dff7ea1bc040" />
</p>

## Construcción de cable tipo DERECHO.

Los pasos seguidos para la construcción del cable fueron los siguientes:

**1°** Cortar el cable Cat5e a una longitud aproximada de 1 metro y pelar un extremo del cable con la crimpadora, sin dañar los hilos internos.

**2°** Desenredar y ordenar los cuatro pares de hilos según el estándar T568B y cortarlos al mismo nivel, dejando un par de centímetros de hilo expuesto.

<p align=center>
<img width="261" height="286" alt="imagen4" src="https://github.com/user-attachments/assets/68d703dd-935a-4331-a576-cd1bb85dd023" />
</p>

**3°** Una vez cortado los cables, introducir los hilos en el conector RJ-45, verificando que cada hilo llegue hasta el fondo del conector siguiendo el protocolo, luego con la crimpadora colocar la ficha jack y ajustar con fuerza para presionar los contactos y realizar la conexión. Lo mismo se realiza con el extremo opuesto siguiendo el mismo estándar. 

<p align=center>
<img width="279" height="261" alt="imagen5" src="https://github.com/user-attachments/assets/7552f083-a1c8-4d99-95fd-be5f74bc4bee" />
</p>


**4°** Como último paso, se verifica la conexión del cable con un tester para cables de red. Este aparato lo que hace es verificar la continuidad entre extremo a extremo de cada cable utp. Haciéndolo de forma secuencial en orden. Donde, a medida que la conexión es correcta el aparato enciende luces de manera creciente hasta llegar al final indicando la correcta conexión del cable.

<p align=center>
<img width="529" height="354" alt="imagen6" src="https://github.com/user-attachments/assets/122a7700-9990-41d5-a6a5-f5ebddfb1e30" />
</p>


### Acceso a administración y modificación de claves del switch

Una vez que aparece la consola, se puede verificar la configuración existente del switch con el comando `show running-config`. Dicho comando muestra la configuración completa del switch: contraseñas (cifradas o en texto plano si es que fueron configuradas), VLANs con sus respectivas interfaces, modos de los puertos (access, trunk), direcciones IP de interfaces de administración, etc. Lo cual es útil para evitar problemas con futuras configuraciones.
Para establecer o modificar las claves de acceso, desde la terminal se puede configurar de la siguiente forma:
```
Switch> enable                        ! Ingresar al modo privilegiado con enable 
Switch# configure terminal            ! Ingresar al modo de configuración global
Switch(config)# enable secret [PWD]   ! Configurar clave cifrada para modo privilegiado
Switch(config)# line console 0        ! Acceder a la configuración del acceso por puerto serie
Switch(config-line)# password [PWD]   ! Configurar clave para la consola
Switch(config-line)# login            ! Habilitar autenticación
Switch(config-line)# exit
Switch(config)# end
Switch# write memory                  ! Guardar la configuración
```

### Configuración de red y prueba de conectividad con pings

Para conectar las PCs al switch y verificar conectividad se hicieron los siguientes pasos:

Conectar cada computadora a un puerto del switch mediante el cable Cat5e armado en la primera parte. Acordar la subred a utilizar y definir para cada host una dirección IP.  Para probar la conectividad se puede usar el comando `ping` entre los hosts, lo cual muestra el tiempo de ida y vuelta (RTT) de cada paquete ICMP, eso confirmaría si la capa física y la capa de red funcionan correctamente de extremo a extremo.

## Conexión entre grupos y verificación de conectividad

Con los cables de la primera parte ya verificados, se procedió a conectar al mismo switch un cable por equipo en distintos puertos Fast Ethernet del switch. Se acordó usar la subred `192.168.2.X/24` y cada grupo configuró su dirección IP estática.  En nuestro caso, la dirección asignada fue `192.168.2.11`

Para configurar la IP dentro de la subred se asignó manualmente desde las configuraciones, configurando los siguientes campos:
<p align=center>
<img width="421" height="251" alt="image" src="https://github.com/user-attachments/assets/570b9744-2bc3-4868-be65-0d56486950dd" />    
</p>


Finalmente, para verificar la configuración aplicada con el comando `ip addr | grep .11`. Se filtraron las salidas y se encontró la dirección activa. Por último y como prueba de la conexión, se utilizó la herramienta `netcat` para abrir un socket y bindear un puerto para poder comunicarse con mensajes con los compañeros en la red. Para eso se utilizó:
```
nc -l 200
```
quedando a la escucha del puerto 2000. En cuanto un usuario de la red mandó un mensaje, se estableció un canal bidireccional, y la herramienta imprimió por `stdout` los mensajes que llegaron al socket.
