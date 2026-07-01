# 01 - Cadrage du projet

## Identité

- Nom et prénom : Candidat-1-ASRC
- Dépôt GitHub : https://github.com/Candidat-1-ASRC/projet-cicd-ec06
- Date de démarrage : 25/06/2026

## Objectif

Mettre en place une chaîne CI/CD permettant de construire, tester, publier et promouvoir une image Docker Nginx contenant un site web statique pour le scénario Catal-Log.

## Contraintes du projet

- Travail individuel.
- Aucune infrastructure fournie, préparée, administrée ou maintenue par le formateur.
- Pas de serveur distant, pas de SSH, pas de cloud provider imposé.
- Les traitements principaux sont exécutés dans GitHub Actions.
- Docker Desktop est installé et fonctionnel sur le poste local Windows.
- Pas de VM personnelle disponible : justification ci-dessous.

## Choix personnels

- Dépôt public sur GitHub sous le compte anonyme Candidat-1-ASRC
- Nom du dépôt : projet-cicd-ec06
- Stratégie de tags : sha court (sha-xxxxxxx), latest et 1.0.0
- Environnement local utilisé : Windows avec Docker Desktop
- Pas de VM personnelle : les tests locaux ont été réalisés directement sur le poste Windows avec Docker Desktop, ce qui est équivalent pour ce projet pédagogique
- Image de base choisie : nginx:alpine pour sa légèreté et sa sécurité
- Registre d'images : GHCR (GitHub Container Registry)
- Deux environnements GitHub simulés : recette (sans protection) et production-simulee (avec approbation manuelle requise)
