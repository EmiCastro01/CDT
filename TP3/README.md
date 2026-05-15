
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
