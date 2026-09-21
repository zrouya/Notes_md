
Les extensions de fichiers Docker Compose permettent de gérer plus facilement le **cycle de vie** traditionnel d'une application (**développement**, **test**, **intégration**, **production**).

## Extension par YAML

L'attribut ``extends:`` d'un [[Fichier Docker compose|fichier]] Docker Compose permet de **partager** des **configurations communes** au sein de services.

Exemple :

```yaml
services:
  web:
    extends:
      file: common-services.yml
      service: webapp
```

Ici le service "web" déployé reprendra les éléments de config (build, ports, volumes,...) du service "webapp" défini dans le fichier common-services.yml.

Il est également possible d'étendre une configuration de fichier Docker Compose au sein même du fichier : 

```yaml
services:
  web:
    extends:
      file: common-services.yml
      service: webapp
    environment:
      - DEBUG=1
    cpu_shares: 5

  important_web:
    extends: web # Ici le service important_web utilise la configuration du service 'web', en la surchargeant (voir ligne en dessous)
    cpu_shares: 10
```

## Extension par ligne de commande

Il est possible de surcharger un fichier Docker Compose par un autre directement via la commande ``docker compose up`` :
- ``docker compose up -f composefile1.yml -f composefile2.yml`` effectuera un compose up en appliquant **d'abord le fichier 1**, **puis le 2** (le composefile2 **surcharge** le 1).
 - **Par défaut**, le fichier nommé ``docker-compose.override.yml`` **surchargera** le fichier ``docker-compose.yml``, s'ils sont dans le même répertoire, et que la commande simple ``docker compose up`` y est exécutée