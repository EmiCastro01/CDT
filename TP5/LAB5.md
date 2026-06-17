# **Trabajo Práctico N°5**

**Integrantes**:  
- _Dario Castillo_
- _Emiliano Castro_
- _Gabriel Arrieta_
- _Priscila Martinez_
 
**Grupo: CDT** 

**Facultad de Ciencias Exactas, Físicas y Naturales**  

**Materia**: _Redes de Computadoras_  
**Profesores**: _Santiago M. Henn_, _Facundo O. Cuneo_  
**Fecha**: 17/06/2026  

---

***Objetivos:***
- Comprender cómo una arquitectura de servicios responde ante distintos tipos de
tráfico.
- Relacionar componentes de infraestructura cloud con conceptos de redes: ruteo,
balanceo de carga, almacenamiento, bases de datos, caché, colas y filtrado de tráfico
malicioso.
- Analizar fallas, cuellos de botella y decisiones de escalabilidad.

***Requisitos:*** 
- Computadora(s) con acceso a internet.
- Navegador web moderno.

## Contexto

En este laboratorio vamos a usar Server Survival como simulador de infraestructura. El juego representa un
sistema que recibe tráfico de distintos tipos y que debe procesarlo correctamente manteniendo presupuesto,
reputación y salud de los servicios.
Aunque el entorno es lúdico, los conceptos son reales: un sistema mal diseñado puede fallar por sobrecarga, mala
distribución del tráfico, ausencia de caché, falta de filtrado ante ataques o una base de datos saturada.

## Despliegue del juego

Utilizamos el repositorio desplegado https://pshenok.github.io/server-survival/ 

# 1) Reconocimiento de arquitectura



- ## Firewall
```planetext
SEGURIDAD. Modulo que bloquea primera linea de trafico malicioso.
```
- **Resuelve el problema de** proteger la red interna filtrando el tráfico entrante y saliente no autorizado mediante reglas de seguridad basadas en IPs, puertos o estados de conexión.

- **Se encuentra en** la capa TCP/IP: Principalmente en la **Capa de Internet** (filtrado por IP) y Capa de Transporte (filtrado por puertos TCP/UDP). Los firewalls modernos de aplicación (WAF) llegan hasta la Capa de Aplicación.

- **Si falta en la arquitectura**: La arquitectura queda vulnerable a ataques de fuerza bruta, escaneos de puertos, inyecciones de código directo y accesos no autorizados

- ## Load Balancer(Balanceador de Carga)
```planetext
BALANCEADOR DE CARGA: funcion de Distribucion de multiples instancia de trafico
```
- **Resuelve el problema de** distribuir las peticiones entrantes de los usuarios de forma equitativa entre varios servidores para evitar que uno solo se sature (alta disponibilidad y escalabilidad).

- **Se encuentra en** la capa TCP/IP: Principalmente en la **Capa de Transporte** (Capa 4 - balanceo por IP y puerto) o en la Capa de Aplicación (Capa 7 - balanceo basado en URLs o cookies).

- **Si falta en la arquitectura**: Un pico repentino de tráfico tumbará el único servidor disponible, generando indisponibilidad del servicio (Single Point of Failure).

- ## Queue (Cola de Mensajes)
```planetext
Cola: Funcionde BUFFER. Es requerido en momentos de SPIKE para prevenir colapso de eventos. El trafico es almacenado en un buffer.
```
- **Resuelve el problema de**: Permite la comunicación asincrónica y el desacoplamiento entre servicios. Absorbe picos de carga acumulando tareas pendientes para que los servidores las procesen a su propio ritmo.

- **Se encuentra** en la capa TCP/IP: **Capa de Aplicación**. Corre sobre protocolos como AMQP o MQTT.

- **Si falta en al arquitectura:** El sistema se vuelve estrictamente sincrónico; si el servidor de procesamiento se satura o se cae, las peticiones de los usuarios se perderán inmediatamente.

- ## Compute (Instancia de Cómputo / Servidor)
```planetext
Funcion Procesar datos. 
```
- **Resuelve el problema de:**: Proporcionar la infraestructura base de hardware virtual (CPU, RAM, disco) necesaria para ejecutar sistemas operativos, aplicaciones y lógica de negocio backend de forma persistente.

