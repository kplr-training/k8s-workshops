### Objectif

Mettre à jour l'application Python sans provoquer d'interruption de service en utilisant Kubernetes.

### Étape 1: Apporter une Modification à l'Application

Dans cette étape, nous allons apporter une modification mineure à l'application en modifiant le message de retour dans `app.py`.

```python
# app.py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello, Kubernetes! This is version 2."

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)

```

### Étape 2: Construire une Nouvelle Image

Nous allons maintenant construire une nouvelle image avec une balise différente (`simple-python-app:v2`).

```sh
# Terminal
docker build -t simple-python-app:v2 .

```

### Étape 3: Mettre à Jour le Déploiement avec la Nouvelle Image

Mettez à jour le déploiement avec la nouvelle image que nous venons de construire.

```bash
# Terminal
kubectl set image deployment python-web-app-deployment python-web-app=simple-python-app:v2

```

### Étape 4: Surveiller la Progression de la Mise à Jour

Surveillez la progression de la mise à jour du déploiement à l'aide de la commande suivante :

```bash
# Terminal
kubectl rollout status deployment python-web-app-deployment
```

Cela affichera des informations sur la progression de la mise à jour. Attendez que le déploiement soit en cours de mise à jour.

### Étape 5: Vérifier la Mise à Jour

Ouvrez un navigateur et accédez à l'URL de service de l'application pour vérifier que la mise à jour a été effectuée avec succès. Si tout s'est bien passé, vous devriez voir le nouveau message de retour.