

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
