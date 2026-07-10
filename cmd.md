# Déploiement du projet Kubernetes

Vérifier que Kubernetes fonctionne :

```bash
kubectl get nodes
```

Résultat attendu :

```text
NAME                    STATUS
desktop-control-plane   Ready
```

---

## 2. Construire les images Docker

### Client

```bash
cd client
docker build -t client:v2
```

### Posts

```bash
cd ../posts
docker build -t posts
```

### Comments

```bash
cd ../comments
docker build -t comments
```

### Query

```bash
cd ../query
docker build -t query
```

### Moderation

```bash
cd ../moderation
docker build -t moderation
```

### Event Bus

```bash
cd ../event-bus
docker build -t event-bus
```

Retour à la racine :

```bash
cd ..
```

---

## 3. Déployer les ressources Kubernetes

```bash
kubectl apply -f infra/deployments
kubectl apply -f infra/services
kubectl apply -f infra/ingress
```

---

## 4. Installer le contrôleur Ingress (une seule fois)

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.2/deploy/static/provider/cloud/deploy.yaml
```

---

## 5. Attendre que le contrôleur démarre

```bash
kubectl get pods -n ingress-nginx
```

Attendre d'obtenir :

```text
ingress-nginx-controller-xxxxxxxx   1/1 Running
```

---

## 6. Faire un Port Forward

```bash
kubectl port-forward -n ingress-nginx service/ingress-nginx-controller 8080:80
```

---

## 7. Ouvrir l'application

```
http://localhost:8080
```

---

# Vérifications

## Vérifier les Pods

```bash
kubectl get pods
```

Résultat attendu :

```text
client-depl         Running
posts-depl          Running
comments-depl       Running
query-depl          Running
moderation-depl     Running
event-bus-depl      Running
```

---

## Vérifier les Services

```bash
kubectl get svc
```

---

## Vérifier l'Ingress

```bash
kubectl get ingress
```

---

# Commandes utiles

## Supprimer un Pod (Kubernetes le recrée automatiquement)

### Posts

```bash
kubectl delete pod -l app=posts
```

### Client

```bash
kubectl delete pod -l app=client
```

### Comments

```bash
kubectl delete pod -l app=comments
```

---

## Arrêter complètement le projet

```bash
kubectl delete -f infra/
```
