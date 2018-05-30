# Ejercicios de teoría tema 4.

## 4.1. Buscar información sobre cuánto costaría en la actualidad un mainframe. Comparar precio y potencia entre esa máquina y una granja web de unas prestaciones similares.
No es fácil saber el precio que cuesta en la actualidad un **mainframe**. El último de **IBM** es el **z13**, el cual, se han invertido 1.000 millones de dólares, cinco años de desarrollo, 500 nuevas patentes y la colaboración de cerca de 60 clientes. Nos podemos ir haciendo una idea del elevado precio que puede costar.

Como sabemos, un **mainframe** es menos escalable que una **granja web** y un fallo en la máquina nos dejaría sin poder servir a los clientes.

Este tipo de cosas no ocurren cuando disponemos de una **granja web**, ya que si un servidor se nos cae, seguimos teniendo el resto funcionando y sirviendo contenido web a los usuarios.

En resumidas cuentas, si nos aseguran tener prestaciones similares con un mainframe y con una granja web, yo personalmente me decantaría por la **granja web**.

## 4.2. Buscar información sobre precio y características de balanceadores hardware específicos. Compara las prestaciones que ofrecen unos y otros.
Podemos comparar precios, características y prestaciones de los balanceadores hardware de la compañía **KEMP Technologies**, con los de **F5 Networks** y **Citrix** en el siguiente [enlace](https://kemptechnologies.com/compare-kemp-f5-big-ip-citrix-netscaler-hardware-load-balancers/)

## 4.3. Buscar información sobre los métodos de balanceo que implementan los dispositivos recogidos en el ejercicio `4.2`.
Depende del modelo, pueden implementar diversos métodos de balanceo, nombraré los más comunes:

+ Round Robin: Se reparte la carga por turno entre todos los servidores.
+ Weighted Round Robin: La diferencia con Round Robin es que a cada servidor del conjunto se le asigna una ponderación numérica estática.
+ Menos conexión: El método de conexión mínima tiene en cuenta la carga actual del servidor. La solicitud actual va al servidor que está atendiendo el menor número de sesiones activas en el momento actual.
+ Conexión menos ponderada: A cada servidor se le asigna un valor numérico. El equilibrador de carga usa esto al asignar solicitudes a los servidores. Si dos servidores tienen el mismo número de conexiones activas, se asignará la nueva solicitud al servidor con mayor ponderación.