- **Se encuentra en** Capa TCP/IP: Atraviesa **todas las capas**, ya que contiene el sistema operativo que maneja desde los drivers de red (Capa de Acceso a Red) hasta los daemons de software (Capa de Aplicación).

- **Si falta en al arquitectura:** No hay dónde ejecutar el código principal de la aplicación ni hospedar el software de forma continua.

- ## Serverless Function
```planetext
Pay-per-use Compute. Auto-scales with traffic. Low upkeep, but charges $0.03 per request. Cheap when idle, expensive at high RPS.
```
- **Resuelve el problema de:** Permitir ejecutar fragmentos de código bajo demanda guiados por eventos (ej. redimensionar una imagen al subirse) sin tener que pagar ni administrar un servidor encendido las 24 horas.

- **Se encuentra en** Capa TCP/IP: **Capa de Aplicación**. Su ciclo de vida es efímero y se activa por peticiones a nivel de software.

- **Si falta en la arquitectura:** Tendrías que hospedar ese micro-servicio en un servidor tradicional (Compute), pagando costos fijos por tiempo ocioso y administrando manualmente el escalado del sistema.

- ## SQL DB (Base de Datos Relacional)
```planetext
Database. Handles READ/WRITE/SEARCH. Upgradeable T1→T3.
```
- **Resuelve el problema de:** Garantizar la persistencia de datos estructurados asegurando integridad, relaciones complejas y transacciones seguras bajo propiedades ACID (ej. transacciones bancarias).

- **Se encuentra en** la capa TCP/IP: **Capa de Aplicación**. (El motor procesa queries SQL y expone un puerto de red en la capa de transporte).

- **Si falta en la arquitectura:** No se pueden almacenar datos de forma estructurada ni relacional; se perdería la consistencia estricta requerida para datos críticos de los usuarios.

- ## NoSQL (Base de Datos No Relacional)
```planetext
Fast Database. Handles READ/WRITE only (no SEARCH). Upgradeable T1→T3.
```

- **Resuelve el problema de**: Almacena volúmenes masivos de datos no estructurados o semiestructurados (JSONs, clave-valor) permitiendo una altísima velocidad de lectura/escritura y escalabilidad horizontal simple.

- **Se encuentra en** capa TCP/IP: **Capa de Aplicación.**

- **Si falta en la arquitectura:** El sistema sufriría cuellos de botella severos si intentás forzar estructuras dinámicas y masivas (como logs o carritos de compra) dentro de una base SQL tradicional.

- ## Cache (Memoria Caché)
```planetext
Caching. Caches responses to reduce DB load. Upgradeable T1→T3.
```
- **REsuelve el problema de:** Reduce la latencia guardando temporalmente en memoria RAM las respuestas a consultas frecuentes, evitando tener que calcularlas o pedirlas a la base de datos principal una y otra vez.

- **Se enceuntra en** capa TCP/IP: **Capa de Aplicación.**

- **Si falta en arquitectura:** La base de datos principal colapsará por la cantidad de consultas repetitivas y los tiempos de respuesta para el usuario final serán mucho más lentos.

- ## CDN (Content Delivery Network)
```planetext
Delivery. Caches STATIC content at edge (95% hit rate).
```
- **Resuelve el problema de:** Distribuir geográficamente réplicas del contenido estático (imágenes, HTML, CSS) en servidores cercanos al usuario para acelerar la carga de la web a nivel mundial.

- **Se encuentra en** Capa TCP/IP: **Capa de Aplicación** (aunque optimiza el enrutamiento y la latencia interactuando fuertemente con la capa de Internet).

- **Si falta en arquitectura:** Los usuarios que estén físicamente lejos del servidor central experimentarán una lentitud extrema al cargar la página por culpa de la distancia geográfica (latencia).

- ## Storage (Almacenamiento de Objetos)
```planetext
	File System. Destination for STATIC/UPLOAD traffic.
```
- **Resuelve el problema de:** Permitir almacenar y servir archivos masivos y no estructurados de forma económica y altamente disponible (ej: imágenes de perfil, PDFs, videos, backups).

