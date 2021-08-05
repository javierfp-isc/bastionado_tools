# bastionado tools

Repositorio con herramientas para el módulo. Consta de varios escenarios de despliegue:

* **distros**: Distribuciones de herramientas. En este caso **Kali Linux**

## Escenario distros

Dentro de ese directorio encontramos un escenario de despliegue de la herramienta Kali Linux

### Creación de la imagen y lanzamiento del container

Ejecutamos:

`docker-compose up -d`

El comando anterior:

* Crea la imagen **distros_kali**
* Levanta el service docker-compose **kali** que corresponde con el docker container de nombre **distros_kali_1**

El service definido en **docker-compose.yml** posee las siguientes características:

* **Está conectado a todas las redes del host**, por tanto será muy útil para auditar y trabajar en cualquier red accesible desde el host, en particular todas las networks creadas en los distintos escenarios de prácticas.
* Permite **ejecutar aplicaciones gráficas que se visualizarán en el motor de ventanas del host**

### Comentarios sobre la imagen

La imagen contiene los **metapaquetes** kali:

* **kali-tools-top10**
* **kali-tools-sniffing-spoofing**
* **kali-tools-passwords**
* **kali-tools-vulnerability**

### Acceso al container desde línea de comandos

De cualquiera de los siguientes modos:

* Mediante docker-compose, desde el directorio del escenario (distros):

`docker-compose exec kali bash`

* Mediante docker CLI con el comando:

`docker exec -it distros_kali_1 bash`

Dentro del mismo podemos utilizar todas las herramientas disponibles en los paquetes de la lista anterior, si necesitamos instalar algún metapaquete adicional lo instalamos con **apt install**. La lista completa de metapaquetes la obtendremos de:

[Metapaquetes Kali](https://www.kali.org/docs/general-use/metapackages/)

### Lanzamiento de aplicaciones gráficas

Aunque el container no dispone de entorno de escritorio es posible lanzar aplicaciones gráficas para que éstas se rendericen en el host. Simplemente lanzando la aplicación en segundo plano:

`wirehsark &`

Para que las aplicaciones gráficas puedan ser ejecutadas desde root dentro del container hay que ejecutar en el host, con el usuario del escritorio:

`xhost si:localuser:root`

Una vez ejecutado el comando anterior ya podremos ejecutar las aplicaciones gráficas dentro del container, las cuales usarán la pantalla asociada al escritorio del host.

