# Preuves d'exécution - EC06

## Lien dépôt GitHub
https://github.com/Candidat-1-ASRC/projet-cicd-ec06

## Runs GitHub Actions

| Workflow | Lien | Statut |
|---------|------|--------|
| 01 - CI Build et Test | https://github.com/Candidat-1-ASRC/projet-cicd-ec06/actions/workflows/01-ci.yml | Succès |
| 02 - Publication GHCR | https://github.com/Candidat-1-ASRC/projet-cicd-ec06/actions/workflows/02-publish-ghcr.yml | Succès |
| 03 - Promotion Recette vers Production | https://github.com/Candidat-1-ASRC/projet-cicd-ec06/actions/workflows/03-promote.yml | Succès |

## Image GHCR
- URL : ghcr.io/candidat-1-asrc/projet-cicd-ec06
- Tag SHA : sha-f2c3aab
- Tag latest : latest
- Tag version : 1.0.0
- Tag production : production
- Digest : sha256: (à compléter avec le digest visible dans GHCR)

## Preuve build Docker automatisé
- Workflow 01-ci.yml exécuté avec succès sur ubuntu-latest
- Commande : docker build -t catal-log:ci-test .
- Résultat : image construite sans erreur

## Preuve test HTTP automatisé
- Test curl sur http://localhost:8080
- Code HTTP retourné : 200
- Contenu vérifié : présence de "Catal-Log" dans le HTML

## Preuve publication GHCR
- Image publiée via workflow 02-publish-ghcr.yml
- Tags générés : sha-f2c3aab, latest, 1.0.0
- Image visible sur ghcr.io/candidat-1-asrc/projet-cicd-ec06

## Preuve validation recette simulée
- Workflow 03-promote.yml déclenché manuellement
- Job "Validation en Recette" : succès
- Test HTTP sur l'image depuis GHCR : code 200

## Preuve promotion production sans rebuild
- Même image taguée sha-f2c3aab re-taguée en production
- Aucun rebuild effectué
- Tag production visible dans GHCR

## Tests locaux Docker
- docker build -t catal-log:local . : succès
- docker run -p 8080:80 : succès
- curl localhost:8080 : HTTP 200 OK
- Site affiché dans le navigateur : OK

## Tests Docker Compose
- docker compose up -d : 2 services démarrés
- Service web sur localhost:8080 : OK
- Service whoami sur localhost:8081 : OK
- Simulation scaling : WARNING container_name fixe (documenté)

## VM personnelle
Pas de VM personnelle disponible.
Les tests locaux ont été effectués directement sur le poste
de travail avec Docker Desktop sous Windows.

## Environnements GitHub
- recette : configuré sans protection
- production-simulee : configuré avec approbation manuelle requise