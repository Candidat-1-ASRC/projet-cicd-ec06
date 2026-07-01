# 06 - Preuve promotion production-simulee

## Promotion

- Workflow concerné : 03-promote.yml
- Environnement GitHub : production-simulee
- Tag source : sha-f2c3aab
- Tag cible : production
- Lien du run : https://github.com/Candidat-1-ASRC/projet-cicd-ec06/actions/workflows/03-promote.yml

## Point essentiel

La promotion réutilise une image existante. Elle ne reconstruit pas l'image.

Les commandes exécutées dans le workflow sont :
```bash
docker pull ghcr.io/candidat-1-asrc/projet-cicd-ec06:sha-f2c3aab
docker tag ghcr.io/candidat-1-asrc/projet-cicd-ec06:sha-f2c3aab ghcr.io/candidat-1-asrc/projet-cicd-ec06:production
docker push ghcr.io/candidat-1-asrc/projet-cicd-ec06:production
```

Aucune commande docker build n'est présente dans ce job. C'est la preuve que l'image n'est pas reconstruite.

## Preuve

Le job "Promotion vers Production Simulee" s'est exécuté en 9 secondes. Une reconstruction complète prendrait plusieurs dizaines de secondes car elle nécessiterait de retélécharger les couches de l'image de base et de recopier les fichiers.

La rapidité de l'exécution (9 secondes) confirme qu'il s'agit uniquement d'un re-tag et d'un push, pas d'un rebuild.

Le tag :production est désormais visible dans GHCR et pointe vers le même digest sha256 que le tag sha-f2c3aab. C'est la preuve que c'est exactement le même artefact.

L'approbation manuelle dans l'environnement production-simulee a été accordée avant l'exécution du job, ce qui simule la validation humaine requise avant tout déploiement en production réelle.
