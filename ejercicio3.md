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
