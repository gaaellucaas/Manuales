# Owncloud

INSTALACION DE OWNCLOUD

1. Primero creamos un contenedor

~~~
lxc launch ubuntu:20.04 elmeucontenidor
~~~

2. Predeterminadamente esta encendido, pero en el caso de estar apagado utiliza:

~~~
lxc start elmeucontenidor
~~~

3. Ejecutamos el contenedor

~~~
lxc exec elmeucontenidor bash
~~~


4. Instalamos Apache2,MYSQL y varias librerias.

~~~
 apt update
 apt upgrade
 apt install apache2
 apt install mysql-server
 apt install php libapache2-mod-php
 apt install php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-c
~~~

CONFIGURAMOS APACHE2

Activamos el modulo ``proxy_fcgi``:

~~~
a2enmod proxy_fcgi
~~~

Para activar la configuracion de `php-fpm` utilizamos este comando:

~~~
a2enconf php-fpm
~~~


Para reiniciar el servidor utilizamos

~~~
systemctl restart apache2
~~~

Para crear una base de datos y un usuario al MySQL debes de hacer lo siguiente:

Accedemos al ``MySQL`` con  el usuari ``root``

~~~
mysql -u root
~~~

Creamos la base de datos (con el nombre a tu eleccion),en mi caso ``lamevabasededades``.

~~~
CREATE DATABASE lamevabasededades;
~~~

Creamos un usuario llamado ``elmeuusuari``(a tu eleccion), le ponemos como contraseña ``elmeupassword``(a tu eleccion) y le damos privilegios sobre la base de datos anteriormente creada:

~~~
CREATE USER 'elmeuusuari'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
~~~

~~~
GRANT ALL ON lamevabasededades.* to 'elmeuusuari'@'localhost';
~~~

~~~
exit
~~~

Comprobamos que todo va perfectamente

~~~
mysql -u elmeuusuari -p
~~~

Teniendo la base de datos y el usuario puedes continuar la instalacion. Lo primero que debemos hacer es sincronizar la carpeta donde estara la carpeta al contenedor ``/var/www/html``.

En nuestra maquina host creamos una serie de claves

~~~
ssh-keygen -f ~/.ssh/elmeucontenidor -N ""
~~~

Para mostrar la clave publica pon lo siguiente:

~~~
 cat ~/.ssh/elmeucontenidor.pub
~~~


Seleccionas la clave privada y la copias a tu contenedor



Pegala en el fichero ``/root/.ssh/authorized_keys`` del contenedor el contenido de la clave publica.

~~~
vim /root/.ssh/authorized_keys
~~~

Crea una carpeta llamada ``elmeucontenidor`` al host.

~~~
mkdir ~/elmeucontenidor
~~~

Adivina la IP del contenedor utiliza:

~~~
ip a
~~~

Tambien puedes saber la IP utilizamos ``lxc list``.

Sincroniza la carpeta elmeucontenidor` del host amb la carpeta `/var/www/html` del contenidor

Substitueix la IP de la comanda per la IP que tingui el teu contenidor:

```console
sshfs root@10.161.122.237:/var/www/html ~/elmeucontenidor
```

Dins de la carpeta ``elmeucontenidor`` te habrà de aparecer  el archivo ``index.html`` que ha creado ``apache2`` en la anterior instalacion


 Copiamos la aplicacion web que queremos instalamos a nuestra carpeta ``elmeucontenidor`` del host y actualizamos los permisos.

Copia el enlace de descarga de la aplicacion y utiliza el siguiente comando:

~~~
wget https://download.owncloud.org/community/owncloud-complete-20210721.zip
~~~

Descomprimimos la aplicacion

~~~
unzip owncloud-complete-20210721.zip
~~~

La descomprension estara en la carpeta llamada ``owncloud`` y nos interesa sacar esos archivos fuera de esa carpeta:

~~~
cp -R owncloud/. .
~~~


Le damos permisos:

~~~
chown -R www-data:www-data /var/www/html
chmod -R 775 /var/www/html
~~~

Accedemos al instalador mediante el navegador.

Debes de poner la IP del contenedor en el navegador y comprovamos su funcionamiento:

~~~
http://172.16.0.79
~~~

Si todo esta correcto, y puedes acceder sin problema, te deberia salir en pantalla lo siguiente:

![](/Manuales/Fotos/ultimopaso.png)
