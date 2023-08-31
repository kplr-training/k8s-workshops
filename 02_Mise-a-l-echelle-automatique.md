Dans ce workshop, nous allons explorer comment configurer la mise à l'échelle automatique d'une application Kubernetes en fonction de l'utilisation CPU. Nous allons appliquer la mise à l'échelle sur l'application Python que nous avons déployée précédemment.

**Objectifs:**

- Comprendre les Services Kubernetes.
- Exposer l'application via un Service.
- Configurer la mise à l'échelle automatique basée sur l'utilisation CPU.

### Étape 1: Appliquer la mise à l'échelle automatique basée sur l'utilisation CPU

Nous allons commencer par appliquer la mise à l'échelle automatique à notre déploiement existant.



```sh
kubectl autoscale deployment simple-python-app --cpu-percent=50 --min=1 --max=10
```

Explication de la commande:

- `python-web-app-deployment` : Le nom du déploiement que nous voulons mettre à l'échelle.
- `--cpu-percent=50` : L'utilisation CPU moyenne souhaitée. Le déploiement sera mis à l'échelle en conséquence.
- `--min=1` : Le nombre minimum de répliques du déploiement.
- `--max=10` : Le nombre maximum de répliques du déploiement.

### Étape 2: Générer de la charge sur l'application

Maintenant que nous avons configuré la mise à l'échelle automatique, générons de la charge sur l'application pour voir la mise à l'échelle en action.

```sh
# Obtenir l'adresse IP du service
SERVICE_IP=$(minikube service python-web-app-service --url)

# Utiliser la commande 'curl' pour envoyer des requêtes à l'application
# Remarque : vous pouvez exécuter cette commande dans un autre terminal
while true; do curl $SERVICE_IP; done

```

Laissez cette commande s'exécuter pendant un moment pour générer de la charge sur l'application. Pendant ce temps, observez les changements dans le nombre de répliques du déploiement à l'aide de la commande :

```sh
kubectl get deployment python-web-app-deployment
```

Vous verrez probablement que le nombre de répliques augmente lorsque la charge CPU augmente et diminue lorsque la charge diminue.