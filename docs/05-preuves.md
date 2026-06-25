# Preuves d'exécution - EC06

## Lien dépôt GitHub
https://github.com/Candidat-1-ASRC/projet-cicd-ec06

## Runs GitHub Actions

| Workflow | Statut |
|---------|--------|
| 01 - CI Build et Test | Succès |
| 02 - Publication GHCR | Succès |
| 03 - Promotion Recette vers Production | Succès |

## Image GHCR
- URL : ghcr.io/candidat-1-asrc/projet-cicd-ec06
- Tag SHA : sha-f2c3aab
- Tag latest : latest
- Tag version : 1.0.0
- Tag production : production

## Digest de l'image
sha256: (à compléter avec le digest visible dans GHCR)

## Tests locaux Docker
- docker build : réussi
- curl localhost:8080 : HTTP 200 OK
- Site affiché dans le navigateur : OK

## Tests Docker Compose
- docker compose up -d : 2 services démarrés
- Service web sur localhost:8080 : OK
- Service whoami sur localhost:8081 : OK
- Simulation scaling : WARNING container_name fixe (documenté)

## Environnements GitHub
- recette : configuré sans protection
- production-simulee : configuré avec approbation manuelle requise