# Práctica 1. Preparación de las herramientas.
## Sergio Vela Pelegrina.
Esta primera práctica, consiste en la instalación de dos máquinas virtuales con **Ubuntu Server**, asegurando la conectividad entre ellas.
En mi caso, he optado por utilizar el software de virtualización **VirtualBox**.

En un principio, mi intención era configurar las IP como estáticas y tras una serie de problemas, decidí configurar la Red de las máquinas como **adaptador puente**.

De tal forma que al trabajar en casa tendré una IP y al trabajar en clase tendré otra.
He de destacar también que fuera de casa deberé darle internet al PC con el móvil, ya que al estar conectado a *eduroam*, las máquinas dejan de funcionar.

Tras la instalación de **Ubuntu Server** en ambas máquinas y siguiendo el guión de prácticas, anotaré las IP de las máquinas, que son las siguientes:

| NOMBRE |   IP CASA   | IP FUERA DE CASA |
|:------:|:-----------:|:----------------:|
|ubuntu1 |192.168.1.107|172.20.10.4       |
|ubuntu2 |192.168.1.111|172.20.10.5       |

A continuación, he comprobado la versión del servidor:

![version_servidor](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%201/version_servidor.png)
###### Figura 1.1. Versión del servidor.

Y posteriormente si está en ejecución en ambas máquinas:
![ejecucion_apache_m1](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%201/apache_ejecicion_m1.png)
###### Figura 1.2. Funcionamiento de apache en MV1.

![ejecucion_apache_m2](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%201/apache_ejecucion_m2.png)
###### Figura 1.3. Funcionamiento de apache en MV2.

Para instalar curl he ejecutado
`sudo apt-get install curl`

Para comprobar que apache está funcionando, he creado un archivo en ambas máquinas, `hola.html` en el directorio `/var/www/html/`.
Ahora, debemos acceder usando **curl** de una máquina a la otra y viceversa:

![curl_m1](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%201/curl_m1.png)
###### Figura 1.4. Acceso mediante curl de la **MV1** a la **MV2**.

![curl_m2](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%201/curl_m2.png)
###### Figura 1.5. Acceso mediante curl de la **MV2** a la **MV1**.

Para finalizar, también podemos probar a acceder de una máquina a otra por **ssh**:
![ssh_m1](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%201/ssh_m1.png)
###### Figura 1.6. Acceso por **ssh** desde la **MV1** a la **MV2**.

![ssh_m2](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%201/ssh_m2.png)
###### Figura 1.7. Acceso por **ssh** desde la **MV2** a la **MV1**.


O incluso utilizar curl para descargar un archivo:
![curl_descarga](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%201/curl_imagen.png)
###### Figura 1.8. Descarga de archivo mediante **curl**.

