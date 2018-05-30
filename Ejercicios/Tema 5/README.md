# Ejercicios de teoría tema 5.

## 5.1. Buscar información sobre cómo calcular el número de conexiones por segundo.
Si estamos usando **apache** como servidor, podemos utilizar el comando `apache2ctl status | grep request`.

Podemos observar el tráfico mediante **netstat** con el comando `netstat -lpan | grep :80 | wc -l`.

Aunque si queremos obtener más información que con el comando anterior, deberemos usar **iptstate** con el siguiente comando `iptstate -1 -d IP -D 80`.

## 5.3. Buscar información sobre características, disponibilidad para diversos SO, etc de herramientas para monitorizar las prestaciones de un servidor.
+ **top**: irve para supervisar el rendimiento, muestra todo el funcionamiento y los procesos en tiempo real, el uso de CPU, de memoria, la memoria de intercambio, la caché, tamaño de búfer, PID de proceso, usuario, carga de memoria...
+ **vmstat**: Proporciona datos sobre la memoria virtual, hilos kernerl, discos, da información sobre procesos de sistema, memoria, bloques de E / S, interrupciones y actividad de la CPU.
+ **netstat**: Es una herramienta de línea de comandos que muestra un listado de las conexiones activas, tanto entrantes como salientes y las estadísticas de la interfaz.
