
La notion de [[Build des images Docker|build]] multi-stage dans Docker fait référence à une technique utilisée dans les fichiers Dockerfile pour optimiser la construction d'images Docker.
Elle permet de **diviser le processus de build** en plusieurs étapes (ou "stages"), chacune ayant potentiellement une image de base différente et **pouvant copier des artefacts** d'une étape à une autre. 
Voir un exemple relatif aux [[Conteneurisation d'applications .Net Core|applications .Net Core]].

### Avantages du Build Multi-Stage

1. **Réduction de la taille de l'image**: En utilisant des images intermédiaires pour compiler ou construire l'application et ensuite copier uniquement les binaires ou artefacts nécessaires dans l'image finale, on **élimine les dépendances de compilation** et les **fichiers source non nécessaires** dans l'environnement de production. Cela **réduit** significativement la **taille de l'image** finale.
    
2. **Amélioration de la sécurité**: En éliminant les outils et dépendances non nécessaires de l'image finale, on **réduit la surface d'attaque potentielle**.
    
3. **Simplification du processus de build**: Les builds multi-stage permettent de **clairement séparer** les étapes de construction et les dépendances requises pour **construire l'application** de celles nécessaires pour **exécuter l'application**. Cela rend les Dockerfiles **plus lisibles** et plus **faciles à maintenir**.
    
4. **Optimisation du cache**: Pendant le processus de build, Docker peut mettre en cache les étapes intermédiaires.
    
5. **Flexibilité et modularité**: Avec les builds multi-stage, il est possible de construire **plusieurs artefacts ou images** à partir du **même Dockerfile**, en sélectionnant différentes étapes comme cibles.
   Cela permet de générer **différentes versions** d'une image, par exemple, pour **différents environnements** (développement, test, production..).
   Exemple : 
	```dockerfile
	# Stage 1: Compilation et build de l'application
	FROM node:14 AS builder
	WORKDIR /app
	COPY package.json package-lock.json ./
	RUN npm install
	COPY . .
	RUN npm run build
	
	# Stage 2: Image de développement avec les outils nécessaires
	FROM node:14 AS dev
	WORKDIR /app
	COPY --from=builder /app .
	# Installation d'outils de développement supplémentaires si nécessaire
	RUN npm install -g nodemon
	CMD ["nodemon", "src/index.js"]
	
	# Stage 3: Image de production allégée
	FROM nginx:alpine AS prod
	COPY --from=builder /app/build /usr/share/nginx/html
	
```
   Puis, ``docker build --target dev .`` ou ``docker build --target prod``
