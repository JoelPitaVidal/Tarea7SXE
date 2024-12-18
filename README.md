# Docker Compose para Joomla, MySQL y phpMyAdmin

## Requisitos previos

- Docker y Docker Compose instalados en tu máquina.
- Guía para instalar [https://docs.docker.com/engine/install/debian/](docker:).

## Instrucciones

### Parte 1: Configuración de Joomla y MySQL

1. Creamos un archivo `docker-compose.yml` con el siguiente contenido:

 ```services:
    Joomla:
        image: joomla
        restart: always
        ports:
            - 8080:80
        environment:
            JOOMLA_DB_HOST: db
            JOOMLA_DB_USER: joomla
            JOOMLA_DB_PASSWORD: 1234
            JOOMLA_DB_NAME: joomla_db
            JOOMLA_SITE_NAME: Joomla
            JOOMLA_ADMIN_USER: Joomla
            JOOMLA_ADMIN_USERNAME: joomla
            JOOMLA_ADMIN_PASSWORD: joomla@secured
            JOOMLA_ADMIN_EMAIL: joomla@example.com
    
    db:
        image: mysql:8.0
        restart: always
        environment:
            MYSQL_DATABASE: joomla_db
            MYSQL_USER: joomla
            MYSQL_PASSWORD: 1234
            MYSQL_RANDOM_ROOT_PASSWORD: '1234'
        
    phpMyAdmin:
        image: phpmyadmin
        restart: always
        ports:
            - 8081:80
        environment:
            - PMA_ARBITRARY = 1
```

2. Ejecutamos `docker-compose up -d` para levantar los servicios.
3. Accedemos a Joomla en [http://localhost:8080](http://localhost:8080).

### Parte 2: Añadir phpMyAdmin

1. Actualizamos el archivo `docker-compose.yml` para incluir el servicio phpMyAdmin:

```yaml
services:
  joomla:
    image: joomla
    ports:
      - "8080:80"
    environment:
      JOOMLA_DB_HOST: db
      JOOMLA_DB_USER: root
      JOOMLA_DB_PASSWORD: example
      JOOMLA_DB_NAME: joomla
    volumes:
      - joomla_data:/var/www/html

  db:
    image: mysql:8.0.13
    command: --default-authentication-plugin=mysql_native_password
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: joomla
    volumes:
      - db_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8081:80"
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: example

volumes:
  joomla_data:
  db_data:
  
```

2. Ejecutamos `docker-compose up -d` para levantar los servicios.
3. Accedemos a phpMyAdmin en [http://localhost:8081](http://localhost:8081).

## Explicación de la configuración

- La configuración de Docker Compose define tres servicios: `joomla`, `db` y `phpmyadmin`.
- Los servicios `joomla` y `db` utilizan volúmenes persistentes para almacenar datos y garantizar la persistencia.
- El servicio `phpmyadmin` permite gestionar la base de datos MySQL a través de una interfaz gráfica web.
