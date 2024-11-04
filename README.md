# Docker Compose para Joomla, MySQL y phpMyAdmin

## Requisitos previos

- Docker y Docker Compose instalados en tu máquina.
- Guía para instalar [https://docs.docker.com/engine/install/debian/](docker:).

## Instrucciones

### Parte 1: Configuración de Joomla y MySQL

1. Creamos un archivo `docker-compose.yml` con el siguiente contenido:

    ```yaml
  services:
  joomla: # Define el servicio llamado "joomla".
    image: joomla # Usa la imagen oficial de Joomla.
    ports:
      - "8080:80" # Mapea el puerto 8080 del host al puerto 80 del contenedor.
    environment: # Establece variables de entorno necesarias para Joomla.
      JOOMLA_DB_HOST: db # Indica el host de la base de datos (el servicio "db").
      JOOMLA_DB_USER: root # Nombre de usuario de la base de datos.
      JOOMLA_DB_PASSWORD: example # Contraseña del usuario de la base de datos.
      JOOMLA_DB_NAME: joomla # Nombre de la base de datos.
    volumes:
      - joomla_data:/var/www/html # Define un volumen persistente para los datos de Joomla.

  db: # Define el servicio llamado "db".
    image: mysql:5.7 # Usa la imagen oficial de MySQL versión 5.7.
    command: --default-authentication-plugin=mysql_native_password # Configuración adicional para MySQL.
    environment: # Establece variables de entorno necesarias para MySQL.
      MYSQL_ROOT_PASSWORD: example # Contraseña del usuario root de MySQL.
      MYSQL_DATABASE: joomla # Crea una base de datos llamada "joomla".
    volumes:
      - db_data:/var/lib/mysql # Define un volumen persistente para los datos de MySQL.

volumes:
  joomla_data: # Define un volumen llamado "joomla_data" para persistencia de datos de Joomla.
  db_data: # Define un volumen llamado "db_data" para persistencia de datos de MySQL.

    ```

2. Ejecutamos `docker-compose up -d` para levantar los servicios.
3. Accedemos a Joomla en [http://localhost:8080](http://localhost:8080).

### Parte 2: Añadir phpMyAdmin

1. Actualizamos el archivo `docker-compose.yml` para incluir el servicio phpMyAdmin:

    ```yaml
services:
  joomla: # Define el servicio llamado "joomla".
    image: joomla # Usa la imagen oficial de Joomla.
    ports:
      - "8080:80" # Mapea el puerto 8080 del host al puerto 80 del contenedor.
    environment: # Establece variables de entorno necesarias para Joomla.
      JOOMLA_DB_HOST: db # Indica el host de la base de datos (el servicio "db").
      JOOMLA_DB_USER: root # Nombre de usuario de la base de datos.
      JOOMLA_DB_PASSWORD: example # Contraseña del usuario de la base de datos.
      JOOMLA_DB_NAME: joomla # Nombre de la base de datos.
    volumes:
      - joomla_data:/var/www/html # Define un volumen persistente para los datos de Joomla.

  db: # Define el servicio llamado "db".
    image: mysql:5.7 # Usa la imagen oficial de MySQL versión 5.7.
    command: --default-authentication-plugin=mysql_native_password # Configuración adicional para MySQL.
    environment: # Establece variables de entorno necesarias para MySQL.
      MYSQL_ROOT_PASSWORD: example # Contraseña del usuario root de MySQL.
      MYSQL_DATABASE: joomla # Crea una base de datos llamada "joomla".
    volumes:
      - db_data:/var/lib/mysql # Define un volumen persistente para los datos de MySQL.

  phpmyadmin: # Define el servicio llamado "phpmyadmin".
    image: phpmyadmin/phpmyadmin # Usa la imagen oficial de phpMyAdmin.
    ports:
      - "8081:80" # Mapea el puerto 8081 del host al puerto 80 del contenedor.
    environment: # Establece variables de entorno necesarias para phpMyAdmin.
      PMA_HOST: db # Indica el host de la base de datos (el servicio "db").
      MYSQL_ROOT_PASSWORD: example # Contraseña del usuario root de MySQL.

volumes:
  joomla_data: # Define un volumen llamado "joomla_data" para persistencia de datos de Joomla.
  db_data: 
    ```

2. Ejecutamos `docker-compose up -d` para levantar los servicios.
3. Accedemos a phpMyAdmin en [http://localhost:8081](http://localhost:8081).

## Explicación de la configuración

- La configuración de Docker Compose define tres servicios: `joomla`, `db` y `phpmyadmin`.
- Los servicios `joomla` y `db` utilizan volúmenes persistentes para almacenar datos y garantizar la persistencia.
- El servicio `phpmyadmin` permite gestionar la base de datos MySQL a través de una interfaz gráfica web.
