# Angular Crash Course 2021 (Task Tracker App)

This is the project from the [YouTube crash course](). It includes the Angular ui as well as JSON-server for our mock backend

## Usage

### Install dependencies

```
npm install
```

### Run Angular server (http://localhost:4200)

```
ng serve
```

### Run the JSON server (http://localhost:5000)

```
npm run server
```

### To build for production

```
ng build
```

### Dockerfile instructions
- ARG WORK_DIR=/build => définit une variable nomée WORK_DIR équivaut au répertoire /build
- FROM node:14.17 as builder => spécifie l'image de base
- ARG WORK_DIR => fait appel à la variable WORK_DIR
- ENV PATH ${WORK_DIR}/node_modules/.bin:$PATH => ajouter le chemin .bin > node_modules > ${WORK_DIR} à la variable d'environnement
- RUN mkdir ${WORK_DIR} => crée un répertoire nommé ${WORK_DIR} qui équivaut à build dans notre cas
- WORKDIR ${WORK_DIR} => spécifie le répertoire de base de notre application
- COPY package.json ${WORK_DIR} => copie le fichier package.json dans notre répertoire de base
- COPY package-lock.json ${WORK_DIR} => opie le fichier package-lock.json dans notre répertoire de base
- RUN npm install @angular/cli
- RUN npm install => exécute la commande npm install à la création de notre image
- COPY . ${WORK_DIR} => copie tous les fichiers et répertoires dans le répertoire de base de l'image
- RUN ng build --prod => exécute la commande ng build --prod pour générer les fichiers statiques
- FROM nginx:latest => spécifie l'image de serveur web pour le déploiement
- ARG WORK_DIR => fait appel à la variable WORK_DIR
- COPY ./nginx/nginx.conf /etc/nginx/conf.d/default.conf => copie le fichier de configuration nginx dans l'image
- EXPOSE 80 => exposer le port 80
- CMD nginx -g "daemon off;" => la commande à exécuter à la création du container de cette image.
- COPY --from=builder ${WORK_DIR}/dist/angular-crash /usr/share/nginx/html => copie les fichiers statiques générés dans le répertoire html du serveur nginx 
