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

### centro.intranet

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/d4d3dc9941596947351745e6afca8068433324a7/Capturas/ejercicio%201/12.png)

### departamentos.centro.intranet
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

# Ejercicio 3 - Instalacion y configuracion de wordpress

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

