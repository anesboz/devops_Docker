## TP 1 - Docker session

### Why do we need a volume to be attached to our postgres container?

1. Pour assurer la persistance des données.
2. Pour faciliter la sauvegarde et la restauration des données.
3. Pour partager les données entre plusieurs conteneurs ou services.
4. Pour faciliter la maintenance et la mise à jour du conteneur sans perdre les données.

### 1-2 Why do we need a multistage build? And explain each step of this dockerfile.

1. L'utilisation d'une construction en plusieurs étapes permet de réduire la taille de l'image Docker finale. Cela permet d'économiser de l'espace de stockage et d'accélérer le déploiement des conteneurs.

Explication des étapes du Dockerfile :

-- etape 1 Build

- Copie du répertoire src contenant les sources de l'application dans le conteneur ainsi que la variable d'environnement MYAPP_HOME.
- Exécution de la commande mvn package -DskipTests pour compiler l'application.

-- etape 2 Run

- Définition de la variable d'environnement MYAPP_HOME pour indiquer le répertoire de l'application.
- Copie des fichiers JAR créés à partir de l'étape précédente dans le répertoire de l'application dans le conteneur.

### Why is docker-compose so important?

1. Car il simplifie l'orchestration des conteneurs Docker.
2. il gère les dépendances entre les services et les conteneurs.
3. il centralise la configuration dans un seul fichier YAML.

---

## Commandes utilisées

# Lancer one shot

docker-compose up --build

# Publish

docker tag tp1-httpd anesboz/tp1-httpd:1.0
docker push anesboz/tp1-httpd:1.0

# Lacer en wrac

## db

docker build -t db-image .

<!-- docker run -d --name mydb-container -p 5432:5432 -v pg_data:/var/lib/postgresql/data --network=app-network db-image -->

docker run -d --name mydb-container -v pg_data:/var/lib/postgresql/data --network=app-network db-image

### adminer

docker run -d --name adminer -p 8080:8080 --link mydb-container:mydb adminer

## api

## basic_api

docker build -t basic_api-image .
docker run -d --name basic_api-container -p 80:80 basic_api-image

## simple_api

docker build -t simple_api-image .
docker run -d --name simple_api-container -p 80:8080 simple_api-image

## simple_api_student

docker build -t simple_api_student-image .
docker run -d --name simple_api_student-container -p 80:8080 --network=app-network simple_api_student-image
