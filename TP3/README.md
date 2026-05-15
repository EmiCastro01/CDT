#  **Trabajo Práctico N°3**

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


## Consignas
## ***1) Investigación conceptual (respuestas breves). Responder en forma concisa.***

***a) ¿Qué es SSH y qué problema resuelve?***

SSH (secure Shell) es un protocolo de administración remota que permite a los usuarios controlar y modificar sus servidores a través de internet. Resuelve el problema de sustituir protocolos antiguos e inseguros como Telnet o rlogin, que enviaban la información (incluidas las contraseñas) en texto plano. SSH garantiza que la comunicación sea segura mediante el uso de criptografía.

***b) Diferencia entre autenticación y cifrado***

<div align="center">
    <img src="img/fig1b.png">
</div>

Autenticación es el proceso de verificar la identidad del usuario. Se logra mediante contraseñas, claves SSH o biométricos.
Cifrado es el proceso de codificar los datos transmitidos para que, si alguien intercepta la comunicación, no pueda leer el contenido. Protege la confidencialidad de la información.

***c) ¿Qué es una clave pública y una clave privada?***

Son los dos componentes de un par de claves criptográficas asimétricas.
Clave Pública es como un “candado” que se coloca en el servidor. Se puede compartir libremente con cualquier persona.
Clase Privada es la ”llave” física que abre ese candado. Solo el dueño de la clave debe poseerla,

***d) ¿Por qué la clave privada no debe compartirse?***

Porque la seguridad de todo el sistema reside exclusivamente en el secreto de esta clave. Si alguien obtiene tu clave privada, puede suplantar tu identidad y acceder a todos los servicios autorizados  sin necesidad de conocer una contraseña, ya que la clave privada es la prueba definitiva de propiedad.

***e) ¿Qué ventajas tienen las claves SSH frente a contraseñas?***

Inmunes a fuerza bruta, son mucho más largas y complejas que cualquier contraseña humana.
No se transmiten, a diferencia de las contraseñas, la clave privada nunca viaja por la red durante el proceso de autenticación.
Automatización, permiten el inicio de sesión seguro en scripts y procesos automatizados sin intervención manual.
Gestión centralizada, es más fácil revocar el acceso a una clave pública especifica que cambiar una contraseña compartida.

<div align="center">
    <img src="img/fig1e.png">
</div>

<div align="center">
    <img src="img/fig1e2.png">
</div>


```bash
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACCaPP+eX9uyzPUchnoHz3qcMixVNolxISrmvy2p36mkIgAAAKBk+UVfZPlF
XwAAAAtzc2gtZWQyNTUxOQAAACCaPP+eX9uyzPUchnoHz3qcMixVNolxISrmvy2p36mkIg
AAAECX9+v7gNv9s2R7baX6SXPDd6f7OCUkswyTTwbqkQOICJo8/55f27LM9RyGegfPepwy
LFU2iXEhKua/LanfqaQiAAAAGHNhbnRpYWdvQExBUFRPUC1DTzVJT05GVgECAwQF
-----END OPENSSH PRIVATE KEY----- 
```
---
```bash
Clave privada RSA
Fortaleza: 3072 bits
```
---

```bash
Algoritmo:	RSA
Tamaño:	3072
Huellas
SHA1:	EE D4 6C 91 17 C1 D5 50 E3 07 16 CF 9C A8 B4 93 C6 3D 2C 5A
SHA256:	6E F7 AD 28 D3 3A 81 E9 A2 69 E2 FD D2 81 71 BD D7 C4 AA BF 20 3E 59 1F 9B FA FF 49 4C 87 A1 E2
```
---
## ***2) Verificar conexión SSH con alguna de las VMs que reservaron. Documentar su paso por la VM creando una carpeta con el nombre de su grupo.***



<div align="center">
    <img src="img/fig2.png">
</div>

```bash
dario@dario-N-737R:~/Escritorio/tp3redesdecompu$ chmod 400 pc3_key.pem 
dario@dario-N-737R:~/Escritorio/tp3redesdecompu$ chmod 400 pc4_key


dario@dario-N-737R:~/Escritorio/tp3redesdecompu$ ssh -i pc3_key.pem pc-alumnos-3@4.206.219.90
Linux redes-de-computadoras-pc3 6.1.0-44-cloud-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.164-1 (2026-03-09) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Apr 22 22:02:34 2026 from 181.9.227.233
```



dentro:


```bash
pc-alumnos-3@redes-de-computadoras-pc3:~$ ls
enredados  ethernautas_v2
pc-alumnos-3@redes-de-computadoras-pc3:~$ mkdir CDT
pc-alumnos-3@redes-de-computadoras-pc3:~$ ls
CDT  enredados  ethernautas_v2
```


## ***3- Usando Wireshark, capturar tráfico SSH y analizar alguno de los paquetes. ¿Podés descifrar el contenido?***

<div align="center">
    <img src="img/fig3.png">
</div>


Al iniciar wireshark con filtro en ssh no vemos trafico. Pero empezamos a notarlo cuando escribimos un comando en la VM . enviando comandos o solo con escribir en la terminal estabamos generando trafico.


<div align="center">
    <img src="img/fig3a.png">
</div>

---

## ***4-  Nuestras VMs (virtual machines) están corriendo un SO Debian en una arquitectura x64. Vamos a utilizar netcat para desplegar servidores simples y capturar tráfico. Instalar netcat en la VM si no está instalado (sudo apt install ncat) y en sus computadoras locales. Luego:***

```bash
sudo apt update && sudo apt install ncat 
```
- Instalar ncat en vm y pc local. Luego:

### ***a) ESCUCHANDO CONEXIONES TCP:***
```bash
pc-alumnos-3@redes-de-computadoras-pc3:~/CDT$ ncat -l 5005
```

- Luego en wireshark fltramos el trafico con ***tcp.port == 5005***

- Localmente conectamos nuestra pc local el cliente y conectamos con la ip de la VM:

```bash
dario@dario-N-737R:~/Escritorio/tp3redesdecompu$ ncat 4.206.219.90 5005
```


- Luego mandamos mensajes desde la terminal local y recibiremos desde la terminal de la VM O VICEVERSA


<div align="center">
    <img src="img/fig4.png">
</div>

<div align="center">
    <img src="img/fig4b.png">
</div>

Podemos ver en wireshark los paquetes TCP descifrados que hemos transmitidos y recibidos por el canal.
