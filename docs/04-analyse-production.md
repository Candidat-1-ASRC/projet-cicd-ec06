# Analyse : Passage vers une Production Réelle - EC06

## 1. Gestion des secrets

Principe fondamental : aucun secret ne doit jamais être versionné
dans le code source.

Dans ce projet, le seul secret utilisé est le GITHUB_TOKEN,
automatiquement fourni par GitHub Actions à chaque run.

En production réelle il faudrait :
- Placer les credentials dans GitHub Secrets ou un coffre de secrets
  comme HashiCorp Vault, AWS Secrets Manager ou Azure Key Vault
- Utiliser des tokens à durée de vie courte avec rotation automatique
- Auditer les accès aux secrets

## 2. Rollback

Le rollback est rendu possible par la stratégie d'artefact immuable :

1. Chaque image est taguée avec son SHA de commit (sha-abc1234)
2. Chaque image possède un digest immuable (sha256:...)
3. Pour revenir en arrière, on relance le workflow 03-promote.yml
   avec le tag de la version précédente
4. Aucun rebuild n'est nécessaire

Le digest garantit qu'on redéploie exactement la même image bit pour bit.

## 3. Sauvegarde et restauration

| Élément | Sauvegarde |
|---------|-----------|
| Code source | Dépôt GitHub avec historique Git |
| Workflows | Versionnés dans .github/workflows/ |
| Documentation | Versionnée dans docs/ |
| Images Docker | Conservées dans GHCR avec tags versionnés |
| Configuration | Versionnée dans compose.yml et Dockerfile |

Pour restaurer : cloner le dépôt et re-promouvoir le tag voulu depuis GHCR.

## 4. Éléments complémentaires pour une production réelle

### Répartition de charge
Utiliser un load balancer devant plusieurs réplicas du service web
comme Nginx upstream, HAProxy ou un load balancer cloud.

### Supervision
Mettre en place des alertes sur les métriques de disponibilité,
temps de réponse et utilisation des ressources avec Prometheus,
Grafana ou Datadog.

### Haute disponibilité
Déployer sur plusieurs serveurs ou zones pour éviter un point
de défaillance unique.

### Contrôle des vulnérabilités
Intégrer un scanner comme Trivy dans le pipeline CI/CD pour
détecter les failles dans l'image Docker avant publication.