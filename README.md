# DockerLAMP


Today, we will be looking at a few technologies that will make it easier to develop dynamic websites with a database back-end. Below is a list of software that we will be using as well as a brief description of it. If you would like more information on one in particular please click the name of the software.
 
[Docker](https://www.docker.com/): "platform-as-a-service products that use OS-level virtualization to deliver software in packages called containers. Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels."  [1](https://en.wikipedia.org/wiki/Docker_(software))

[MariaDB](https://mariadb.org/): open source MySQL database server

[Phpmyadmin](https://www.phpmyadmin.net/): Web based and built with php, this software allows you to administer MySQL databases visually.



```YAML
version: '3'
services: 
  apache-php:
    build:
      context: ./apache-php
    ports:
      - 80:80
    volumes:
      - ./www:/var/www
    networks:
      - localSite
    restart: unless-stopped
  # Database
  mariadb:
    image: mariadb
    volumes:
      - mariadb_data:/var/lib/mysql
    ports:
      - 3306:3306
    environment:
      MYSQL_ALLOW_EMPTY_PASSWORD: 'no'
      MYSQL_ROOT_PASSWORD: 'ChangeMe!FIXME'
      MYSQL_INITDB_SKIP_TZINFO: 1
    networks:
      - localSite
    restart: unless-stopped
  # phpmyadmin
  phpmyadmin:
    depends_on:
      - mariadb
    image: phpmyadmin/phpmyadmin
    ports:
      - 8080:80
    networks:
      - localSite
    restart: unless-stopped
    environment:
      PMA_HOST: mariadb
      MYSQL_ROOT_PASSWORD: 'ChangeMe!FIXME'
networks:
  localSite:
volumes:
  mariadb_data:
```
