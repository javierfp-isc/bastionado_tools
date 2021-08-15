# bastionado tools

Repositorio con herramientas para el módulo. Consta de varios escenarios de despliegue:

* **distros**: Distribuciones de herramientas. En este caso **Kali Linux**
* **testers**: Testeadores para realización de pruebas

## Requisitos

Para ejecutar los escenarios descritos a continuación debemos de disponer:

* **Host Debian Buster**
* **Git**
* **Docker engine**
* **Herramienta docker-compose**

En el siguiente enlace se ofrece información sobre cómo realizar la instalación y el despliegue de las mismas:

[Requisitos](https://github.com/javierfp-isc/sxe_requisitos/blob/master/REQUISITOS.md)

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

`wireshark &`

Para que las aplicaciones gráficas puedan ser ejecutadas desde root dentro del container hay que ejecutar en el host, con el usuario del escritorio:

`xhost si:localuser:root`

Una vez ejecutado el comando anterior ya podremos ejecutar las aplicaciones gráficas dentro del container, las cuales usarán la pantalla asociada al escritorio del host.

El comando xhost solo afecta a la sesión en la que se ejecuta, para poder hacer persistente su efecto añadimos la siguiente línea al principio del archivo **~/.bashrc** en la sesión de nuestro usuario:

`xhost si:localuser:root > /dev/null`

## Escenario testers

Dentro de ese directorio encontramos un escenario de despliegue de containers para realización de pruebas

### Creación de la imagen

Ejecutamos:

`docker-compose build`

El comando anterior crea la imagen **testers_lxde**. Esta imagen:

* Es una imagen para crear containers basados en **Debian con escritorio LXDE**
* Dispone del usuario **tester (abc123.)** para realizar pruebas desde un entorno de escritorio LXDE
* Tiene instalado un **servidor RDP ,xrdp,** para acceder al escritorio a través de herramientas de acceso remoto, como Remmina.

Será la base de partida para la creación de containers utilizados para realizar pruebas.

### Lanzamiento de container basado en la imagen

Una vez creada la imagen creamos un container:

`docker run -d --name lxde_tester --privileged --network=host --hostname=tester testers_lxde`

Con el comando anterior el container se conectará al entorno de red del host pero puede ser útil cambiar el parámetro network para que el container se conecte a la red indicada. Por ejemplo para conectar la red a la network perimetral_adm:

`docker run -d --name lxde_tester --privileged --network=perimetral_adm --hostname=tester testers_lxde`

Una vez creado el container determinamos su dirección IP, para ello accedemos al mismo:

`docker exec -it lxde_tester bash`

y ejecutamos:

`ip addr`

Otra opción es asignar la IP manualmente en el archivo **/etc/network/interfaces**

Con la IP elegida vamos a un cliente de escritorio para el protocolo RDP, como por ejemplo Remmina, y nos conectamos a través de ese protocolo con el usuario: **tester** password: **abc123.**

