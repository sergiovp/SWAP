# SWAP
## Práctica 2. Clonar la información de un sitio web.
### Sergio Vela Pelegrina.
Esta segunda práctica consiste en:
+ 1.Aprender a copiar archivos mediante shh. 
+ 2.Clonar contenidos entre máquinas.
+ 3.Configurar el ssh para acceder a máquinas remotas sin contraseña. 
+ 4.Establecer tareas en cron.

## 1. Copiar archivos mediante ssh.

En esta primera parte, como aproximación a la idea de tener contenido replicado en ambas máquinas, podemos usar el siguiente comando. `tar czf - directorio | ssh equipodestino 'cat > ~/tar.tgz'`
Con el cual estaremos creando un **tar.gz** en la **MV2** a través de la **MV1** como podemos ver a continuación.

![tar](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%202/tar.png)
###### Figura 2.1. Ejecución del comando en **MV1**.

![tar2](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%202/tar2.png)
###### Figura 2.2. Creación del tar en **MV2**.

El problema que esto conlleva, es la necesidad de estar una persona ejecutando dicho comando cada vez que se quiera hacer una copia de seguridad, dicho proceso, a continuación, veremos cómo automatizarlo con la herramienta rsync, la cual he instalado con `sudo apt-get install rsync`. Aunque en mi caso, ya la tenía instalada.

## 2. Clonar contenidos entre máquinas.

Acto seguido, debemos probar el funcionamiento de rsync. Para ello, situados en la MV2, vamos a clonar la carpeta con el contenido del servidor web principal, ejecutando la siguiente orden:
`rsync -avz -e ssh vela1@172.20.10.4:/var/www/ /var/www/`

Como podemos apreciar en la imagen, en la MV2 (desde la que hemos ejecutado el comando) se nos ha clonado el contenido del servidor web principal. Estoy segudo de que se ha clonado correctamente al hacerle un cat al archivo hola.html y ver el mensaje "maquina 1 funcionando correctamente", que es el mensaje que tenia en la MV1.

Como se nos especifica en el guión, rsync nos permite determinar que directorios copiar y cuáles ignorar en el proceso de copia.
Para ello, debemos ejecutar ls siguiente orden
`rsync -avz --delete --exclude=**/stats --exclude=**/error --exclude=**/files/pictures -e ssh vela1@172.20.10.4:/var/www/ /var/www/` 



