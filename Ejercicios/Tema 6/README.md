# Ejercicios de teoría tema 6.

## 6.2. Comprobar qué puertos tienen abiertos nuestras máquinas, su estado, y qué programa o demonio lo ocupa.
Para ello, usaré el comando `netstat -tulpn`
![imagen](https://github.com/sergiovp/SWAP/blob/master/Ejercicios/Tema%206/ejercicio_6.2.png)

## 6.3. Buscar información acerca de los tipos de ataques más comunes en servidores web (p.ej. secuestros de sesión). Detallar en qué consisten, y cómo se pueden evitar.
+ **Ataque Por Injection**. Los ataques de inyección, más específicamente sqli (Structured Query Language Injection) es una técnica para modificar una cadena de consulta de base de datos mediante la inyección de código en la consulta. El SQLI explota una posible vulnerabilidad donde las consultas se pueden ejecutar con los datos validados.

La solución sería evitar que se pudieran introducir caracteres especiales (comillas) sin haberlas transformado antes (por ejemplo, una comilla doble: " debería de 
transformarse en \" que así interpretará como texto la comilla y no como el carácter que cierra o abre el una texto en la consulta, pero según el lenguaje se puede implementar de distintas formas y algunas son automáticas y más optimizadas.

+ **DDoS**. La Denegación de Servicio (DoS) ó Denegación de Servicio Distribuida (DDoS) son las formas más comunes para congelar el funcionamiento de un sitio web. Estos son los intentos de inundar un sitio con solicitudes externas, por lo que ese sitio no podría estar disponible para los usuarios reales. Los ataques de denegación de servicio por lo general se dirigen a puertos específicos, rangos de IP o redes completas, pero se pueden dirigir a cualquier dispositivo o servicio conectado.

Para evitarlos se debe conocer el tráfico (monitorizar tanto el tráfico de entrada como
el de salida para ganar visibilidad en volúmenes poco usuales o diseños que puedan identificar sitios target o revelar botnets dentro de la red).

+ **Fuerza bruta**. Estos son básicamente intenta “romper” todas las combinaciones posibles de nombre de usuario + contraseña en una página web. Los ataques de fuerza bruta buscan contraseñas débiles para ser descifradas y tener acceso de forma facil. Los atacantes cuentan con buen tiempo, así que el truco es hacer que tus contraseñas sean lo bastante seguras y así el atacante se cansaría antes de descifrar tu contraseña. Mientras que las computadoras se vuelven más y más poderosa la necesidad de contraseñas más fuertes se vuelve cada vez más importante. El caso mas reciente de esta vulnerabilidad, se ha visto en cuanto a la vulneración de cuentas de algunos famosos alojadas en iCloud.

    + Para evitarlos:
    + Deja de usar el nombre de usuario que viene por defecto “admin”.
    + Utiliza una contraseña segura.
    + Hacer regularmente copias de seguridad de archivos y base de datos.
    + Usar autenticación de dos factores.
    + Proteger con una contraseña el WP-Admin y limita los intentos de conexión.