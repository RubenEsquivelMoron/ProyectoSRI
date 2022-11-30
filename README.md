# Trabajo SRI

## Ejercicio 1 - Instalacion de apache con dominios

- Para comenzar actualizaremos los repositorios de Linux con el comando:

```bash
sudo apt update
```
- Continuaremos con la instalacion de Apache

```bash
sudo apt install apache2
```

- Seguidamente, comprobaremos que se ha instalado correctamente, yendonos al navegador y escribiendo, localhost o 127.0.0.1

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/d4d3dc9941596947351745e6afca8068433324a7/Capturas/ejercicio%201/3.png)

- Tambien podremos comprobar que el servicio de apache esta instalado correctamente con el siguiente comando:

```bash
sudo ufw app list
```

- Tras comprobar que el apache esta instalado correctamente, crearemos los dominios en la carpeta www, para ello deberemos hacerlo con permisos de administrador:

```bash
cd /var/www
sudo mkdir centro.intranet
sudo mkdir departamentos.centro.intranet
ls
centro.intranet departamentos.centro.intranet html
```

- Tras crear las carpetas para los dominios, configuraremos el archivo hosts para indicarselo a apache

```bash
cd /etc
sudo nano hosts
```

- Escribiremos una IP por cada host

```
127.0.0.2        centro.intranet
127.0.0.3        departamentos.centro.intranet
```

- Asignaremos la propiedad del directorio con la variable de entorno $USER que hará referencia al directorio actual
- Lo haremos con los dos dominios

```bash
sudo chown -R $USER:$USER /var/www/centro.intranet
sudo chown -R $USER:$USER /var/www/departamentos.centro.intranet
```

- Crearemos un archivo .conf donde le diremos al servidor que abra centro.intranet usando /var/www/centro.intranet
- De nuevo, haremos esto con los dos dominios

```bash
sudo nano /etc/apache2/sites-available/centro.intranet.conf
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
```

- Dentro de este archivo de texto, escribiremos lo siguiente

