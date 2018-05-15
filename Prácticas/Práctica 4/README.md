# SWAP
## Práctica 4. Asegurar la granja web.
### Sergio Vela Pelegrina.
El objetivo de esta práctica es llevar a cabo la configuración de seguridad de la granja web. Para ello, llevaremos a cabo las siguientes tareas:

+ Instalar un certificado SSL para configurar el acceso HTTPS a los servidores.
+ Configurar las reglas del cortafuegos para proteger la granja web.

## 1. Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS.

Un certificado **SSL** sirve para proporcionar  seguridad al visitante de su página web.
Es una forma de decirle a los clientes que el sitio web es **auténtico**, **real** y **confiable** para ingresar *datos personales*.

**SSL** proporciona servicios de comunicación segura entre cliente y servidor y existen diversas formas de obtenerlo e instalarlo en nuestro servidor web, para ello, lo principal es conseguir un certificado que podremos conseguir de las siguientes formas:

+ Mediante una autoridad de certificación.
+ Crear nuestros propios certificados SSL auto-firmados usando la herramienta openssl.
+ Utilizar certificados del proyecto Certbot (antes Let’s Encrypt).

#### Generar e instalar un certificado autofirmado.
Únicamente debemos activar el módulo SSL de Apache, generar los certificados y especificarle la ruta a los certificados en la configuración. Así pues, como root ejecutaremos:
`
a2enmod ssl
service apache2 restart
mkdir /etc/apache2/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout
/etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt
`

Editamos el archivo de configuración del sitio default-ssl: `nano /etc/apache2/sites-available/default-ssl`

Y agregamos estas lineas debajo de donde pone SSLEngine on:
`
SSLCertificateFile /etc/apache2/ssl/apache.crt
SSLCertificateKeyFile /etc/apache2/ssl/apache.key
`

Activamos el sitio default--ssl y reiniciamos apache:
`
a2ensite default-ssl
service apache2 reload
`

Ya podemos hacer peticiones **HTTPS** mediante **curl** ejecutando `curl –k https://172.20.10.X/hola.html` 

![peticiones](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%204/images/https_VM1.png)
###### Figura 4.1. Peticiones HTTPS MV1.

![peticiones](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%204/images/https_VM2.png)
###### Figura 4.2. Peticiones HTTPS MV2.

Tal y como nos aparece en el guión y a modo de curiosidad, si accedemos al servidor con el navegador, en la barra de dirección aparece en rojo el **HTTPS**, ya que se trata de un certificado autofirmado cuya autoridad no reconoce.

![peticiones](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%204/images/https_rojo.png)
###### Figura 4.3. No es seguro.

Tras tener lo anterior, podemos configurar **nginx** en el servidor de balanceo de carga, con el fin de que pueda balancear tráfico **HTTPS**.

![peticiones](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%204/images/balanceo_https.png)
###### Figura 4.4. Balanceo de tráfico HTTPS.


## 2. Configuración del cortafuegos.
Un cortafuegos es un componente esencial que protege la granja web de accesos indebidos. Son dispositivos colocados entre subredes para realizar diferentes tareas de manejo de paquetes. Actúa como el guardián de la puerta al sistema web, permitiendo el tráfico autorizado y denegando el resto.

En general, todos los paquetes TCP/IP que entren o salgan de la granja web deben pasar por el cortafuegos, que debe examinar y bloquear aquellos que no cumplan los
criterios de seguridad establecidos. Estos criterios se configuran mediante un conjunto de reglas, usadas para bloquear puertos específicos, rangos de puertos, direcciones IP, rangos de IP, tráfico TCP o tráfico UDP.

#### Configuración del cortafuegos iptables en Linux.

iptables es una herramienta de cortafuegos, de espacio de usuario, con la que el superusuario define reglas de filtrado de paquetes, de traducción de direcciones de red, y mantiene registros de log. Para más información sobre la herramienta:
~~~

man iptables
iptables –h
~~~

Para comprobar el estado del cortafuegos, debemos ejecutar:
`iptables –L –n -v`
Para lanzar, reiniciar o parar el cortafuegos, y para salvar las reglas establecidas hasta
ese momento, ejecutaremos respectivamente:

~~~
service iptables start
service iptables restart
service iptables stop
service iptables save
~~~

Bloquear todo el tráfico ICMP (ping), para evitar ataques como el del ping de la muerte:
`iptables -A INPUT -p icmp --icmp-type echo-request -j DROP`

Abrir el puerto 22 para permitir el acceso por SSH:
`
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A OUTPUT -p udp --sport 22 -j ACCEPT
`

Abrir los puertos HTTP/HTTPS (80 y 443) para configurar un servidor web:
`
iptables -A INPUT -m state --state NEW -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -m state --state NEW -p tcp --dport 443 -j ACCEPT
`

Abrir el puerto 53 para permitir el acceso a DNS:
`
iptables -A INPUT -m state --state NEW -p udp --dport 53 -j ACCEPT
iptables -A INPUT -m state --state NEW -p tcp --dport 53 -j ACCEPT
`

Bloquear todo el tráfico de entrada desde una IP:
`iptables -I INPUT -s 150.214.13.13 -j DROP`

Bloquear todo el tráfico de salida hacia una IP:
`iptables -I OUTPUT -s 31.13.83.8 -j DROP`

Evitar el acceso a www.facebook.com especificando el nombre de dominio:
`iptables -A OUTPUT -p tcp -d www.facebook.com -j DROP`

Para volver a la configuración de la máquina inicial y permitir todo el tráfico
`
**Eliminar todas las reglas (configuración limpia)**
iptables -F
iptables -X
iptables -Z
iptables -t nat -F
**política por defecto: aceptar todo**
iptables −P INPUT ACCEPT
iptables −P OUTPUT ACCEPT
iptables −P FORWARD ACCEPT
iptables -L -n -v
`

Cuando terminemos nuestra configuración, podemos comprobar los puertos que hay abiertos:
`netstat -tulpn`

Para saber si está abierto o cerrado el puerto 80 ejecutamos:
`netstat -tulpn | grep :80`

Lo habitual es crear un script que se ejecute en el arranque del sistema. Veamos a continuación un ejemplo de script para la configuración básica de la **MV1**.
![peticiones](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%204/images/script.png)
###### Figura 4.5. Script.

Y su ejecución muestra lo siguiente:
![peticiones](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%204/images/ejecucion.png)
###### Figura 4.6. Ejecución del script.



Si queremos que se ejecute al arrancar el sistema, basta con modificar el archivo de la ruta `/etc/rc.local` añadiendo `sh /home/iptables.sh`.

