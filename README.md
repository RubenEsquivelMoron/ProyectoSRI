##Trabajo SRI

###Ejercicio 1 - Instalacion de apache con dominios

- Para comenzar actualizaremos los repositorios de Linux con el comando:

```bash
sudo apt update
```
- Continuaremos con la instalacion de Apache

```bash
sudo apt install apache2
```

- Seguidamente, comprobaremos que se ha instalado correctamente, yendonos al navegador y escribiendo, localhost o 127.0.0.1

//imagen//

- Tras comprobar que el apache esta instalado correctamente, crearemos los dominios en la carpeta www, para ello deberemos hacerlo con permisos de administrador::

```bash
cd /var/www
sudo mkdir centro.intranet
sudo mkdir departamentos.centro.intranet
ls
<centro.intranet> <departamentos.centro.intranet> <html>
```
