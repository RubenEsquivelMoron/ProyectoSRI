[Volver](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/7e66498129283427178ea365e2d7c50528d52644/README.md)

# Ejercicio 1 - Instalacion de apache con dominios

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

## centro.intranet

![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/d4d3dc9941596947351745e6afca8068433324a7/Capturas/ejercicio%201/12.png)

## departamentos.centro.intranet
![](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/d4d3dc9941596947351745e6afca8068433324a7/Capturas/ejercicio%201/11.png)

[Volver](https://github.com/RubenEsquivelMoron/ProyectoSRI/blob/7e66498129283427178ea365e2d7c50528d52644/README.md)
