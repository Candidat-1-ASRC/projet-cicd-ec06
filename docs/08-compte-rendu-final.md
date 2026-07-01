# 08 - Compte rendu final

## 1. Synthèse

J'ai mis en place une chaîne CI/CD complète pour le site statique de Catal-Log. Le pipeline automatise entièrement le build, les tests et la publication de l'image Docker via GitHub Actions et GHCR. La promotion vers la production simulée est manuelle et nécessite une approbation explicite, ce qui simule le processus de validation humaine en entreprise.

## 2. Fonctionnement technique

**Commit** : Le développeur pousse son code sur la branche main avec git push.

**Build** : GitHub Actions déclenche automatiquement 01-ci.yml qui construit l'image Docker depuis le Dockerfile basé sur nginx:alpine.

**Test** : Le conteneur est lancé sur le port 8080. Trois tests sont exécutés : code HTTP 200, présence de "Catal-Log" dans le HTML, présence de "EC06" dans version.json.

**Publication GHCR** : 02-publish-ghcr.yml publie l'image dans GHCR avec trois tags : sha court, latest et 1.0.0. Le digest sha256 est enregistré.

**Validation recette** : 03-promote.yml est déclenché manuellement. Le job validate-recette pull l'image depuis GHCR et la teste dans l'environnement recette.

**Promotion production-simulee** : Après approbation manuelle, le job promote-production re-tag l'image en :production sans aucun rebuild. C'est le même artefact identifié par son digest.

## 3. Conteneurisation C12

Le Dockerfile est basé sur nginx:alpine, image officielle légère et sécurisée. Il copie le site statique dans /usr/share/nginx/html/ et expose le port 80. L'image est construite automatiquement dans GitHub Actions et testée avant publication. Les preuves du build et des tests sont visibles dans les logs des workflows.

## 4. Orchestration et scaling C13

Le compose.yml décrit deux services : web (Nginx avec le site Catal-Log) et whoami (Traefik whoami pour démonstration multi-conteneurs). Les deux services partagent le réseau bridge catal-log-network. La simulation de scaling avec docker compose up --scale web=2 a révélé une limite : le container_name fixe empêche la création d'un second réplica. Cette limite est documentée et illustre pourquoi Docker Compose ne remplace pas Kubernetes en production.

## 5. Automatisation et sécurité C14

Trois workflows GitHub Actions automatisent le pipeline. Le GITHUB_TOKEN est utilisé pour l'authentification GHCR sans aucun secret stocké dans le code. Les permissions sont limitées au strict nécessaire dans chaque workflow. Le rollback est possible en re-promouvant un tag SHA antérieur depuis GHCR sans rebuild.

## 6. Production réelle

**Gestion des secrets :** En production, aucun secret ne doit apparaître dans le code. Le GITHUB_TOKEN est suffisant pour ce projet mais en production réelle, les credentials sensibles seraient stockés dans GitHub Secrets ou un coffre comme HashiCorp Vault avec rotation automatique.

**Rollback :** Grâce aux tags SHA et au digest immuable sha256, revenir à une version précédente consiste à relancer le workflow 03-promote.yml avec le tag souhaité. Aucun rebuild n'est nécessaire. Le rollback est instantané.

**Sauvegarde/restauration :** Le dépôt GitHub contient tout le nécessaire pour restaurer le projet : code, workflows, documentation. Les images GHCR sont conservées avec leurs tags versionnés. La restauration consiste à cloner le dépôt et re-promouvoir le bon tag.

**Répartition de charge :** En production, un load balancer distribuerait le trafic entre plusieurs réplicas du service web.

**Supervision :** Un outil comme Prometheus et Grafana surveillerait la disponibilité, les performances et déclencherait des alertes automatiques.

## 7. Preuves

- Dépôt GitHub : https://github.com/Candidat-1-ASRC/projet-cicd-ec06
- Workflow 01-ci.yml : https://github.com/Candidat-1-ASRC/projet-cicd-ec06/actions/workflows/01-ci.yml
- Workflow 02-publish-ghcr.yml : https://github.com/Candidat-1-ASRC/projet-cicd-ec06/actions/workflows/02-publish-ghcr.yml
- Workflow 03-promote.yml : https://github.com/Candidat-1-ASRC/projet-cicd-ec06/actions/workflows/03-promote.yml
- Image GHCR : https://github.com/Candidat-1-ASRC/projet-cicd-ec06/pkgs/container/projet-cicd-ec06
- Tag utilisé : sha-f2c3aab
- Tag production : production

## 8. Difficultés et apprentissages

**Difficultés rencontrées :**
- Syntaxe YAML des workflows corrompue par PowerShell lors de la création des fichiers
- Nom d'image Docker avec majuscules rejeté par GHCR (doit être en minuscules)
- Gestion des credentials Git avec deux comptes GitHub différents
- Container_name fixe empêchant le scaling Docker Compose

**Ce que j'ai compris techniquement :**
- Le CI/CD sépare clairement la responsabilité : le développeur pousse le code, la machine fait le reste
- Un digest sha256 est une empreinte immuable qui garantit l'intégrité de l'image
- La promotion sans rebuild est fondamentale : ce qu'on teste est exactement ce qu'on déploie
- Docker Compose est utile en local mais ne remplace pas Kubernetes pour la production
- Les permissions GitHub Actions doivent être limitées au strict nécessaire
