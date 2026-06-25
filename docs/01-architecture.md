# Architecture du Projet CI/CD - EC06

## Schéma d'ensemble

[Dev Local] → git push → [GitHub]
                              ↓
                    [GitHub Actions - 01-ci.yml]
                    - Build image Docker
                    - Test HTTP automatisé
                    - Vérification contenu
                              ↓
                    [GitHub Actions - 02-publish-ghcr.yml]
                    - Login GHCR via GITHUB_TOKEN
                    - Push image avec tags sha, latest, 1.0.0
                    - Digest enregistré
                              ↓
                    [GitHub Actions - 03-promote.yml]
                    - Pull image depuis GHCR
                    - Validation en recette
                    - Approbation manuelle requise
                    - Re-tag production sans rebuild

## Composants

| Composant | Rôle |
|-----------|------|
| GitHub | Dépôt, historique, documentation, preuves |
| GitHub Actions | Automatisation CI/CD complète |
| GHCR | Registre d'images Docker |
| Dockerfile | Image Nginx reproductible |
| compose.yml | Orchestration locale |
| Environnements GitHub | Simulation recette et production |

## Flux de l'artefact

L'image est construite une seule fois lors du CI.
Elle est identifiée par son tag SHA et son digest immuable.
La promotion ne reconstruit pas l'image : elle re-tag l'artefact existant.
C'est le principe fondamental de la traçabilité CI/CD.