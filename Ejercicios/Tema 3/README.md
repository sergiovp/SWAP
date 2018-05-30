# Ejercicios de teoría tema 3.

## 3.1. Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra.

**En Linux**, la configuración de red está en `/etc/network/interfaces`, archivo donde podemos establecer las configuraciones de red. El enrutamiento podemos visualizarlo ejecutando el comando **route**, el cual nos muestra la tabla de rutas.

A través de las opciones del comando **route** podemos realizar más funciones, como por ejemplo, añadir una ruta a una red en la tabla de enrutamiento con el comando **add** o quitarla con el comando **del**.

## 3.2. Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el filtrado y bloqueo de paquetes.

+ **En Windows**, con el propio Firewall podemos administrar funciones simples de su comportamiento.

+ En **Linux**, como vimos en la **práctica 4**, podemos utilizar **iptables**, que nos permite incluir múltiples reglas para dejar pasar o bloquear paquetes, de manera muy sencilla, lo que hace que sea una herramienta muy conocida.