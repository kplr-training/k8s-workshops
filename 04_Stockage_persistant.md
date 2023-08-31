### Objectif

Dans cet atelier, vous apprendrez comment utiliser un volume persistant pour stocker les données de votre application. Nous allons utiliser un fichier `index.html` comme exemple et configurer un volume persistant pour le stocker.

### Étape 1: Créer le Fichier `index.html`

Tout d'abord, créons un fichier `index.html` avec un contenu simple.

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Simple Python App</title>
</head>
<body>
    <h1>Welcome to the Simple Python App!</h1>
</body>
</html>

```

### Étape 2: Mettre à Jour le Fichier `Dockerfile`

Mettez à jour votre fichier `Dockerfile` pour copier le fichier `index.html` dans un dossier web de l'image.

```Dockerfile
# Dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY app.py app.py
COPY index.html /var/www/html

CMD ["python", "app.py"]

```

### Étape 3: Construire la Nouvelle Image

Construisez la nouvelle image avec la commande suivante :

```bash
# Terminal
docker build -t simple-python-app:v3 .
```

### Étape 4: Créer une Ressource de ReplicaSet avec un Volume Persistant

Créez un fichier `replicaset.yaml` avec le contenu suivant :

```yaml
# replicaset.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: simple-python-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: simple-python-app
  template:
    metadata:
      labels:
        app: simple-python-app
    spec:
      containers:
        - name: simple-python-app
          image: simple-python-app:v3
          volumeMounts:
            - name: html-volume
              mountPath: /var/www/html
      volumes:
        - name: html-volume
          hostPath:
            path: /path/to/host/folder

```

Assurez-vous de remplacer `/path/to/host/folder` par le chemin absolu d'un dossier sur votre hôte.

### Étape 5: Appliquer la Configuration dans Kubernetes

Appliquez la configuration dans Kubernetes à l'aide de la commande suivante :

```bash
# Terminal
kubectl apply -f replicaset.yaml
```

