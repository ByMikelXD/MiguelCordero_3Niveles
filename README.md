# lamp_tres_niveles
### Balanceador de carga con infraestructura LAMP en tres niveles.

# Índice
1. [Introducción](#introducción)
2. [Infraestructura](#infraestructura)
   * [Infraestructura de red](#infraestructura-de-red)
3. [Balanceador](#configuración-del-balanceador)
4. [Resultado](#resultado)
5. [Apache-Sever1](#configuración-del-servidor-de-apache-1)
6. [MYSQL](#configuración-del-servidor-de-base-de-datos-mysql)

# Introducción

En este proyecto se realiza la infraestructura de una pila LAMP en tres niveles sobre máquinas virtuales en AWS.

En un primer nivel, tendremos el balanceador de carga. En el segundo nivel, tendremos dos servidores web en Backend. En el tercer nivel, tendremos el servidor de bases de datos.

# Infraestructura

En este proyecto se explicarán detalladamente los pasos a seguir para la creación de la infraestructura necesaria.

## Infraestructura de red

#### VPC

En primer lugar, creamos una VPC. Vamos al menú de servicios de AWS, elegimos VPC y pulsamos sobre el botón de Crear VPC.

En este menú, tenemos dos opciones: Solo la VPC y la opción VPC y más. La diferencia entre ellas es que con la primera solo creamos la VPC, y con la segunda podemos crear las subredes, las tablas de enrutamiento y las puertas de enlace desde el mismo menú.

Primero, creamos nuestras VPC donde vamos a alojar nuestras dos subredes: la subred 1 donde estará el balanceador junto a nuestros servidores apache y en la segunda subred donde se almacenará el servidor MySQL.

![Imagen 1](Fotos/1.png)
![Imagen 2](Fotos/2.png)
![Imagen 3](Fotos/3.png)
![Imagen 4](Fotos/4.png)


Creamos las instancias. Las instancias que he usado son Debian.

![Imagen 5](Fotos/5.png)
![Imagen 6](Fotos/6.png)
![Imagen 7](Fotos/7.png)

A continuación, vamos a asociar una IP estática al balanceador para poder conectarnos y tener acceso a Internet.

![Imagen 8](Fotos/8.png)

Conectamos la gateway antes creada a Internet.

![Imagen 9](Fotos/9.png)

Luego nos vamos a asociar una dirección IP elástica para tener acceso a Internet desde el balanceador.

![Imagen 10](Fotos/10.png)

Se me olvidaba decir que tenemos que poner una puerta de enlace a Internet para poder tener Internet en las otras máquinas mientras las configuramos y descargamos los archivos necesarios.

# Configuración del balanceador

Copiamos el contenido del archivo default por si liamos y le ponemos un nombre para saber cuál es.

![Imagen 13](Fotos/11.png)

A continuación, dentro del balanceador copiamos lo siguiente.

![Imagen 14](Fotos/12.png)

Luego de copiar lo anterior, habilitamos el sitio web con `a2ensite`.

Nos vamos a nuestro explorador y ponemos la IP del balanceador.

# Resultado
![Imagen 17](Fotos/13.png)

Pero como queremos que nos muestre la página con un certificado, vamos a instalar Certbot y configurar un nombre de dominio.



# Configuración del servidor de Apache 1

sudo apt update
sudo apt install -y apache2
sudo apt install php libapache2-mod-php php-mysql


Volvemos a copiar el archivo por si la volvemos a liar.

![Imagen 11](Fotos/11.png)

En `DocumentRoot`, la ruta donde tiene que buscar el `index.php`.

![Imagen 12](Fotos/12.png)

Activamos el sitio que acabamos de configurar.

![Imagen 13](Fotos/13.png)
![Imagen 14](Fotos/14.png)


Copiamos el repositorio git donde está la aplicación que queremos implementar en la página web.

![Imagen 24](Fotos/24.png)
![Imagen 25](Fotos/25.png)

Luego vamos al archivo `config` en el Apache 1.

![Imagen 26](Fotos/26.png)

Le tenemos que dar permisos a la carpeta Apache1.

![Imagen 27](Fotos/27.png)

sudo systemctl restart apache2


Instalamos `mariadb-client` para conectarnos al servidor de base de datos.

![Imagen 28](Fotos/28.png)

Pasamos la base de datos al servidor MySQL.

![Imagen 29](Fotos/29.png)

# Configuración del servidor de base de datos (MySQL)

Lo primero es instalar `mariadb-server`.

![Imagen 30](Fotos/30.png)

Nos vamos al archivo `50-server.conf` y donde está el `bind-address` ponemos la IP del servidor MySQL.

![Imagen 31](Fotos/31.png)

Entramos a la base de datos y metemos un usuario para la base de datos que hemos pasado antes.

![Imagen 32](Fotos/32.png)
![Imagen 33](Fotos/33.png)
![Imagen 34](Fotos/34.png)

Si todo sale bien, cuando ponemos nuestro nombre de DNS, saldrá la página donde podemos ingresar, eliminar o editar los usuarios de la base de datos.

![Imagen 34](Fotos/35.png)
