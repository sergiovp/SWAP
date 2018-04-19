# SWAP
## Práctica 3. Balanceo de carga.
### Sergio Vela Pelegrina.
En esta práctica, configuraremos una red entre varias máquinas de forma que tengamos un balanceador que reparta la carga entre varios servidores finales.
Balancearemos los servidores **HTTP** que tenemos configurados, de forma que conseguiremos una infraestructura redudante y de alta disponibilidad.

Existen dispositivos hardware específicos para realizar el balanceo de carga, pero en nuestro caso, optaremos por una solución software para realizar esta tarea.

Para balancear la carga entre varios servidores, utilizaremos **DNS**.

Podríamos dividir la práctica en tres tareas básicas:
+ Configurar una máquina e instalarle **nginx**.
+ Configurar una máquina e instalarle **haproxy**.
+ Comparación de prestaciones entre ambos balanceadores.

## 1. Configurar una máquina e instalarle **nginx**.

En esta primera parte de la práctica, debemos configurar una tercera máquina que se utilizará como balanceador de carga.
Tras la instalación de **Ubuntu Server** en esta tercera máquina, procedemos a la instalación de nginx mediante el comando `sudo apt-get install nginx`.
Hay que tener en cuenta que esta máquina, no deberá tener instalado apache, con el fin de tener libre el **puerto 80**.
Ahora, empezaremos con la configuración de **nginx** como balanceador de carga.
Para ello, he tenido que configurar el fichero `/etc/nginx/conf.d/default.conf` tal y como se indica en el guión de prácticas.

Ya estaría funcionando correctamente, y por tanto, podemos hacer peticiones a la IP de la máquina balanceadora usando **curl** como veremos en la siguiente imagen:

![nginx](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%203/images/nginx_funcionando.png)
###### Figura 3.1. **nginx** funcionando.

## 2. Configurar una máquina e instalarle **haproxy**.

En este caso, haremos el mismo procedimiento que la parte anterior, pero en lugar de **nginx** para el balanceo, utilizaremos **haproxy**.

Tal y como se nos indica en el guión, comenzaremos con la instalación.
Para instalar **haproxy** simplemente usaremos el comando `sudo apt-get install haproxy`.


Una vez instalado, debemos modificar el archivo `/etc/haproxy/haproxy.cfg` tal y como se nos explica en el guión, ya que la configuración que nos trae por defecto no nos vale.

De haberlo hecho bien, ya deberíamos tenerlo funcionando y podríamos hacerle peticiones como veremos a continuación:

![haproxy](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%203/images/haproxy_funcionando.png)
###### Figura 3.2. **haproxy** funcionando.

## 3. Someter a una alta carga el servidor de balanceo y comparar prestaciones.

Para medir el rendimiento de un servidor necesitaremos una herramienta que ejecutar en los clientes para crear una carga HTTP específica.

Existen diversas herramientas para comprobar el rendimiento de servidores web, en nuestro caso usaremos **Apache Benchmark**.

Es conveniente ejecutar los benchmark en otra máquina diferente a las que forman parte de la granja web.
Para utilizar **Apache Benchmark**, ejecutaremos el siguiente comando `ab -n 1000 -c 10 http://172.20.10.X/hola.html`.
Dónde los parámetros son los siguientes:
Se solicita la página con dirección http://172.20.10.X/hola.html 1000 veces (-n 1000 indica el número de peticiones) y hacer esas peticiones concurrentemente de 10 en 10 (-c 10 indica el nivel de concurrencia).