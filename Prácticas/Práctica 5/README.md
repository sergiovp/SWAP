# SWAP
## Práctica 5. Replicación de bases de datos MySQL.
### Sergio Vela Pelegrina.

Los objetivos concretos de esta práctica son:
+ Copiar archivos de copia de seguridad mediante ssh.
+ Clonar manualmente BD entre máquinas.
+ Configurar la estructura maestro-esclavo entre dos máquinas para realizar el clonado automático de la información.

## 1. Crear una BD e insertar datos.

Debemos crearnos una BD en MySQL e insertar algunos datos. Así tendremos datos con los cuales hacer las copias de seguridad. En todo momento usaremos la interfaz de línea de comandos del MySQL:

![BD](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%205/images/1_BD_creada.png)
###### Figura 5.1. BD creada.

Ya tenemos datos (un registro) insertados en nuestra BD llamada “datos”. Podemos haber insertado más registros. Veamos cómo entrar y hacer una consulta:

![BD](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%205/images/2_consulta_BD.png)
###### Figura 5.2. Consulta en la BD.

## 2. Replicar una BD MySQL con mysqldump.

MySQL ofrece una herramienta para clonar las BD que tenemos en nuestra maquina. Esta herramienta es **mysqldump**.
La sintaxis de uso es: `mysqldump ejemplodb -u root -p > /root/ejemplodb.sql` 
Esto puede ser suficiente, pero tenemos que tener en cuenta que los datos pueden estar actualizándose constantemente en el servidor de BD principal. En este caso, antes de hacer la copia de seguridad en el archivo .SQL debemos evitar que se acceda a la BD para cambiar nada.
Así, en el servidor de BD principal (maquina1) hacemos:

![BD](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%205/images/3_evitar_acceso_bd.png)
###### Figura 5.3. Evitar que se acceda a la BD.

Ahora ya sí podemos hacer el mysqldump para guardar los datos. En el servidor principal (maquina1) hacemos:
`mysqldump ejemplodb -u root -p > /tmp/ejemplodb.sql`
Como habíamos bloqueado las tablas, debemos desbloquearlas (quitar el “LOCK”):
~~~
mysql -u root –p
mysql> UNLOCK TABLES;
mysql> quit
~~~
Ya podemos ir a la máquina esclavo (maquina2, secundaria) para copiar el archivo .SQL con todos los datos salvados desde la máquina principal (maquina1):
`scp maquina1:/tmp/ejemplodb.sql /tmp/`
y habremos copiado desde la máquina principal (1) a la máquina secundaria (2) los datos que hay almacenados en la BD.

![BD](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%205/images/4_copia_BD.png)
###### Figura 5.4. Copia de la BD a MV2.

Debemos crear la **BD** en la **MV2** y restaurar los datos contenidos con `mysql -u root -p ejemplodb < /tmp/ejemplodb.sql`.
Y como podemos comprobar, se ha importado correctamente:

![BD](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%205/images/5_consulta_BD_2.png)
###### Figura 5.5. Consulta de la BD en la MV2.

## 3. Replicación de BD mediante una configuración maestro-esclavo.

La opción anterior funciona perfectamente, pero es algo que realiza un operador a mano. Deberemos automatizar el proceso de la siguiente forma:

En la **MV1** o **Maestro** editaremos el archivo `/etc/mysql/mysql.conf.d/mysql.cnf`.

+ Comentamos el parámetro bind-address que sirve para que escuche a un servidor: `#bind-address 127.0.0.1`
+ Le indicamos el archivo donde almacenar el log de errores: `log_error = /var/log/mysql/error.log`
+ Establecemos el identificador del servidor: `server-id = 1`
+ El registro binario contiene toda la información que está disponible en el registro de actualizaciones: `log_bin = /var/log/mysql/bin.log`
+ Guardamos el documento y reiniciamos el servicio: `/etc/init.d/mysql restart`

Si no nos ha dado ningún error la configuración del maestro, podemos pasar a hacer la configuración del mysql del **esclavo** que es igual, pero en lugar de `server-id = 1` será `server-id = 2`.

Al no tener ningún error, Podemos volver al maestro para crear un usuario y darle permisos de acceso para la replicación.

Entraremos en **mysql** y ejecutaremos:
~~~
mysql> CREATE USER esclavo IDENTIFIED BY 'esclavo';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'esclavo'@'%' IDENTIFIED BY 'esclavo';
mysql> FLUSH PRIVILEGES;
mysql> FLUSH TABLES;
mysql> FLUSH TABLES WITH READ LOCK;
~~~

Para finalizar con la configuración en el maestro, obtenemos los datos de la BD que vamos a replicar para posteriormente usarlos en la configuración del esclavo:
`mysql> SHOW MASTER STATUS;`

![BD](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%205/images/6_configuracion_maestro.png)
###### Figura 5.6. Configuración maestro.

Volvemos a la máquina esclava, entramos en mysql y le damos los datos del maestro.
~~~
mysql> CHANGE MASTER TO MASTER_HOST='172.20.10.4',
MASTER_USER='esclavo', MASTER_PASSWORD='esclavo',
MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=980,
MASTER_PORT=3306;
~~~

Y por último, arrancamos el esclavo y ya está todo listo.
`mysql> START SLAVE;`

Para comprobar que todo funciona, añadiremos datos en la **BD** de la **MV1** y veremos como se actualiza en la **BD** de la **MV2**.

![BD](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%205/images/7_insercion_MV1.png)
###### Figura 5.7. Insertamos un dato en la BD de la MV1.

![BD](https://github.com/sergiovp/SWAP/blob/master/Pr%C3%A1cticas/Pr%C3%A1ctica%205/images/8_clonado_MV2.png)
###### Figura 5.8. Comprobamos si se ha insertado en la BD de la MV2.

Como podemos ver, se ha realizado el clonado perfectamente.
