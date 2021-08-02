#-----------------------------------------------------------------------------------
# On donne le nom "wordpress" à ce container,
# à partir de l'image "wordpress",
# on le lie à notre bdd nommée "db",
# sur le port 8081 de notre machine, qui pointe sur le port 80 de notre "container",
# puis pour finir on copie le "volume".
#-----------------------------------------------------------------------------------
# On donne le nom "wpmariadb" à ce container,
# à partir de la dernière image "mariadb",
# on init une variable "MYSQL_ROOT_PASSWORD = root",
# sur le port 3306 de notre machine, qui pointe sur le port 3306 de notre "container".
#-----------------------------------------------------------------------------------
# On donne le nom "wpphpmyadmin" à ce "container",
# à partir de l'image "phpmyadmin/phpmyadmin",
# on le lie à notre bdd nommée "db",
# on init 2 variables "MYSQL_USERNAME= root" et "MYSQL_ROOT_PASSWORD=root",
# sur le port 8080 de notre machine, qui pointe sur le port 80 de notre "container",
# on crée un volume "/sessions".
#-----------------------------------------------------------------------------------
# Ressources :
# https://docs.docker.com/compose/compose-file/#image
# https://docs.docker.com/compose/compose-file/#restart
# https://docs.docker.com/compose/compose-file/#volumes
# https://docs.docker.com/compose/compose-file/#environment
#-----------------------------------------------------------------------------------

# Version a utiliser pour le format du fichier docker-compose
version: "3.9"

services :

  # CONTAINER - MYSQL : https://hub.docker.com/_/mysql
  db:
    image: mariadb:latest
    container_name: wp_mariadb
    restart: always
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - wp

  # CONTAINER - WORDPRESS : https://hub.docker.com/_/wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    container_name: wp_wordpress
    restart: always
    volumes: ["./:/var/www/html"]
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    ports:
      - "80:80"
    networks:
      - wp
    links:
      - "maildev:maildev"

  # CONTAINER - PHPMYADMIN : https://hub.docker.com/r/phpmyadmin/phpmyadmin
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin:latest
    container_name: wp_phpmyadmin
    restart: always
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: password
    ports:
      - "8080:80"
    networks:
      - wp

  maildev:
    image: maildev/maildev
    container_name: wp_maildev
    command: bin/maildev --web 80 --smtp 25 #--hide-extensions STARTTLS
    ports:
       - "1080:80"  # Interface web
       - "1025:25"  # SMTP
    restart: always
    networks:
      - wp

networks:
  wp:

volumes:
  db_data:
