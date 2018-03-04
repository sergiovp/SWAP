# Práctica 1. Preparación de las herramientas.
## Sergio Vela Pelegrina.
Esta primera práctica, consiste en la instalación de dos máquinas virtuales con **Ubuntu Server**, para poder usarlas como máquinas servidores.
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



Y posteriormente si está en ejecución en ambas máquinas:


Para instalar curl he ejecutado
`sudo apt-get install curl`
