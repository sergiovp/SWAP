# SWAP
## Práctica 2. Clonar la información de un sitio web.
### Sergio Vela Pelegrina.
Esta segunda práctica consiste en:
+ Aprender a copiar archivos mediante shh. 
+ Clonar contenidos entre máquinas.
+ Configurar el ssh para acceder a máquinas remotas sin contraseña. 
+ Establecer tareas en cron.

## 1. Copiar archivos mediante ssh.

En esta primera parte, como aproximación a la idea de tener contenido replicado en ambas máquinas, podemos usar el siguiente comando. `tar czf - directorio | ssh equipodestino 'cat > ~/tar.tgz'`
Con el cual estaremos creando un **tar.gz** en la **MV2** a través de la **MV1** como podemos ver a continuación.

![tar](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%202/tar.png)
###### Figura 2.1. Ejecución del comando en **MV1**.

![tar2](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%202/tar2.png)
###### Figura 2.2. Creación del tar en **MV2**.

El problema que esto conlleva, es la necesidad de estar una persona ejecutando dicho comando cada vez que se quiera hacer una copia de seguridad, dicho proceso, a continuación, veremos cómo automatizarlo con la herramienta rsync, la cual he instalado con `sudo apt-get install rsync`. Aunque en mi caso, ya la tenía instalada.

## 2. Clonar contenidos entre máquinas.

Acto seguido, debemos probar el funcionamiento de rsync. Para ello, situados en la **MV2**, vamos a clonar la carpeta con el contenido del servidor web principal de la **MV1**, ejecutando la siguiente orden:
`rsync -avz -e ssh vela1@172.20.10.4:/var/www/ /var/www/`

![rsync](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%202/rsync_clonar.png)
###### Figura 2.3. Clonado de directorio con rsync.

Como podemos apreciar en la imagen, en la **MV2** (desde la que hemos ejecutado el comando) se nos ha clonado el directorio. Estoy segudo de que se ha clonado correctamente al hacerle un cat al archivo hola.html y ver el mensaje "maquina 1 funcionando correctamente", que es el mensaje que tenia en la **MV1**.

Como se nos especifica en el guión, rsync nos permite determinar que directorios copiar y cuáles ignorar en el proceso de copia.
Para ello, debemos ejecutar la siguiente orden.
`rsync -avz --delete --exclude=**/stats --exclude=**/error --exclude=**/files/pictures -e ssh vela1@172.20.10.4:/var/www/ /var/www/`

![rsync](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%202/rsync_clonar_excluyendo.png)
###### Figura 2.4. Clonado de directorio con rsync excluyendo.

Donde:
+ --delete indica que aquellos ficheros que se hayan eliminado en la máquina origen, también se borren en la máquina destino.
+ --exclude indica que ciertos directorios o ficheros no deben copiarse.

## 3. Acceso sin contraseña para ssh.

Como hemos comentado previamente, la intención de la práctica es poder mantener el contenido de varias máquinas actualizado e idéntico entre ellas. 
Podríamos hacer uso de algún script que automatizara el proceso, pero no tendría mucho sentido querer automatizar el proceso y a la vez tener que estar pendientes de cuando se nos pida la contraseña. Por ello, configuraremos **ssh** para acceder sin que se solicite contraseña.

Para ello, hay que seguir dos pasos:

+ En la **MV2** debemos ejecutar el siguiente comando `ssh-keygen -b 4096 -t rsa` con el que podremos generar la clave.

+ Ejecutar, nuevamente en la **MV2** el comando `ssh-copy-id vela1@172.20.10.4` para hacer la copia de la clave.

A continuación, podemos acceder a la **MV1** sin contraseña.

![ssh](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%202/acceso_ssh_sin_contrase%C3%B1a.png)
###### Figura 2.5. Acceso mediante ssh sin contraseña.

## 4. Programar tareas con crontab.

Por último en esta prática, crearemos una tarea con **cron**, el cual ejecuta procesos en el instante indicado en el fichero *crontab*.

Haremos que la tarea en **cron** se ejecute en cada momento, para así tener actualizado el contenido del directorio `/var/www` en todo momento entre las dos máquinas.

Para ello, añadiremos la siguiente tarea en el fichero `/etc/crontab`

`* * * * * vela2 rsync -avz -e ssh vela1@172.20.10.4:/var/www/ /var/www/`

Con la que se creará una copia de seguridad automática del directorio en la **MV2**.




