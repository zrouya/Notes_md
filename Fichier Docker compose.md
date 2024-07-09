

Un fichier Docker Compose est un fichier [[Fichier yaml|yaml]] qui permet de **configurer l'exécution** d'un groupe de conteneur.
Il permet également de [[Docker Compose build|configurer le build]] de l'image.

Exemple :
```yaml
# version isn't needed as of 2020 for docker compose CLI.
# All 2.x and 3.x features supported
version: '2'

services:
  wordpress: # <- Name of the service (container). Used for DNS resolution.
    image: wordpress
    build:
      context: .
      dockerfile: wordpress.dockerfile
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: example
      WORDPRESS_DB_PASSWORD: examplePW
    volumes:
      - ./wordpress-data:/var/www/html
      
  mysql:
    # we use mariadb here for arm support
    # mariadb is a fork of MySQL that's often faster and better multi-platform
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: examplerootPW
      MYSQL_DATABASE: wordpress
      MYSQL_USER: example
      MYSQL_PASSWORD: examplePW
    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:
```

**Notes :** 
- Un fichier Docker Compose fait référence à la **version de Docker Compose** (première ligne ``version:``). La valeur par défaut est **version: 1** (spécifier une version différente si nécessaire).
- Par défaut, ``docker compose up`` créera un **[[Gestion du réseau Docker|network Docker]] custom** hébergeant les container exécutés.
- Il est possible de facilement gérer le **cycle de vie** d'une application en utilisant des [[Extensions de Docker Compose files|extensions]] de fichiers Docker Compose, ou en spécifiant un **enchainement** de fichiers Docker Compose à appliquer : 
	- ``docker compose up -f composefile1.yml -f composefile2.yml`` effectuera un compose up en appliquant **d'abord le fichier 1**, **puis le 2** (le composefile2 **surcharge** le 1).
	 - **Par défaut**, le fichier nommé ``docker-compose.override.yml`` **surchargera** le fichier ``docker-compose.yml``.