```bash
<VirtualHost *:80>
    ServerName centro.intranet
    ServerAlias www.centro.intranet
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/centro.intranet
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

- Seguidamente habilitaremos los virtualhost que hemos creado

```bash
sudo a2ensite centro.intranet
sudo a2ensite departamentos.centro.intranet
```

- Puede venir bien deshabilitar el sitio web predeterminado que viene instalado con apache

```bash
sudo a2dissite 000-default
```

- Tambien, podremos confirmar si la sintaxis del archivo de configuracion esta correctamente escrito

```bash
sudo apache2ctl configtest
```

- Si no hay ningun error nos devolverá: Syntax OK

- Ahora, reiniciaremos el servicio de apache

```bash
sudo service apache2 restart
```
### Comprobaciones

- Para comprobar que todo funciona correctamente, crearemos  un index.html en /var/www/tu_dominio para la ver si podemos visualizar la pagina correctamente
```bash
cd /var/www/centro.intranet
sudo nano index.html
```
- Y escribiremos un 'h1' y un 'p' de prueba
```html
<h1>Funciona perfecto</h1>
<p>Esto es una prueba <b>centro.intranet</b></p>
```
- Tambien lo crearemos para el otro dominio
```bash
cd /var/www/centro.intranet
sudo nano index.html
```
```html
<h1>Esto es una prueba del segundo</h1>
<p>estamos en departamentos.centro.intranet</p>
```

- Y podemos ver que todo funciona correctamente

#### centro.intranet
![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/d4d3dc9941596947351745e6afca8068433324a7/Capturas/ejercicio%201/12.png)

#### departamentos.centro.intranet
![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/d4d3dc9941596947351745e6afca8068433324a7/Capturas/ejercicio%201/11.png)

## Ejercicio 2 - Activacion de modulos para php y mysql

- Para instalar apache 
```bash
sudo apt-get install php
```

- Seguidamente, instalaremos mysql server
```bash
sudo apt-get install mysql-server
```

- Para comprobar que mysql ha sido instalado
```bash
sudo mysql
```
![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/32ef4e2c532aa524345eb31debf709a2028215fa/Capturas/Ejercicio%202/3.png)

- Ahora, terminaremos de instalar php con el siguiente comando
```bash
sudo apt-get install php libapache2-mod-php php-mysql
```
- Comprobaremos que se ha instalado correctamente
```bash
php -v
```
![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/32ef4e2c532aa524345eb31debf709a2028215fa/Capturas/Ejercicio%202/5.png)

## Ejercicio 3 - Instalacion y configuracion de wordpress

- Para comenzar, deberemos irnos a la pagina oficial de wordpress en ubuntu y descargarlo
- [Link a Wordpress](https://wordpress.org)

- Al descargarlo deberemos enviarlo a la carpeta de departamentos.centro.intranet, situada en el /var/www

```bash
cp  -r /home/administrador/Escritorio/wordpress /var/www/centro.intranet/
```

- Al tenerlo copiado lo extraeremos 

```bash
unzip nombre_wordpress.zip
```
- Moveremos el interior de la carpeta que obtengamos a /var/www/centro.intranet/
```bash
cd /var/www/centro.intranet 
mv wordpress/* .
```

- Seguidamente, borraremos el fichero comprimido y la carpeta de instalacion

```bash
rm archivo_wordpress.zip
rmdir carpeta_wordpress
```

- Daremos los permisos del usuario www, el cual pertenece a apache

```bash
cd /var/www/departamentos.centro.intranet
chown -R www-data:www-data departamentos.centro.intranet
```
- Ahora, nos conectaremos a mysql

```
mysql -u root -p
```

- Crearemos la base de datos de Wordpress

```mysql
create database wordpress;
```

- Crearemos un usuario de la base de datos  para poder conectarnos

```mysql
grant all on wordpress.* to 'wordpressuser'@'localhost' identified by 'wordpressuser';
```

- Salimos de apache

```mysql
exit;
```

- Seguidamente, reiniciaremos apache
```bash
sudo service apache2 restart
```

### Configuracion de Wordpress en  web

- Ingresaremos en el navegador para acceder el siguiente link
- [http://centro.intranet](http://centro.intranet)


- Podremos visualizar la pagina de configuracion de wordpress que comenzará con la seleccion del idioma

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/ccad5f28af34f71133e6ce80502d99ace3df9e24/Capturas/Ejercicio%203/9.png)

- Seguidamente, escribiremos algunos credenciales

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/ccad5f28af34f71133e6ce80502d99ace3df9e24/Capturas/Ejercicio%203/10.png)

- Continuaremos con el proceso escribiendo el usuario y contraseña entre otras credenciaes finales

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/ccad5f28af34f71133e6ce80502d99ace3df9e24/Capturas/Ejercicio%203/11.png)

- Acabaremos la configuracion de Wordpress

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/ccad5f28af34f71133e6ce80502d99ace3df9e24/Capturas/Ejercicio%203/12.png)

- Por ultimo, para ingresar a wordpress, deberemos escribir la siguiente url
- [http://centro.intranet/wp-login.php](http://centro.intranet/wp-login.php)


![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/ccad5f28af34f71133e6ce80502d99ace3df9e24/Capturas/Ejercicio%203/13.png)

- Y ya estaremos dentro de Wordpress

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/ccad5f28af34f71133e6ce80502d99ace3df9e24/Capturas/Ejercicio%203/14.png)

## Ejercicio 4 - Instalacion de modulo WSGI

- Escribiremos 

```bash
sudo apt-get install libapache2-mod-wsgi
```

## Ejercicio 5 - Creación y desplegue de app python web

- Crearemos la carpeta departamentos.centro.intranet dentro del directorio html en

```bash
cd /var/www/html/
sudo mkdir departamentos.centro.intranet
```

- Al crearla deberemos crear 2 capetas mas dentro de ella, mypythonapp y public_html

```bash
cd /var/www/html/departamentos.centro.intranet
sudo mkdir mypythonapp
sudo mkdir public_html
```

-  Crearemos un archivo python controlador dentro de la carpeta mypythonapp

```bash
cd /var/www/html/departamentos.centro.intranet/mypythonapp
sudo nano controller.py
```

- Escribiremos lo siguiente

```python
# -*- conding: utf-8 -*-

def application(environ, start_response):
    # Genero la salida HTML a mostrar al usuario
    output = "<p>Bienvenido a mi <b>PythonApp</b>!!!</p>"
    # Inicio una respuesta al navegador
    start_response('200 OK', [('Content-Type', 'text/html; charset=utf-8')])
    # Retorno el contenido HTML
    return output
```
- Seguidamente, crearemos el archivo de virtual host en el directorio "sites-available"

```bash
cd /etc/apache2/sites-available
sudo nano departamentos.centro.intranet.conf
```

- Dentro escribiremos lo siguiente

```bash
<VirtualHost *:80>
    ServerName departamentos.centro.intranet

    DocumentRoot /var/www/html/departamentos.centro.intranet/public_html
    WSGIScriptAlias / /var/www/html/departamentos.centro.intranet/mypythonapp/controller.py

</VirtualHost>
```

- Guardamos el archivo y habilitaremos el virtual host con el siguiente comando

```bash
sudo a2ensite departamentos.centro.intranet.conf
Site departamentos.centro.intranet already enabled
```

- Reiniciamos el servicio de apache

```bash
sudo service apache2 restart
```

- Y probaremos a entrar a la web con la url

```
http://departamentos.centro.intranet
```

- Resultado

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/0898e23f12ca632f27cd34060cf776bba073c310/Capturas/Ejercicio%205/4.png)

## Ejercicio 6 - Protección de aplicacion por autenticación

- Comenzaremos actualizando el servidor e instalaremos las utilidades de apache

```bash
sudo apt-get update
sudo apt-get install apache2-utils
```

- Crearemos el usuario con el comando

```bash
sudo htpasswd -c /etc/apache2/.htpasswd Ruben
New password:
Re-type new password:
Adding password for user Ruben
``` 

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/bc975ecf6434d9ac4c889ec3f51283f72c6afb6c/Capturas/Ejercicio%206/1.png)

- Ahora, nos iremos al archivo de configuracion de departamentos.centro.intranet

```bash
sudo nano /etc/apache2/sites-enabled/departamentos.centro.intranet.conf
```

- Y agregaremos los siguiente a lo que ya esta escrito

```bash
<Directory "/var/www/html/departamentos.centro.intranet">
      AuthType Basic
      AuthName "Restricted Content"
      AuthUserFile /etc/apache2/.htpasswd
      Require valid-user
  </Directory>
```
- Quedará de la siguiente manera

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/bc975ecf6434d9ac4c889ec3f51283f72c6afb6c/Capturas/Ejercicio%206/3.png)

- Verificaremos la configuracion

```bash
sudo apache2ctl configtest
```

- Por ultimo, reiniciaremos apache

```bash
sudo systemctl restart apache2
```

### Comprobaciones

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/bc975ecf6434d9ac4c889ec3f51283f72c6afb6c/Capturas/Ejercicio%206/5.png)

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/bc975ecf6434d9ac4c889ec3f51283f72c6afb6c/Capturas/Ejercicio%206/6.png)

## Ejercicio 7 - Instala y configura awstat.

- Comenzaremos con la instalacion del servicio de AWstats

```bash
sudo apt-get install awstats
```

- Ahora, habilitaremos el modulo cgi

```bash
sudo service apache2 restart
sudo a2enmod cgi
```

- Seguidamente, configuraremos el sitio web

```bash
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.departamentos.centro.intranet.conf
sudo nano /etc/awstats/awstats.departamentos.centro.intranet.conf
```

- Editaremos las siguientes zonas

```
LogFile=”/var/log/apache2/access.log”
SiteDomain=”departamentos.centro.intranet” 
HostAliases=”departamentos.centro.intranet localhost 127.0.0.1” 
AllowToUpdateStatsFromBrowser=1
```
![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/6bd0c6a32cd3d8751ff213d5fb63fba1f29135ae/Capturas/Ejercicio%207/Comandos/9.png)
![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/6bd0c6a32cd3d8751ff213d5fb63fba1f29135ae/Capturas/Ejercicio%207/Comandos/10.png)
![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/6bd0c6a32cd3d8751ff213d5fb63fba1f29135ae/Capturas/Ejercicio%207/Comandos/11.png)
![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/6bd0c6a32cd3d8751ff213d5fb63fba1f29135ae/Capturas/Ejercicio%207/Comandos/12.png)

- construiremos las estadisticas iniciales que se generan a partir de los registros del servidor

```bash
sudo /usr/lib/cgi-bin/awstats.pl -config=departamentos.centro.intranet -update
```

- Por ultimo, configuraremos apache para el AWStats

```
sudo cp -r /usr/lib/cgi-bin /var/www/html/departamentos.centro.intranet

sudo chown -R www-dat www-data /var/www/html/departamentos.centro.intranet/cgi-bin/
```

- Por ultimo, haremos un restart en el apache

```bash
sudo service apache2 restart
```

- Ahora, para comprobar que se ha implementado el AWstat

```bash
http://192.168.73.45/cgi-bin/awstats.pl?config=departamentos.centro.intranet
```

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/a72b7dab2656925709630eb3dad61fec3787a108/Capturas/Ejercicio%207/2.png)

## Ejercicio 8 - Instalacion de servidor Nginx

- Comenzaremos actualizando los repositorios de linux e instalando Nginx

```bash
sudo apt update
sudo apt install nginx
```

- Ahora, ajustaremos el software de firewall para permitir el acceso al servicio.

```bash
sudo ufw app list
```

- Devuelve

```
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

- Seguidamente, habilitaremos el servicio de Nginx HTTP

```bash
sudo ufw allow 'Nginx HTTP'
```

- Y ya tendremos el servidor activo, pero para poder visualizarlo correctamente, deberemos cambiar el puerto del servidor

```bash
sudo nano /etc/nginx/sites-available/default
```

- Deberemos cambiar el puerto en la siguiente linea mostrada en pantalla

```bash
server {
        listen 8050 default_server;
        listen [::]:80 default_server;}
```

//Imagen//

- Guardaremos el archivo y reiniciaremos el servicio de nginx

```bash
sudo service nginx restart
```

- Y ya podremos visualizar la pagina web default de nginx

//Imagen//



