# 05 - Preuve recette simulée

## Workflow de validation recette

- Workflow concerné : 03-promote.yml
- Environnement GitHub : recette
- Tag source validé : sha-f2c3aab
- Digest observé : sha256: (à compléter avec le digest visible dans GHCR)
- Lien du run : https://github.com/Candidat-1-ASRC/projet-cicd-ec06/actions/workflows/03-promote.yml

## Résultat

Le job "Validation en Recette" a été exécuté avec succès en 11 secondes.

Déroulement :
1. Connexion à GHCR via GITHUB_TOKEN
2. Pull de l'image ghcr.io/candidat-1-asrc/projet-cicd-ec06:sha-f2c3aab
3. Lancement du conteneur sur le port 8080
4. Test HTTP sur http://localhost:8080
5. Code HTTP reçu : 200
6. Message affiché : "Validation recette reussie"
7. Arrêt et suppression du conteneur de test

Le test HTTP confirme que l'image publiée dans GHCR est fonctionnelle et sert correctement le site statique Catal-Log.
