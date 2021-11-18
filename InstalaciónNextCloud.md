# NextCloud

INSTALACIÓN DE NEXTCLOUD

1. Entrar pàgina de NextCloud

![](/Manuales/Fotos/Pagina.png)


2. Seleccionas la opción `Get NextCloud`.

![](/Manuales/Fotos/Opcion.png)


3. Seleccionas la opción ``Server packages``y descargas el archivo.


![](/Manuales/Fotos/Descargar1.png)


4. Creamos una carpeta llamada ``clouds``.

![](/Manuales/Fotos/carpetanueva.png)

5. Dentro de la carpeta copiamos el archivo anteriormente descargado.

![](/Manuales/Fotos/copiar.png)

6. En esta misma carpeta encendemos vagrant y la iniciamos.

![](/Manuales/Fotos/vagrantup.png)



![](/Manuales/Fotos/vagrantssh.png)



7. Siendo root instalamos ``unzip``.

~~~
apt install unzip
~~~
8. movemos el archivo ``zip`` al directo ``/var/www/html``

~~~
mv nextloud-22.2.0.zip /var/www/html
~~~

9. descomprimimos el archivo zip en este directorio

~~~
unzip nextloud-22.2.0.zip
~~~


10. Actualizamos la maquina con:

~~~
apt update
apt upgrade
~~~
11. Instalamos el servidor ``apache2``

~~~
apt install -y apache2
~~~
12. Instalamos la base de datos ``mysql-server``
~~~
apt install -y mysql-server
~~~
13. Instalamos varias librerias ``php``.
~~~
apt install php libapache2-mod-php
apt install php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
~~~
14. Creación de la base de datos.
~~~
CREATE DATABASE bbdd;
~~~
15. Creamos un usuario.
~~~
CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
~~~
16. Le damos privilegios al usuario
~~~
GRANT ALL ON bbdd.* to 'usuario'@'192.168.22.100';
~~~
17. Salimos de la base de datos
~~~
exit
~~~
18. Comprobamos la conexión a la base de datos
~~~
mysql -u usuario -p
~~~
19. Aplicamos los permisos a nuestras aplicaciones web.
~~~
cd /var/www/html
chmod -R 775 .
chown -R root:www-data .
~~~
20. Nos vamos al navegador y buscamos ``localhost:8080``.

![](/Manuales/Fotos/localhost:8080.png).

21. Para terminar la instalación debemos rellenar los campos vacios con la información correspondiente que hayamos introducido anteriormente.

![](/Manuales/Fotos/ultima.png)
