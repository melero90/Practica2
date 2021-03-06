Práctica 2: Aislamiento de una aplicación web
=============================================

#### Descripción

El principal objetivo de esta práctica es familiarizarse con este tipo de infraestructura virtual que se usa generalmente
para dar un acceso limitado a una aplicación o un servicio tal como un servidor web o a un usuario, que pueda acceder por
ejemplo sólo para depositar ficheros.

El objetivo secundario es el que el alumno tenga instaladas las herramientas necesarias para crear *jaulas chroot* y 
tenga claro en qué casos son la mejor y más eficiente opción; también en qué casos conviene usarlas por motivos de 
seguridad. Estas herramientas se añadirán a la panoplia de herramientas de un administrador que al terminar la asignatura tendría 
que tener el alumno.

El énfasis de esta práctica es en el despliegue en una *jaula*. por lo que no será necesario hacer una aplicación 
especifica para la misma. Sin embargo, es posible que sea más fácil crear la aplicación para esta jaula usando cualquier
lenguaje de scripting (tal como Python, PHP, Perl o Ruby) que enjaular una aplicación ya creada, con todas sus 
dependencias;  posiblemente, en todo caso, merezca la pena hacerlo. Si se desea, se puede usar la misma aplicación que 
en la primera práctica.

#### Realización

Para poder instalar nuestra distro de Ubuntu es necesario instalar del paquete *debootstrap* en nuestro sistema, para 
poder instalar la versión de Ubuntu. Escribimos en el terminal:

> sudo apt-get install debootstrap

Una vez instalado, lo siguiente sera elegir una versión de Ubuntu a instalar. Podemos elegir entre la siguientes:

> http://es.wikipedia.org/wiki/Anexo:Versiones_de_Ubuntu#Ubuntu_13.04

La versión que voy a utilizar es debian de 32 bits, ya que la tenía instalada para la realización de los ejercicios.
Para ello, lo primero, será crear un directorio en /home/jaulas/debian mediante la orden y creado el directorio proce-
demos a instalar la máquina virtual haciendo uso de *debootstrap*. Con la orden *--arch=i386* especificamos que nuestra
distribución a instalar es de 32bits

> sudo mkdir -p /home/jaulas/debian

> sudo debootstrap --arch=i386 debian /home/jaulas/debian  http://archive.ubuntu.com/ubuntu

Aquí una captura en el proceso de instalación:

![captura1](https://dl.dropbox.com/s/60vl1wpdk9crsbh/captura1.png)

Entramos en la jaula con 

> sudo chroot /home/jaulas/debian

![captura2](https://dl.dropbox.com/s/12jse27ehjztx4g/captura2.png)

La máquina tal como está es usable, pero no está completa. Para solucionarlo escribimos:

> mount -t proc proc /proc

Y para solucionar los errores con el lenguaje español instalamos el siguiente paquete:

> apt-get install language-pack-es

Esta captura muestra como ya si podemos ejecutar una orden como es *top*:

![captura3](https://dl.dropbox.com/s/x3cc3214nah9dbn/captura3.png)

Ahora que nuestra máquina está preparada, lo siguiente será instalar el servidor Apache. Tecleamos desde el terminal:

> apt-get install apache2

ahora el modulo para php5:

> apt-get install php5 libapache2-mod-php5

Reiniciamos el servicio Apache:

![captura4](https://dl.dropbox.com/s/df5qjvi9fhws2t0/captura4.png)

Todo instalado ahora copiamos nuestra aplicacion en la carpeta: Metemos nuestra aplicacion en /debian/var/www

![captura5](https://dl.dropbox.com/s/c8pj4sntdlyptmg/captura5.png)

> sudo cp -r Periodico/* /home/jaulas/debian/var/www

![captura6](https://dl.dropbox.com/s/d3e36ia8ocnewmg/captura6.png)

Ahora hacemos lo siguiente, para reiniciar el servicio apache, de nuevo:

> etc/init.d/apache2 restart

![captura7](https://dl.dropbox.com/s/47qo5u8qethx9j5/captura7.png)

Abrimos nuestro navegador web y escribimos en la barra de direcciones "localhost". El Periodico está funcionando.
Aquí una captura:

![captura8](https://dl.dropbox.com/s/t8wqe0asqzyciva/captura8.png)


#### Conclusión

La creación de una *jaula* con chroot básicamente es crear una máquina virtual (árbol del sistema) en un directorio.
En esta estructura var, bin, etc,...  introduces las bibliotecas, ficheros y binarios necesarios para que un proceso 
encuentre lo necesario y se ejecute sin salir del sistema de ficheros creado, es decir que solo vea esa estructura de 
ficheros y directorios y el usuario que ejecute el proceso no pueda salir de la jaula creada (no salga facilmente).

#### Enlace al Repositorio

https://github.com/melero90/Practica2

#### Bibliografía

1. https://wiki.ubuntu.com/DebootstrapChroot
2. http://jj.github.io/IV/documentos/temas/Tecnicas_de_virtualizacion
3. http://es.wikipedia.org/wiki/Anexo:Versiones_de_Ubuntu#Ubuntu_13.04
4. http://soportetecnicocurc.blogspot.com.es/2013/03/instalar-apache-php-mysql-y-phpmyadmin.html
5. http://rogerdudler.github.io/git-guide/index.es.html

