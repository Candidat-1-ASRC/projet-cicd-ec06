# Fiche Sécurité - EC06

## Secrets et tokens

### GITHUB_TOKEN
- Token temporaire généré automatiquement par GitHub Actions à chaque run
- N'est jamais stocké dans le code source
- Durée de vie limitée à l'exécution du workflow
- Permissions déclarées explicitement dans chaque workflow

### Règle fondamentale
Aucun secret ne doit jamais apparaître dans le code source.
Tout secret de production doit être placé dans GitHub Secrets
ou dans un coffre de secrets en production réelle.

### Ce qui irait dans GitHub Secrets en production
- Clés d'accès au registre d'images privé
- Credentials de connexion aux serveurs
- Clés SSH de déploiement
- Tokens d'APIs externes

## Sécurité de l'image Docker

| Mesure | Implémentation |
|--------|---------------|
| Image de base minimale | nginx:alpine (Alpine Linux) |
| Healthcheck intégré | Vérifie la disponibilité du service |
| Pas de secret dans l'image | Aucune variable d'environnement sensible |

## Limites identifiées
- Pas de scan de vulnérabilités automatisé
- Pas de signature d'image
- Pas de politique de rotation des secrets
- Environnement de production simulé uniquement