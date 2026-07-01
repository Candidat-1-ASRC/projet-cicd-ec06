# 03 - Fiche tests

## Test automatisé GitHub Actions

- Workflow concerné : 01-ci.yml
- Lien vers le run réussi : https://github.com/Candidat-1-ASRC/projet-cicd-ec06/actions/workflows/01-ci.yml
- Ce qui est testé :
  - Construction de l'image Docker depuis le Dockerfile
  - Lancement du conteneur sur le port 8080
  - Test HTTP : code 200 attendu sur http://localhost:8080
  - Vérification du contenu HTML : présence de "Catal-Log"
  - Vérification du fichier version.json : présence de "EC06"
- Résultat : Tous les tests passent avec succès. Code HTTP 200 reçu. Contenu HTML et version.json validés.

## Test local Docker ou Docker Compose

### Situation A - Test réalisé

Commandes utilisées :

```bash
docker build -t catal-log:local .
docker run -d --name test-local -p 8080:80 catal-log:local
curl http://localhost:8080
```

Résultat observé : Image construite avec succès. Conteneur démarré. Site accessible sur http://localhost:8080 avec code HTTP 200. Page Catal-Log affichée dans le navigateur.

```bash
docker compose up -d
docker compose ps
```

Résultat observé : Les deux services catal-log-web et catal-log-whoami démarrés avec succès. Service web accessible sur localhost:8080, service whoami sur localhost:8081.

## Simulation de scaling

```bash
docker compose up -d --scale web=2
docker compose ps
```

Résultat observé :
WARNING: The "web" service is using the custom container name "catal-log-web". Docker requires each container to have a unique name. Remove the custom name to scale the service.

Docker Compose ne peut pas créer un second conteneur car le container_name est fixe. C'est une limite connue et documentée.

## Limites de la simulation

- Absence de vrai load balancer : sans proxy frontal (Nginx upstream, HAProxy), les deux réplicas ne reçoivent pas de trafic équilibré
- Absence de haute disponibilité : tout tourne sur un seul hôte Windows
- Absence de supervision : pas d'alertes en cas de panne
- container_name fixe : empêche le scaling horizontal sans modification du compose.yml
- Dépendance à l'environnement local : si Docker Desktop est arrêté, tout s'arrête
- En production réelle, Kubernetes gèrerait le scaling, le self-healing et la répartition de charge automatiquement
