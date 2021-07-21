# bastionado tools
Repositorio con herramientas para el módulo

## Kali

### Creación de la imagen

Dentro del directorio tools ejecutamos:

`docker-compose build`

### Lanzamiento del container

`docker run -d --name kali --network host --privileged=true tools_kali`

El container generado, de nombre **kali**, está conectado a la red host, por tanto tiene acceso a todos los bridges del host.

### Acceso al container desde línea de comandos

`docker exec -it kali bash`

### Comentarios sobre la imagen

La imagen contiene los paquetes kali:

* **kali-tools-top10**
* **kali-tools-sniffing-spoofing**
* **kali-tools-passwords**
* **kali-tools-vulnerability**
* **kali-desktop-xfce**: Entorno de escritorio xcfe para Kali

Accederemos al container a través de un cliente RDP, como Remmina, con la cualquier IP correspondiente al container. Para conocer la IP del container podemos ejecutar dentro de él:

`ip addr`

Por defecto se configura la **distribución del teclado en español** al iniciar sesión. Hay que salir siempre del container cerrando sesión (log out) o parando el container para evitar que esta configuración se pierda
