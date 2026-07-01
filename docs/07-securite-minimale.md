# 07 - Sécurité minimale

## Permissions GitHub Actions

Les permissions sont déclarées explicitement dans chaque workflow et limitées au strict nécessaire :

- 01-ci.yml : contents: read
  → Le workflow doit uniquement lire le code source pour construire l'image. Aucune écriture nécessaire.

- 02-publish-ghcr.yml : contents: read, packages: write
  → Le workflow doit lire le code source et écrire l'image dans GHCR. La permission packages: write est nécessaire pour le push.

- 03-promote.yml : contents: read, packages: write
  → Le workflow doit lire le code et écrire dans GHCR pour le re-tag. Aucune autre permission n'est nécessaire.

Limiter les permissions au strict nécessaire réduit la surface d'attaque. Si un workflow est compromis, il ne peut faire que ce pour quoi il a été autorisé.

## Gestion des secrets

**Pourquoi aucun secret ne doit être stocké dans le code :**
Un secret stocké dans le code source est visible par tous ceux qui ont accès au dépôt, et reste dans l'historique Git même après suppression. Un dépôt public expose ces secrets au monde entier. C'est une faille de sécurité critique.

**Usage du GITHUB_TOKEN dans ce projet :**
Le GITHUB_TOKEN est un token temporaire généré automatiquement par GitHub Actions à chaque exécution de workflow. Il est injecté dans l'environnement du runner et n'est jamais stocké dans le code. Sa durée de vie est limitée à l'exécution du workflow. Il est utilisé pour se connecter à GHCR et pousser l'image Docker.

**Éléments à placer dans GitHub Secrets ou un coffre de secrets en production :**
- Credentials d'un registre d'images privé externe
- Clés SSH pour se connecter aux serveurs de déploiement
- Tokens d'APIs externes (monitoring, notifications)
- Certificats TLS
- Mots de passe de bases de données

## Rollback

Pour revenir à une version précédente :
1. Identifier le tag SHA de la version souhaitée dans GHCR (ex: sha-abc1234)
2. Relancer le workflow 03-promote.yml manuellement via workflow_dispatch
3. Saisir le tag SHA de la version précédente dans le champ image_tag
4. Valider et approuver la promotion
5. L'image ancienne est re-taguée en :production sans rebuild

Le digest sha256 garantit qu'on redéploie exactement la même image que celle qui a été testée lors de la première promotion. Le rollback est rapide, traçable et sans risque de régression liée à un rebuild.

## Sauvegarde / restauration

**Ce qu'il faudrait sauvegarder :**
- Dépôt GitHub : l'historique Git contient tout le code, les workflows et la documentation. Un fork ou un clone régulier suffit.
- Images GHCR : ne pas supprimer les tags versionnés (sha-xxxxxxx et 1.0.0). Les conserver indéfiniment permet le rollback.
- Configuration des environnements GitHub : documenter les règles de protection de production-simulee.
- Captures d'écran et preuves : conserver localement en cas de suppression du dépôt.

**Comment restaurer :**
1. Cloner le dépôt GitHub sur un nouveau poste
2. Recréer les environnements GitHub (recette et production-simulee)
3. Re-promouvoir le tag voulu depuis GHCR via le workflow 03
4. Le site est de nouveau opérationnel

## Deux éléments complémentaires

**Répartition de charge :**
En production réelle, plusieurs réplicas du service web seraient déployés derrière un load balancer (Nginx upstream, HAProxy ou load balancer cloud). Cela permet de distribuer le trafic, d'absorber les pics de charge et de maintenir la disponibilité si un réplica tombe.

**Supervision :**
Un système de supervision (Prometheus + Grafana, Datadog) surveillerait en permanence la disponibilité HTTP, le temps de réponse, l'utilisation CPU et mémoire des conteneurs. Des alertes seraient déclenchées automatiquement en cas d'anomalie, permettant une intervention rapide avant que les utilisateurs ne soient impactés.
