# Exercice 7 - Création d'un service SWARM

Dans cet exercice, vous travaillerez avec un cluster Docker SWARM composé d'un unique noeud : votre poste de travail.

### 1. Initialisation du cluster

- Exécutez la commande **docker swarm init** pour initialiser votre poste de travail en tant que noeud Manager

### 2. Création du service

- A l'aide de la commande **[docker service create](https://docs.docker.com/reference/cli/docker/service/create/)**, créez le service suivant :

    - **Nom** - front
    - **Image** - httpd
    - **Version de l'image** - 2.4.62

- Exécutez la commande `docker service ls` et confirmez que votre service a bien été créé

- Exécutez la commande `docker service ps front` et notez que :
    - le conteneur du service listé
    - le hostname du noeud où a été créé le conteneur est affiché

- Listez les conteneurs en cours d'exécution sur votre Docker Host et notez la présence du conteneur du service que vous venez de créer

- Listez les réseaux présents sur votre Docker Host et notez la présence du réseau **ingress**

- Inspectez le conteneur de votre service et identifiez le réseau auquel il est connecté (via l'ID ou l'adresse de sous-réseau affichée)

- Inspectez le réseau **ingress** (automatiquement créé à l'initialisation de Docker SWARM) et notez que le conteneur n'y est pas connecté

- Inspectez le réseau **bridge** (automatiquement créé à l'installation de Docker) et confirmez que le conteneur y est connecté

### 3. Configuration du port-binding

- Mettez à jour le service en mappant le port **8080 du Docker Host** avec le port **80 des conteneurs du service front**

Notez que Docker SWARM recrée le conteneur du service.

- Listez les services et notez que le port-binding que vous venez d'ajouter apparaît bien

- Exécutez la commande `docker service ps front` et notez que l'ancien et le nouveau conteneur apparaissent

- Inspectez le conteneur et notez qu'il est maintenant connecté au réseau **ingress** : En effet, lorsqu'un port-binding est configuré sur un service et qu'aucun réseau n'est spécifié, les conteneurs de ce service sont automatiquement connectés au réseau ingress du cluster Docker SWARM

- Inspectez votre service et notez qu'il dispose d'une adresse IP virtuelle

- Accédez à l'application de votre service via l'URL **http://localhost:8080**

### 4. Gestion des replicas

- Mettez à jour le service en configurant un nombre de **replicas** à **10**

- Listez les services et les conteneurs, et notez que le nombre de replicas a bien été modifié

- Accédez à l'application de votre service via l'URL **http://localhost:8080** et confirmez qu'elle fonctionne toujours correctement

- Inspectez le réseau **ingress** et constatez que les 10 conteneurs de votre service y sont connectés

- Listez les conteneurs et arrêtez / supprimez en un ou plusieurs

- Exécutez plusieurs fois la commande `docker ps` et notez que les conteneurs arrêtés / supprimés sont automatiquement recréés.

### 5. Rollback

- Inspectez votre service et analysez la section **PreviousSpec** du JSON affiché

Cette section représente - comme son nom l'indique - la configuration de la révision précédente. Notez que le nombre de replica de cette précédente révision est à 1.

- Effectuez un rollback du service via la commande **docker service rollback**

- Listez les services et notez que :
    - Votre service n'a plus qu'un replica
    - L'unique replica est up depuis plusieurs minutes

Docker SWARM a supprimé les replicas jusqu'à ce que les (le) replicas restants correspondent au nombre de replica souhaité.