# Práctica 4: Asegurar la granja web
    Autores: David Joaquín González-Venegas Guerra-Librero y Marina Hurtado Rosales
    Correos: davidvenegasfb@correo.ugr.es; marinahurtado@correo.ugr.es
El objetivo de esta práctica es configurar todos los aspectos relativos a la seguridad de
la granja web ya creada.
Hay que llevar a cabo las siguientes tareas:
- Crear e instalar en la máquina 1 un certificado SSL autofirmado para configurar
el acceso HTTPS a los servidores. Una vez configurada la máquina 1, se debe
copiar al resto de máquinas servidoras y al balanceador de carga. Se debe
configurar nginx adecuadamente para aceptar y balancear correctamente tanto
el tráfico HTTP como el HTTPS.
- Configurar las reglas del cortafuegos con IPTABLES en uno de los servidores
web finales para asegurarlo, permitiendo el acceso por los puertos de HTTP y
HTTPS a dicho servidor. Como se indica, esta configuración se hará en una de
las máquinas servidoras finales (p.ej. en la máquina 1), y se debe poner en un
script con las reglas del cortafuegos que se ejecute en el arranque del sistema.

-------------------------------------------------------------------------------------

## Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS

Vamos a generar un certificado SSL autofirmado en Ubuntu Server:

> Activamos el módulo SSL de Apache:

![img](https://github.com/davidvenegasfb/SWAP/blob/master/practica4/imagenes/1.png)

> Pasamos a generar los certificados:

![img](https://github.com/davidvenegasfb/SWAP/blob/master/practica4/imagenes/2.png)

> Editamos el archivo de configuración del sitio default-ssl, y agregamos estas lineas debajo de donde pone SSLEngine on:
SSLCertificateFile /etc/apache2/ssl/apache.crt
SSLCertificateKeyFile /etc/apache2/ssl/apache.key

![img](https://github.com/davidvenegasfb/SWAP/blob/master/practica4/imagenes/3.png)

Activamos el sitio default-ssl y reiniciamos apache:

![img](https://github.com/davidvenegasfb/SWAP/blob/master/practica4/imagenes/4.png)

Por último, y como queremos que la granja nos permita usar el HTTPS, debemos
configurar el balanceador para que también acepte este tráfico (puerto 443). Para
hacer esto, copiaremos la pareja de archivos (el .crt y el .key) a todas las máquinas de la granja web. Vamos a copiar los certifdicados en los archivos apache.crt y apache.key que generamos en el primer servidor en el paso anterior vamos a
copiarlos al otro servidor y al balanceador.

![img](https://github.com/davidvenegasfb/SWAP/blob/master/practica4/imagenes/5.png)

En el segundo servidor debemos activar el sitio default-ssl y reiniciar apache (como
hicimos en el primer servidor). 

![img](https://github.com/davidvenegasfb/SWAP/blob/master/practica4/imagenes/6.png)

En el balanceador pondremos la ruta a la carpeta donde hayamos copiado el apache.crt y el apache.key. Después, en el balanceador nginx debemos añadir lo siguiente al archivo /etc/nginx/conf.d/default.conf y reiniciarlo:

![img](https://github.com/davidvenegasfb/SWAP/blob/master/practica4/imagenes/7.png)

## 4.2 Configuración del cortafuegos

Creamos un script con las órdenes para una configuración básica de un servidor web y lo ejecutamos:

![img](https://github.com/davidvenegasfb/SWAP/blob/master/practica4/imagenes/8.png)
