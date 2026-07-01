# 04 - Preuve image GHCR

## Image publiée

- Nom de l'image : ghcr.io/candidat-1-asrc/projet-cicd-ec06
- Tag principal : sha-f2c3aab
- Tag latest : latest
- Tag version : 1.0.0
- Tag production : production
- Digest : sha256: (à compléter avec le digest visible dans GHCR)
- Lien GHCR : https://github.com/Candidat-1-ASRC/projet-cicd-ec06/pkgs/container/projet-cicd-ec06

## Explication

**Pourquoi le tag est utile :**
Le tag SHA (sha-f2c3aab) identifie précisément le commit qui a généré l'image. Il permet de savoir exactement quelle version du code correspond à quelle image. En cas de problème en production, on peut identifier immédiatement quelle version a été déployée et depuis quel commit.

**Pourquoi le digest est utile :**
Le digest sha256 est une empreinte cryptographique unique et immuable de l'image. Contrairement au tag (qui peut être réassigné), le digest ne change jamais. Il garantit que l'image déployée en production est exactement la même que celle testée en recette, bit pour bit. C'est la garantie absolue de l'intégrité de l'artefact.

**Rollback :**
En cas de régression, il suffit de relancer le workflow 03-promote.yml avec le tag SHA d'une version précédente. L'image est déjà dans GHCR, aucun rebuild n'est nécessaire. Le rollback est instantané et traçable.
