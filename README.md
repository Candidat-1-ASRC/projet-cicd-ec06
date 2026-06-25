# Projet CI/CD – EC06 ASRC

Pipeline CI/CD complet pour le site statique Catal-Log.
Build, test, publication et promotion d'une image Docker Nginx
via GitHub Actions et GitHub Container Registry.

## Liens rapides

- Dépôt : https://github.com/Candidat-1-ASRC/projet-cicd-ec06
- Image GHCR : ghcr.io/candidat-1-asrc/projet-cicd-ec06
- Actions : https://github.com/Candidat-1-ASRC/projet-cicd-ec06/actions

## Structure du projet

- site/ → Site statique servi par Nginx
- Dockerfile → Image Nginx reproductible
- compose.yml → Orchestration locale avec 2 services
- .github/workflows/ → Les 3 workflows GitHub Actions
- docs/ → Documentation complète

## Les 3 workflows

| Workflow | Déclencheur | Rôle |
|---------|-------------|------|
| 01-ci.yml | Push sur main | Build et test automatisé |
| 02-publish-ghcr.yml | Push sur main | Publication image dans GHCR |
| 03-promote.yml | Manuel | Promotion recette vers production |

## Pipeline

Build → Test → Publish → Recette → Production

## Formation

ASRC – EC06 – Bloc BC02