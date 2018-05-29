# Ejercicios de teoría tema 2.

## 2.1. Calcular la disponibilidad del sistema si tenemos dos réplicas de cada elemento (en total 3 elementos en cada subsistema).
+ **Web**: 97'75 + (1 - 97'75) * 85 = **99'6625**
+ **Application**:  99 + (1 - 99) * 90 = **99'9**
+ **Database**: 99.9999 + (1 - 99'9999) * 99'9 = **99'99999**
+ **DNS**: 99.96 + (1 - 99'96) * 98 = **99'9992**
+ **Firewall**: 97'75 + (1 - 97'75) * 85 = **99'6625**
+ **Switch**: 99.99 + (1 - 99'99) * 99 = **99'9999**
+ **ISP**: 99'75 + (1 - 99'75) * 95 = **99'9875**

**Disponibilidad**: 0,996625 * 0,999 * 0,9999999 * 0,999992 * 0,996625 * 0,999999 * 0,999875 = **99,2034961%**

## 2.2. Buscar frameworks y librerías para diferentes lenguajes que permitan hacer aplicaciones altamente disponibles con relativa facilidad.

#### Laravel
**Laravel** maneja una sintaxis expresiva, elegante, con el objetivo de eliminar la molestia del desarrollo web facilitando las tareas comunes, como la autenticación, enrutamiento, sesiones y caché. Proporciona, potentes herramientas accesibles necesarias para construir grandes aplicaciones robustas, se puede utilizar para aplicaciones de nivel empresarial enormes ó simples usando la API JSON, lo que significa que es perfectamente adecuado para todos los tipos y tamaños de proyectos.

#### CodeIgniter
**CodeIgniter** cuenta con un amplio conjunto de librerías para tareas comúnmente necesarias, así como una interfaz sencilla y la estructura lógica para acceder a estas bibliotecas. Es excepcionalmente rápido, ya que su sistema central sólo requiere algunas pequeñas bibliotecas, con bibliotecas adicionales cargadas dinámicamente a petición, con base en sus necesidades de un determinado proceso. Esto significa que el sistema base es ágil y flexible.

#### NKSIP
**NkSIP** es un framework software libre para el desarrollo de cualquier tipo de aplicaciones SIP, de forma sencilla, escalable y altamente disponible.
Facilita en gran medida el desarrollo de aplicaciones del lado del servidor como proxy.

#### Zend Framework 2
Es un FrameWork de código abierto para desarrollar aplicaciones web, utilizando el código orientado a objetos. Los componentes de la biblioteca estándar forman una poderosa herramienta extensible cuando se combinan, ofreciendo una aplicación MVC de alto rendimiento y bastante robusta. Es altamente adaptable a tus necesidades, con una base modular para que pueda usar bloques de construcción en combinación con otros FrameWorks.

## 2.3. ¿Cómo analizar el nivel de carga de cada uno de los subsistemas en el servidor? Buscar herramientas y aprender a usarlas.
+ En Linux podemos usar el comando **top** que nos muestra, entre otras cosas, el uso de CPU y de memoria que están haciendo los procesos se están ejecutando en nuestra máquina.

+ Por otro lado, en Windows tenemos la herramienta Performance Monitor para obtener esta información con un entorno gráfico, además podemos extraer datos de ella de forma automática.

+ También tenemos otra herramienta, **Gnome System Monitor**, que muestra qué programas están en ejecución y cuánto procesador, tiempo, memoria y espacio en disco están usando.

## 2.4. Buscar información sobre:
    
#### 2.4.1. Ejemplos de balanceadores software y hardware (productos comerciales):
+ Software:
        + HAProxy, Pound, Nginx
    + Hardware:
        + Cisco, LoadMaster

#### 2.4.2. Productos comerciales para servidores de aplicaciones:
+ WebSphere, Internet Information Server

#### 2.4.3. Productos comerciales para servidores de almacenamiento:
+ De la compañía **DELL** podemos encontrar lo siguiente:
        + PowerVault MD1200, PowerVault MD1220, PowerVault MD3060e denso, Chasis denso Dell Storage MD1280, entre otros.