- **Se encuentra en** capa TCP/IP: **Capa de Aplicación** (se accede comúnmente vía APIs HTTP/HTTPS).

- **Si falta en arquitectura:** Tendrías que guardar los archivos subidos por los usuarios dentro del disco local de los servidores de cómputo, agotando rápidamente su espacio y rompiendo el principio de servidores sin estado (stateless).

- ## Search Engine (Motor de Búsqueda)
```planetext
Search. Specialized for SEARCH queries. 3x faster than SQL DB. Upgradeable T1→T3.
```
- **Resuelve el problema de**: Permitir realizar búsquedas complejas de texto completo (full-text search), filtros avanzados y sugerencias tipográficas a gran velocidad sobre millones de datos (ej. buscadores de e-commerce).

- **Se encuentra en:** capa TCP/IP: **Capa de Aplicación.**

- **Si falta en arquitectura:** Las búsquedas dentro de la aplicación serían extremadamente lentas e ineficientes, obligando a usar comandos pesados (LIKE o REGEX) directamente en la base de datos principal.


- ## Réplica (Database Replica)
```planetext
Read Offload. Offloads READ from master DB. Requires DB connection. Upgradeable T1→T3.
```

- **Resuelve el problema de:** Copia los datos de la base de datos principal a nodos secundarios para derivarles el tráfico de lectura, liberando al nodo principal y asegurando que los datos sigan disponibles si este falla.

- **Se encuentra en** capa TCP/IP: **Capa de Aplicación** (utiliza protocolos propios del motor de base de datos para la sincronización).

- **Si falta en arquitectura:** Si la base de datos principal recibe muchas lecturas se saturará rápidamente, y ante una caída del nodo principal, todo el sistema dejará de funcionar por completo.


---


# 2) Tipos de tráfico


### Tabla de Tipos de Tráfico (Server Survival)

| Tipo de Tráfico | Ejemplo Real | Componente Recomendado | Riesgo si se procesa incorrectamente |
| --- | --- | --- | --- |
| **STATIC** | Archivos HTML, imágenes de la interfaz, hojas de estilo CSS o scripts de JavaScript. | **CDN** o **Storage** | En el juego, si mandás STATIC a los servidores de cómputo (`Compute`), les agotás la capacidad de CPU/RAM inútilmente, provocando que colapsen cuando llegue tráfico pesado. |
| **READ** | Consultar el catálogo de productos, leer publicaciones en un feed de inicio o ver perfiles. | **Cache** o **Réplica** de Base de Datos. | Sobrecarga de consultas repetitivas en la base de datos principal (`SQL DB`), aumentando su tiempo de respuesta y bloqueando las escrituras. |
| **WRITE** | Crear un nuevo usuario, confirmar un pago o guardar una publicación nueva. | **Queue** acoplada a una **SQL DB** (o **NoSQL**). | Si la base de datos procesa demasiadas escrituras simultáneas directas sin una cola (`Queue`) que regule el flujo, el componente se satura y se pierden transacciones críticas. |
| **UPLOAD** | Subir fotos de perfil, adjuntar documentos PDF o subir videos a la plataforma. | **Storage** | Guardar archivos directamente en el disco del servidor (`Compute`) agota su almacenamiento local instantáneamente, dejando el sistema operativo fuera de servicio. |
| **SEARCH** | Buscar palabras clave como "Zapatillas Adidas" o aplicar filtros complejos en un buscador. | **Search Engine** | Las búsquedas de texto plano con `LIKE` o `REGEX` son pesadísimas para una base de datos relacional. Si no usás un motor de búsqueda indexado, la base de datos principal se congela. |
| **MALICIOUS** | Ataques de Denegación de Servicio (DDoS), inyecciones de código (SQLi) o intentos de hackeo. | **Firewall** | El tráfico malicioso saturará y quemará cualquier componente de tu arquitectura. Si no es bloqueado inmediatamente en la entrada por el Firewall, tira todo el sistema abajo. |

---


