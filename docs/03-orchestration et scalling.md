# Orchestration et Scaling - EC06

## Rôle de Docker Compose

Docker Compose joue le rôle d'orchestrateur léger dans ce projet.
Il permet de :
- Décrire et démarrer plusieurs services en une seule commande
- Définir les dépendances entre services
- Documenter la configuration du service web
- Simuler un environnement multi-conteneurs localement

## Les deux services du compose.yml

| Service | Image | Port | Rôle |
|---------|-------|------|------|
| web | catal-log:local | 8080 | Site statique Nginx |
| whoami | traefik/whoami | 8081 | Démonstration second service |

## Simulation de scaling

Commande testée :
docker compose up -d --scale web=2

Résultat obtenu :
WARNING: The "web" service is using the custom container name "catal-log-web".
Docker requires each container to have a unique name.
Remove the custom name to scale the service.

Explication : avec un container_name fixe, Docker Compose ne peut pas
créer un second conteneur avec le même nom. C'est une limite connue.

## Limites de Docker Compose

| Limitation | Impact |
|-----------|--------|
| Pas de haute disponibilité | Si l'hôte tombe, tout tombe |
| Pas de load balancing natif | Nécessite un proxy en frontal |
| Pas de self-healing | Les conteneurs ne migrent pas |
| Mono-hôte | Pas de distribution sur plusieurs machines |

## Comparaison Docker Compose vs Kubernetes

| Critère | Docker Compose | Kubernetes |
|---------|---------------|------------|
| Déploiement | Mono-hôte | Multi-noeuds |
| Scaling | Manuel, limité | Automatique |
| Haute disponibilité | Non | Oui |
| Self-healing | Non | Oui |
| Complexité | Faible | Élevée |
| Usage | Dev et test local | Production réelle |

## Lien avec la chaîne CI/CD

Docker Compose illustre la reproductibilité : le même compose.yml
décrit le service en local et en production simulée.
La promotion d'un artefact identifié par son tag SHA et son digest
garantit qu'on déploie exactement ce qui a été testé.