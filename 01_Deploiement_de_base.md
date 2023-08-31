Déployer une application Python simple à l'aide d'une ressource de déploiement.

**Objectifs:**

- Comprendre les concepts de base de Kubernetes.
- Installer un cluster Kubernetes local (par exemple, avec Minikube).
- Créer et déployer un Pod simple.

1. Créez un fichier `app.py` avec le contenu suivant pour votre application Python simple :

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello Kubernetes!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=80)

```

2. Créez un fichier `Dockerfile` pour créer une image Docker de votre application :

```dockerfile
FROM python:3.8

COPY app.py /app.py
RUN pip install flask

CMD ["python", "/app.py"]

```

3. Construisez l'image Docker : 

	`docker build -t simple-python-app:v1 .`
    
4. Créez un déploiement Kubernetes en utilisant la commande `kubectl create deployment` :

`kubectl create deployment simple-python-app --image=simple-python-app:v1
`
5. Exposez le déploiement via un service :
`kubectl expose deployment simple-python-app --type=NodePort --port=80
`

6. Obtenez le port exposé :

`kubectl get svc simple-python-app`

7. Accédez à l'application depuis votre navigateur en utilisant l'IP du nœud et le port exposé.