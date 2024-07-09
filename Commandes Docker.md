
Les commandes Docker sont réparties en 2 sortes : 
- Les commandes "directes" : ``docker <command> ``  
	 ex : ``docker run --detach --publish 80:80 nginx
- Les commandes "de management", organisées en sous commandes : ``docker <command> <sub-command>
	ex : ``docker container rm myContainerName

Certaines commandes sont exécutables des 2 manières, pour assurer la rétro compatibilité des différentes versions de Docker.

Doc Docker sur les commandes de base : 
https://docs.docker.com/engine/reference/commandline/docker/