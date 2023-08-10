# CKA (administrateur certifié Kubernetes)
- [CKA (Administrateur certifié Kubernetes)] (# CKA-certifié-Kubernetes-adminrator)- [Configuration] (# Configuration)- [Pods] (# Pods)- [Dépannage Pods] (# Dépannage-pods)- [Espaces de noms] (# Espaces)- [nœuds] (# nœuds)- [Services] (# Services)- [répliques] (# répliques)- [Dépannage des répliques] (# Dépannage-replicasts)- [Déploiements] (# déploiements)- [Dépannage des déploiements] (# Dépannage des déploiements)- [planificateur] (# planificateur)- [Affinité du nœud] (# Node-Affinity)- [Étiquettes et sélecteurs] (# étiquettes et sélecteurs)- [Sélecteur de nœud] (# Node-Selector)- [Taints] (# Taints)- [limites de ressources] (# Resources-Limits)- [surveillance] (# surveillance)- [Scheduler] (# Scheduler-1)
## Installation
* Configurez le cluster Kubernetes.Utiliser sur ce qui suit1. Minikube pour un cluster gratuit et simple2. Cluster géré (Ex, GKE, AKS)
* Définir les alias
`` 'alias k = kubectlAlias ​​et = Huketl Supprimeralias kds = kubectl décrireAlias ​​k = Cubstal Editalias kr = run kubectlalias kg = kubectl get`` '
## PODS
<Dettots><summary> Exécutez une commande pour afficher toutes les pods dans l'espace de noms actuel </summary> <br> <b>
`Kubectl get gods`
Remarque: Créez un alias (`alias k = kubectl`) et habituez-vous à` k get po`</b> </fettest>
<Dettots><summary> Exécutez une pod appelée "nginx-test" en utilisant l'image "nginx" </summary> <br> <b>
`k run nginx-test --image = nginx`</b> </fettest>
<Dettots><summary> En supposant que vous avez une pod appelée "Nginx-test", comment le supprimer? </summary> <br> <b>
«k supprimer nginx-test»</b> </fettest>
<Dettots><summary> Dans quel espace de noms l'espace <code> etcd </code> Pod est en cours d'exécution?Énumérez les pods dans cet espace de noms </summary> <br> <b>
Marven a un pauvre - Kijien Tiet Tabiftife Ifious
Disons que vous ne saviez pas de quel espace de noms.Vous pouvez ensuite exécuter `K Get Po -a |grep etc «pour trouver le pod et voir dans quel espace de noms il réside.</b> </fettest>
<Dettots><summary> Liste des pods de tous les espaces de noms </summary> <br> <b>
`k get po -a`
La version longue serait `Kubectl get pods - allamespaces`.</b> </fettest>
<Dettots><summary> Écrivez un yaml d'une pod avec deux conteneurs et utilisez le fichier yaml pour créer le pod (utilisez les images que vous préférez) </summary> <br> <b>
`` 'chat> pod.yaml << eolApversion: V1genre: godC'est le bordel:nom: testerSPEC:conteneurs:- Image: AlpineNom: Alpine- Image: Nginx-UnpriviledNom: Nginx-inprividegedEol
k Create -f pod.yaml`` '
Si vous vous demandez comment je me souviens avoir écrit tout cela?Pas de soucis, vous pouvez simplement exécuter `kubectl exécuter quelque_pod --image = redis -o yaml --dry-run = client> pod.yaml`.Si vous vous demandez "comment suis-je censé me souvenir de cette longue commande" temps pour changer d'attitude;)</b> </fettest>
<Dettots><summary> Créez un yaml d'une pod sans exécuter réellement le pod avec la commande kubectl (utilisez l'image que vous préférez) </summary> <br> <b>
`k exécuter quelque-pod -o yaml --image nginx-unprivileged --dry-run = client> pod.yaml`</b> </fettest>
<Dettots><summary> Comment tester un manifeste est valide? </summary> <br> <b>
Avec un drapeau `--sécheur» qui ne le créera pas réellement, mais il le testera et vous pouvez trouver de cette façon tous les problèmes de syntaxe.
`k Create -f yaml_file --sécheur`</b> </fettest>
<Dettots><summary> Comment vérifier à quelle image un certain pod utilise? </summary> <br> <b>
`k décrire po <pod_name> |grep -i image`</b> </fettest>
<Dettots><summary> Comment vérifier combien de conteneurs fonctionnent en un seul pod? </summary> <br> <b>
`K Get PO POD_NAME` et voyez le numéro dans la colonne" Ready ".
Vous pouvez également exécuter `k décrire po pod_name`</b> </fettest>
<Dettots><summary> Exécutez une pod appelée "Remo" avec la dernière image Redis et l'étiquette 'année = 2017' </summary> <br> <b>
`K Run Remo --image = redis: Derniter -L Year = 2017`</b> </fettest>
<Dettots><summary> Liste des pods et leurs étiquettes </summary> <br> <b>
`K Get Po --show-Babels`</b> </fettest>
<Dettots><summary> Supprimer une pod appelée "nm" </summary> <br> <b>
`k supprimer po nm`</b> </fettest>
<Dettots><summary> Énumérez toutes les pods avec le label "Env = prod" </summary> <br> <b>
`k get po -l env = prod`
Pour les compter: `K Get Po -l env = prod --no-headers |wc -l`</b> </fettest>
<Dettots><summary> Créez un pod statique avec l'image <code> python </code> qui exécute la commande <code> sleep 2017 </code> </summary> <br> <b>
Tout d'abord, le répertoire suivi par Kubelet pour créer un pod statique: `CD / etc / kubernetes / manifestes` (vous pouvez vérifier le chemin en lisant le fichier Kubelet conf.)
Créez maintenant la définition / manifeste dans ce répertoire`k exécuter quelque-pod --image = python - Command Sleep 2017 --restart = never --ry-run = client -o yaml> static-pod.yaml`</b> </fettest>
<Dettots><Summary> Décrivez comment supprimer un pod statique</summary> <br> <b>
Localisez le répertoire Static Pods (regardez `staticpodpath` dans le fichier de configuration de Kubelet).
Allez dans ce répertoire et supprimez le manifeste / définition du pod staic (`rm <static_pod_path> / <pod_definition_file>`)</b> </fettest>
### Dépannage des gousses
<Dettots><Summary> Vous essayez d'exécuter un pod mais voyez le statut "crashloopbackoff".Qu'est-ce que ça veut dire?Comment identifier le problème? </summary> <br> <b>
Le conteneur n'a pas fonctionné (pour des raisons différentes) et Kubernetes essaie à nouveau d'exécuter le pod après un certain retard (= temps de revers).
Quelques raisons pour que cela échoue:- erreurs de conformité - Opelage erroné, valeur non prise en charge, etc.- Ressource non disponible - Les nœuds sont en panne, PV non monté, etc.
Certaines façons de déboguer:
1. `kubectl décrire pod pod_name`1. Concentrez-vous sur «State» (qui devrait attendre, Crashloopbackoff) et «Last State» qui devrait dire ce qui s'est passé avant (comme pourquoi il a échoué)2. Exécutez `kubectl journaux mypod`1. Cela devrait fournir une sortie précise de2. Pour un conteneur spécifique, vous pouvez ajouter `-c conteneur_name`3. Si vous ne savez toujours pas pourquoi il a échoué, essayez «Kubectl Get Events»4</b> </fettest>
<Dettots><summary> Ce que signifie l'erreur <code> ImagePullbackoff </code>? </summary> <br> <b>
Vous n'avez probablement pas écrit correctement le nom de l'image que vous essayez de tirer et d'exécuter.Ou peut-être qu'il n'existe pas dans le registre.
Vous pouvez confirmer avec `kubectl décrire po pod_name`</b> </fettest>
<Dettots><summary> Comment vérifier quel nœud un certain pod est en cours d'exécution? </summary> <br> <b>
`k get po pod_name -o large`</b> </fettest>
<Dettots><summary> Exécutez la commande suivante: <code> kubectl run ohno --image = sheris </code>.Cela a-t-il fonctionné?pourquoi pas?Réparez-le sans retirer le pod et en utilisant n'importe quelle image que vous souhaitez </summary> <br> <b>
Parce qu'il n'y a pas une telle image «Sheris».Au moins pour l'instant :)
Pour le réparer, exécutez `kubectl edit ohno` et modifiez la ligne suivante` - image: sheris` vers `- image: redis` ou toute autre image que vous préférez.</b> </fettest>
<Dettots><Summary> Vous essayez d'exécuter un pod mais il est dans l'état "en attente".Quelle pourrait être la raison? </summary> <br> <b>
Une raison possible est que le planificateur qui censé planifier des pods sur les nœuds n'est pas en cours d'exécution.Pour le vérifier, vous pouvez exécuter `kubectl get po -a |Grep Scheduler` ou vérifiez directement dans l'espace de noms «Kube-System».</b> </fettest>
<Dettots><summary> Comment afficher les journaux d'un conteneur fonctionnant dans un pod? </summary> <br> <b>
`k logs pod_name`</b> </fettest>
<Dettots><Summary> Il y a deux conteneurs à l'intérieur d'un POD appelé "Some-Pod".Que se passera-t-il si vous exécutez <code> kubectl journaux quelque pod </code> </summary> <br> <b>
Cela ne fonctionnera pas car il y a deux conteneurs à l'intérieur du pod et vous devez en spécifier l'un avec `kubectl journaux pod_name -c contener_name`</b> </fettest>
## Espaces de noms
<Dettots><summary> Énumérez toutes les espaces de noms </summary> <br> <b>
«k get ns»</b> </fettest>
<Dettots><summary> Créez un espace de noms appelé 'alle' </summary> <br> <b>
`K Créer ns alle`</b> </fettest>
<Dettots><summary> Vérifiez combien d'espaces de noms y a-t-il </summary> <br> <b>
`K Get NS --No-Headers |wc -l`</b> </fettest>
<Dettots><summary> Vérifiez combien de pods existent dans l'espace de noms "Dev" </summary> <br> <b>
Marven a un pauvre - - Shen</b> </fettest>
<Dettots><summary> Créez une pod appelée "Kartos" dans le dev.Le pod doit utiliser l'image "redis". </summary> <br> <b>
Si l'espace de noms n'existe pas déjà: `K Créer ns Dev`
`k run kratos --image = redis -n dev`</b> </fettest>
<Dettots><Summary> Vous cherchez une pod appelée "Atreus".Comment vérifier dans quel espace de noms il fonctionne? </summary> <br> <b>
Hoak a un pauvre -i eh |Agat Heart est avec lui hipaire.</b> </fettest>
## nœuds
<Dettots><summary> Exécutez une commande pour afficher tous les nœuds du cluster </summary> <br> <b>
`Kubectl get nœuds '
Remarque: Créez un alias (`alias k = kubectl`) et habituez-vous à` k obtenez no`</b> </fettest>
<Dettots><summary> Créez une liste de tous les nœuds au format JSON et stockez-le dans un fichier appelé "some_nodes.json" </summary> <br> <b>
`k get nœuds -o json> some_nodes.json`</b> </fettest>
<Dettots><summary> Vérifiez les étiquettes que l'un de vos nœuds dans le cluster a </summary> <br> <b>
«k ne pas obtenir de minikube - show-labels»</b> </fettest>
## Prestations de service
<Dettots><summary> Vérifiez combien de services fonctionnent dans l'espace de noms actuel </summary> <br> <b>
`k get svc`</b> </fettest>
<Dettots><summary> Créez un service interne appelé "Sevi" pour exposer l'application «Web» sur le port 1991 </summary> <br> <b>
`kubectl exposer pod web --port = 1991 --name = sevi`</b> </fettest>
<Dettots><summary> Comment faire référence par son nom un service appelé "App-Service" dans le même espace de noms? </summary> <br> <b>
service d'application</b> </fettest>
<Dettots><summary> Comment vérifier le Targetport d'un service? </summary> <br> <b>
`k Décrivez SVC <ferge_name>`</b> </fettest>
<Dettots><summary> Comment vérifier les points de terminaison du SVC? </summary> <br> <b>
`k Décrivez SVC <ferge_name>`</b> </fettest>
<Dettots><summary> Comment faire référence par son nom un service appelé "App-Service" dans un autre espace de noms, appelé "Dev"? </summary> <br> <b>
app-service.dev.svc.cluster.local</b> </fettest>
<Dettots><summary> Supposons que vous avez un déploiement en cours d'exécution et que vous devez créer un service pour exposer les pods.C'est ce qui est nécessaire / connu:
* Nom du déploiement: juste* Port cible: 8080* Type de service: nodeport* Sélecteur: Jabulik-App* Port: 8080</summary> <br> <b>
`Kubectl Expose Deployment Jabulik --name = Jabulik-Service --Target-Port = 8080 --Type = NodePort --port = 8080 - Dry-run = Client -o Yaml -> Svc.yaml`
`vi svc.yaml` (assurez-vous que le sélecteur est défini sur` jabulik-appa`)
`k appliquer -f svc.yaml`</b> </fettest>
## répliques
<Dettots><summary> Comment vérifier combien de répliques définies dans l'espace de noms actuel? </summary> <br> <b>
`k get rs»</b> </fettest>
<Dettots><Summary> Vous avez un ensemble de répliques défini pour exécuter 3 pods.Vous avez supprimé l'une de ces 3 gousses.Que va-t-il se passer ensuite?Combien de pods y aura-t-il? </summary> <br> <b>
Il y aura encore 3 pods en cours de route parce que l'objectif de l'ensemble des répliques est de le garantir.Donc, si vous supprimez une ou plusieurs gousses, il exécutera des gousses supplémentaires, il y a donc toujours 3 pods.</b> </fettest>
<Dettots><summary> Comment vérifier quelle image de conteneur a été utilisée dans le cadre de la réplique du jeu appelé "Repli"? </summary> <br> <b>
`K Décrivez RS Repli |grep -i image`</b> </fettest>
<Dettots><summary> Comment vérifier combien de pods sont prêts dans le cadre d'un ensemble de répliques appelé "revi"? </summary> <br> <b>
`K Décrivez RS Repli |grep -i "status pods" `</b> </fettest>
<Dettots><summary> Comment supprimer un ensemble de répliques appelé "Rori"? </summary> <br> <b>
`k supprimer RS ​​Ruralo`</b> </fettest>
<Dettots><summary> Comment modifier un ensemble de répliques appelé "Rori" pour utiliser une image différente? </summary> <br> <b>
`K Hôpital K Edis Rori '</b> </fettest>
<Dettots><summary> Évoluer un ensemble de répliques appelé "Rori" pour exécuter 5 pods au lieu de 2 </summary> <br> <b>
`k échelle RS Rori --replicas = 5 '</b> </fettest>
<Dettots><summary> Échelle un ensemble de répliques appelé "Rori" pour exécuter 1 pod au lieu de 5 </summary> <br> <b>
`k échelle rs rori --replicas = 1`</b> </fettest>
### Dépannage des répliques
<Dettots><Summary> Correction de la définition du répliquant suivant
`` `YamlApversion: applications / v1genre: réplicageC'est le bordel:Nom: redisÉtiquettes:App: redisniveau: cacheSPEC:sélecteur:MatchLabels:niveau: cachemodèle:C'est le bordel:Étiquettes:Tier: CachySPEC:conteneurs:- Nom: redisImage: redis`` '</summary> <br> <b>
Le genre doit être répliques et non répliCacet :)
</b> </fettest>
<Dettots><Summary> Correction de la définition du répliquant suivant
`` `YamlApversion: applications / v1genre: répliqueC'est le bordel:Nom: redisÉtiquettes:App: redisniveau: cacheSPEC:sélecteur:MatchLabels:niveau: cachemodèle:C'est le bordel:Étiquettes:Tier: CachySPEC:conteneurs:- Nom: redisImage: redis`` '</summary> <br> <b>
Le sélecteur ne correspond pas à l'étiquette (Cache vs Cachy).Pour le résoudre, réparez Cachy pour que ce soit le cache à la place.
</b> </fettest>
## déploiements
<Dettots><summary> Comment répertorier tous les déploiements dans l'espace de noms actuel? </summary> <br> <b>
«k get déploie»
</b> </fettest>
<Dettots><summary> Comment vérifier quelle image un certain déploiement utilise? </summary> <br> <b>
`K Décrivez Deploy <Deploiment_name> |image grep`
</b> </fettest>
<Dettots><summary> Créez une définition / manifeste de fichiers d'un déploiement appelé "DEP", avec 3 répliques qui utilise l'image 'redis' </summary> <br> <b>
`K Créer Deploy Dep -o Yaml --image = redis --dry-run = client --replicas 3> Deployment.yaml`
</b> </fettest>
<Dettots><summary> supprimer le déploiement `depdep` </summary> <br> <b>
`k Supprimer Deployer Depdep`
</b> </fettest>
<Dettots><summary> Créez un déploiement appelé "Pluck" en utilisant l'image "redis" et assurez-vous qu'il exécute 5 répliques </summary> <br> <b>
`Kubectl Créer un déploiement Pluck --image = redis --replicas = 5`
</b> </fettest>
<Dettots><summary> Créez un déploiement avec les propriétés suivantes:
* appelé "Blufer"* Utilisation de l'image "Python"* Exécute 3 répliques* Tous les pods seront placés sur un nœud qui a l'étiquette "Blufer"</summary> <br> <b>
`Kubectl Créer le déploiement Blufer --image = python --replicas = 3 -o yaml --dry-run = client> Deployment.yaml`
Ajoutez la section suivante (`vi déploiement.yaml`):
`` 'SPEC:affinité:nodeaffinity:ObligatoireDurringSCheDuliningIgnoredUringExecution:NODEDELECTORTERMS:- Faire correspondre les expressions:- Clé: BluferOpérateur: existe`` '
`Kubectl appliquer -f déploiement.yaml`</b> </fettest>
### Dépannage des déploiements
<Dettots><Summary> Corrigez le manifeste de déploiement suivant
`` `YamlApversion: applications / v1genre: déploierC'est le bordel:créationtimestamp: nullÉtiquettes:App: DEPNom: DEPSPEC:répliques: 3sélecteur:MatchLabels:App: DEPstratégie: {}modèle:C'est le bordel:créationtimestamp: nullÉtiquettes:App: DEPSPEC:conteneurs:- Image: redisNom: redisressources: {}statut: {}`` '</summary> <br> <b>
Modifier `Kind: Deploy` en` aimne: déploiement '</b> </fettest>
<Dettots><Summary> Corrigez le manifeste de déploiement suivant
`` `YamlApversion: applications / v1genre: déploiementC'est le bordel:créationtimestamp: nullÉtiquettes:App: DEPNom: DEPSPEC:répliques: 3sélecteur:MatchLabels:APP: DEPDEPstratégie: {}modèle:C'est le bordel:créationtimestamp: nullÉtiquettes:App: DEPSPEC:conteneurs:- Image: redisNom: redisressources: {}statut: {}`` '</summary> <br> <b>
Le sélecteur ne correspond pas à l'étiquette (DEP vs Depdep).Pour le résoudre, corrigez depdep pour que ce soit le DEP à la place.</b> </fettest>
## Planificateur
<Dettots><summary> Comment planifier un pod sur un nœud appelé "Node1"? </summary> <br> <b>
`k exécuter quelque-pod --image = redix -o yaml --dry-run = client> pod.yaml`
`vi pod.yaml` et ajouter:
`` 'SPEC:nodename: node1`` '
`k appliquer -f pod.yaml`
Remarque: Si vous n'avez pas de nœud1 dans votre cluster, le pod sera coincé sur l'état "en attente".</b> </fettest>
### Affinité du nœud
<Dettots><summary> À l'aide de l'affinité du nœud, définissez un pod pour planifier un nœud où la clé est "région" et la valeur est "Asie" ou "EMEA" </summary> <br> <b>
Confiture
`` `Yamlaffinité:nodeaffinity:ObligatoireDurringSCheDuliningIgnoredUringExecution:NODEDELECTORTERMS:- Faire correspondre les expressions:- Clé: régionOpérateur: dansvaleurs:- Asie- emea`` '</b> </fettest>
<Dettots><Summary> En utilisant l'affinité du nœud, définissez un pod pour ne jamais planifier un nœud où la clé est "région" et la valeur est "Neverland" </summary> <br> <b>
Confiture
`` `Yamlaffinité:nodeaffinity:ObligatoireDurringSCheDuliningIgnoredUringExecution:NODEDELECTORTERMS:- Faire correspondre les expressions:- Clé: régionOpérateur: notinvaleurs:- Pays imaginaire`` '</b> </fettest>
## Étiquettes et sélecteurs
<Dettots><summary> Comment répertorier tous les pods avec l'étiquette "app = web"? </summary> <br> <b>
`k get po -l app = web`</b> </fettest>
<Dettots><summary> Comment répertorier tous les objets étiquetés comme "Env = staging"? </summary> <br> <b>
`K Get All -l Env = Staging`</b> </fettest>
<Dettots><summary> Comment répertorier tous les déploiements à partir de "Env = prod" et "type = web"? </summary> <br> <b>
`K Get Deploy -l Env = prod, type = web`</b> </fettest>
Sélecteur de nœud ###
<Dettots><summary> Appliquez l'étiquette "hw = max" sur l'un des nœuds de votre cluster </summary> <br> <b>
`Kubectl Label nœuds quelque no-node hw = max`
</b> </fettest>
<Dettots><summary> Créez et exécutez un pod appelé «Some-Pod» avec l'image `redis` et configurez-le pour utiliser le sélecteur` hw = max` </summary> <br> <b>
`` 'kubectl exécuter quelque-pod --image = redis --run = client -o yaml> pod.yaml
vous sous. Yaml
SPEC:Elector de nœuds:HW: Max
kubectl appliquer -f pod.yaml`` '
</b> </fettest>
<Dettots><summary> Expliquez pourquoi les sélecteurs de nœuds pourraient être limités </summary> <br> <b>
Supposons que vous souhaitez exécuter votre pod sur tous les nœuds avec avec «HW» réglé sur Max ou sur Min, au lieu de simplement Max.Ce n'est pas possible avec les Électeurs de nœuds qui sont assez simplifiés et c'est là que vous voudrez peut-être envisager «Node Affinity».</b> </fettest>
## Taints
<Dettots><summary> Vérifiez s'il y a des taints sur le nœud "maître" </summary> <br> <b>
`K ne décrit pas de maître |grep -i taints`</b> </fettest>
<Dettots><Summary> Créez une souillure sur l'un des nœuds de votre cluster avec la clé de "App" et la valeur de "Web" et l'effet de "Noschedule".Vérifiez qu'il a été appliqué </summary> <br> <b>
`k Taint Node minikube app = web: noschedule`
`K ne décrivez pas de minikube |grep -i taints`</b> </fettest>
<Dettots><summary> Vous avez appliqué une souillure avec <code> k Taint nœud minikube app = web: noschedule </code> sur le seul nœud de votre cluster, puis exécuté <code> kubectl exécuter certains pod --image = redis </ code>.Que se passera-t-il? </summary> <br> <b>
Le pod restera dans l'état "en attente" en raison du seul nœud du cluster ayant une souillure de "app = web".</b> </fettest>
<Dettots><summary> Vous avez appliqué une souillure avec <code> k Taint nœud minikube app = web: noschedule </code> sur le seul nœud de votre cluster, puis exécuté <code> kubectl exécuter certains pod --image = redis </ code> Mais le pod est en attente.Comment le réparer? </summary> <br> <b>
`Kubectl éditer po quelque pod` et ajouter ce qui suit
`` '- Effet: Noscheduleclé: applicationOpérateur: égalvaleur: web`` '
Sortir et enregistrer.Le pod devrait être en cours d'exécution maintenant.</b> </fettest>
<Dettots><summary> Supprimez une souillure existante de l'un des nœuds de votre cluster </summary> <br> <b>
`k Taint Node minikube app = web: noschedule -`</b> </fettest>
## limites de ressources
<Dettots><summary> Vérifiez s'il y a des limites sur l'une des pods de votre cluster </summary> <br> <b>
`kubectl décrivent po <pod_name> |grep -i limites`</b> </fettest>
<Dettots><summary> Exécutez une pod appelée "yay" avec l'image "Python" et la demande de ressources de la mémoire 64mi et 250m CPU </summary> <br> <b>
`kubectl run yay --image = python --dry-run = client -o yaml> pod.yaml`
Confiture
`` 'SPEC:conteneurs:- Image: PythonImagePullPolicy: toujoursNom: yayressources:Demandes:CPU: 250mMémoire: 64mi`` '
`kubectl appliquer -f pod.yaml`</b> </fettest>
<Dettots><summary> Exécutez une pod appelée "yay2" avec l'image "Python".Assurez-vous qu'il a une demande de ressources de la mémoire de 64mi et du processeur de 250 m et les limites sont de la mémoire de 128mi et du processeur de 500m </summary> <br> <b>
`kubectl run yay2 --image = python --run = client -o yaml> pod.yaml`
Confiture
`` 'SPEC:conteneurs:- Image: PythonImagePullPolicy: toujoursNom: Yay2ressources:limites:CPU: 500mMémoire: 128miDemandes:CPU: 250mMémoire: 64mi`` '
`kubectl appliquer -f pod.yaml`</b> </fettest>
## Surveillance
<Dettots><summary> Déployer les métriques-serveur </summary> <br> <b>
`kubectl appliquer -f https: // github.com / kubernetes-sigs / metrics-server / releases / le dernier / downlow / composants.yaml`</b> </fettest>
<Dettots><Summary> À l'aide de Metrics-Server, consultez ce qui suit:
* Les nœuds les plus performants dans le cluster* Pods les plus performants</summary> <br> <b>
* Nœuds supérieurs: «Kubectl Top nœuds»* Top Pods: «Kubectl Top Pods»
</b> </fettest>
## Planificateur
<Dettots><summary> Pouvez-vous déployer plusieurs planificateurs? </summary> <br> <b>
Oui c'est possible.Vous pouvez exécuter une autre pod avec une commande similaire à:
`` 'SPEC:conteneurs:- commande:- Kube-Scheduler- --Address = 127.0.0.1- --leader-élu = vrai- --Scheduler-Name = Some-personnSumed...`` '</b> </fettest>
<Dettots><summary> En supposant que vous avez plusieurs planificateurs, comment savoir quel planificateur a été utilisé pour un pod donné? </summary> <br> <b>
L'exécution de «Kubectl Get Events» Vous pouvez voir quel planificateur a été utilisé.</b> </fettest>
<Dettots><Summary> Vous souhaitez exécuter un nouveau pod et vous souhaitez qu'il soit planifié par un schduler personnalisé.Comment y parvenir? </summary> <br> <b>
Ajoutez ce qui suit à la spécification du pod:
`` 'SPEC:Schedulername: certains programmes de personnalité`` '</b> </fettest>