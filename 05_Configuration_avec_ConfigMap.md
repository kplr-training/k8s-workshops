**Objectif:** Utiliser ConfigMap pour gérer la configuration de l'application.

Dans cet atelier, nous allons apprendre comment utiliser ConfigMap pour gérer la configuration d'une application Kubernetes.

### Étape 1: Créer un fichier de configuration ConfigMap

1. Créez un fichier `config.yaml` avec la configuration suivante :

```yaml
apiVersion: v1
kind: ConfigMap 
metadata:   
  name: app-config 
data:   
  message: "Bonjour depuis Kubernetes!"

```

Ce fichier définit une ConfigMap nommée `app-config` avec une clé `message` contenant le message de salutation.

### Étape 2: Appliquer la configuration ConfigMap

2. Appliquez la configuration en exécutant la commande suivante :

```bash
kubectl apply -f config.yaml
```

Cela créera la ConfigMap dans votre cluster Kubernetes.

### Étape 3: Mettre à jour le déploiement pour utiliser ConfigMap

3. Mettez à jour le fichier `deployment.yaml` pour utiliser la valeur de ConfigMap comme variable d'environnement pour l'application :

```yaml
spec:
  containers:
    - name: simple-python-app
      image: simple-python-app:v3
      env:
        - name: MESSAGE
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: message

```

Dans cette section, nous avons ajouté une variable d'environnement `MESSAGE` à notre conteneur. La valeur de cette variable est extraite de la ConfigMap `app-config` en utilisant la clé `message`.

### Étape 4: Redéployer le déploiement

4. Redéployez le déploiement pour prendre en compte la nouvelle configuration :
```bash
kubectl apply -f deployment.yaml
```

Cela mettra à jour les pods en cours d'exécution avec la nouvelle configuration.
