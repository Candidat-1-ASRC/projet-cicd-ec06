# Compte Rendu Final - EC06

## Ce que j'ai réalisé

J'ai mis en place une chaîne CI/CD complète pour le site statique
de Catal-Log. Le pipeline automatise entièrement le build, les tests
et la publication de l'image Docker via GitHub Actions et GHCR.
La promotion vers la production simulée est manuelle et nécessite
une approbation explicite.

## Les étapes réalisées

1. Création du dépôt GitHub avec les environnements recette
   et production-simulee
2. Développement du site statique servi par Nginx
3. Création du Dockerfile reproductible basé sur nginx:alpine
4. Mise en place du compose.yml avec deux services
5. Création des trois workflows GitHub Actions :
   - 01-ci.yml : build et test automatisé
   - 02-publish-ghcr.yml : publication dans GHCR
   - 03-promote.yml : promotion manuelle avec approbation
6. Tests locaux avec Docker et Docker Compose
7. Validation de la promotion sans rebuild

## Ce que j'ai appris

- La différence entre CI et CD
- L'importance d'un artefact immuable identifié par son digest
- Le rôle de GitHub Actions comme moteur d'automatisation
- Les limites de Docker Compose par rapport à Kubernetes
- La gestion des secrets dans un pipeline CI/CD
- Le principe de promotion sans rebuild

## Difficultés rencontrées

- Correction de la syntaxe YAML des workflows
- Problème de majuscules dans le nom de l'image Docker
- Configuration des credentials Git avec le compte anonyme

## Limites de mon implémentation

- La production est simulée dans GitHub, pas un vrai serveur
- Pas de scan de vulnérabilités automatisé
- Pas de monitoring en place
- Docker Compose ne remplace pas une orchestration de production
- Le scaling est limité par le container_name fixe

## Ce qu'il faudrait en production réelle

Un vrai environnement de production nécessiterait :
- Un serveur cible réel
- Un orchestrateur comme Kubernetes
- De la supervision avec Prometheus et Grafana
- Une gestion avancée des secrets avec un coffre
- Une politique de déploiement Blue/Green ou Canary
- Un scanner de vulnérabilités intégré au pipeline