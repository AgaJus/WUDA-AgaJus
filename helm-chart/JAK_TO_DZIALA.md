# Działanie Helm Charta

## Co zrobiono

Kompletny Helm chart, który wdraża aplikację na klastrze Kubernetes. Chart składa się z:

### 1. **PostgreSQL** 
- **Deployment** (`postgres-deployment.yaml`) - uruchamianie kontenera z PostgreSQL
- **Service** (`postgres-service.yaml`) - typu ClusterIP (umożliwienie komunikacji wewnątrz klastra)

### 2. **Backend** 
- **Deployment** (`backend-deployment.yaml`) - uruchamianie kontenera z backendem
- **Service** (`backend-service.yaml`) - typu ClusterIP (umożliwienie komunikacji z frontendem)

### 3. **Frontend** 
- **Deployment** (`frontend-deployment.yaml`) - uruchamienie kontenera z frontendem
- **Service** (`frontend-service.yaml`) - typu NodePort (umożliwienie dostępu z przeglądarki)

## Jak działa komunikacja?

```
Przeglądarka (użytkownik)
    |
    |
    http://<minikube-ip>:30080  (NodePort)
    |
    |
Frontend Service (NodePort, port 30080)
    |
    |
Frontend Pod (port 8000)
    |
    |
    Komunikacja wewnątrz klastra
    |
    |
Backend Service (ClusterIP)
    |
    |
Backend Pod (port 8080)
    |
    |
    Komunikacja wewnątrz klastra
    |
    |
PostgreSQL Service (ClusterIP)
    |
    |
PostgreSQL Pod (port 5432)
```

## Inne

W naszym przypadku mamy:
- Frontend dostępny na porcie **30080** na każdym węźle
- Jeśli używasz Minikube: `http://<minikube-ip>:30080` (Justyna zobacz sobie czy ci to odpowiada czy chcesz to na Kind, bo ja robiłam na Minikube)
- PostgreSQL i Backend używają ClusterIP, bo komunikują się tylko wewnątrz klastra
- Frontend używa NodePort, bo musi być dostępny z przeglądarki

## Jak Helm używa values.yaml

Wszystkie pliki w katalogu `templates/` używają składni Helm:
- `{{ .Values.postgres.image }}` - pobiera wartość z `values.yaml`
- `| default "postgres:latest"` - jeśli wartość nie jest ustawiona, używa domyślnej


## Kolejność wdrażania (jak to działa)

1. Najpierw tworzone są Services (mogą czekać na Pods)
2. Potem tworzone są Deployments (które tworzą Pods)
3. Pods łączą się z Services przez selektory (labels)