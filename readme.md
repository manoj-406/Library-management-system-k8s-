
# Library Application

This project is a Library Application deployed on Azure Kubernetes Service (AKS) using multiple microservices. The architecture includes a frontend (React.js) and two backend services (FastAPI with PostgreSQL databases) for managing users and books.

![image](images/plan.png)

## Architecture

- **Frontend:** Library Webstore built using React.js.
- **Backend Services:**
  - **Users Service:** Manages user-related operations using FastAPI.
  - **Books Service:** Manages book-related operations using FastAPI.
- **Databases:**
  - **Users DB:** PostgreSQL database for users data.
  - **Books DB:** PostgreSQL database for books data.

## Technical Stack

- **Frontend:** React.js
- **Backend:** FastAPI (Python)
- **Databases:** PostgreSQL
- **Containerization:** Docker
- **Orchestration:** Kubernetes (AKS)

![image](images/k8s_realization.png)
  
## Prerequisites

- Azure Kubernetes Service (AKS) cluster
- kubectl configured to access your AKS cluster
- Docker images for each service pushed to a container registry (e.g., Docker Hub, Azure Container Registry)

## Kubernetes Manifest Files

### Users Database (`userdb.yml`)
- **ReplicaSet** for running the users database (PostgreSQL).
- **ClusterIP Service** to expose the database within the cluster.

### Users Service (`usersvc.yml`)
- **ReplicaSet** for running the Users Service (FastAPI).
- **NodePort Service** to expose the service on port 8000.

### Books Database (`booksdb.yml`)
- **ReplicaSet** for running the books database (PostgreSQL).
- **ClusterIP Service** to expose the database within the cluster.

### Books Service (`bookssvc.yml`)
- **ReplicaSet** for running the Books Service (FastAPI).
- **NodePort Service** to expose the service on port 8000.

### Library Webstore (`library.yml`)
- **ReplicaSet** for running the Library Webstore (React.js).
- **LoadBalancer Service** to expose the frontend on port 80.

## Environmental Variables

### Books Database
- `POSTGRES_USER: user`
- `POSTGRES_PASSWORD: password`
- `POSTGRES_DB: booksdb`

### Books Service
- `DATABASE_URL: "postgresql://user:password@booksdb-svc:5432/booksdb"`
- `SECRET_KEY: 'YtDEVWnL35aAIP-5yxeLjAZ49R920-mMNDfwPyWULu63HFsYzo0f-LO2InxC8eu428k'`

### Users Database
- `POSTGRES_USER: user`
- `POSTGRES_PASSWORD: password`
- `POSTGRES_DB: usersdb`

### Users Service
- `DATABASE_URL: "postgresql://user:password@userdb-svc:5432/usersdb"`
- `SECRET_KEY: 'YtDEVWnL35aAIP-5yxeLjAZ49R920-mMNDfwPyWULu63HFsYzo0f-LO2InxC8eu428k'`

### Library Webstore
- `REACT_APP_BACKEND_API_URL: http://<BACKEND_HOST>:8000/api/v1`
- `REACT_APP_BOOKS_API_URL: http://<BACKEND_HOST>:8000/api/v1/books`
- `REACT_APP_USERS_API_URL: http://<BACKEND_HOST>:8000/api/v1/users`

## How to Deploy

1. Clone the repository or obtain the Kubernetes manifest files.
2. Ensure your AKS cluster is up and running, and you have `kubectl` access to it.
3. Apply the manifests in the following order:

```bash
kubectl apply -f userdb.yml
kubectl apply -f usersvc.yml
kubectl apply -f booksdb.yml
kubectl apply -f bookssvc.yml
kubectl apply -f library.yml
```

4. Verify the pods are running:

```bash
kubectl get pods
```

5. Access the Library Webstore through the external IP assigned by the LoadBalancer service:

```bash
kubectl get svc library-svc
```

## Troubleshooting

- **Pods Not Running:** Check the pod logs for errors.
  
  ```bash
  kubectl logs <pod-name>
  ```

- **Service Not Accessible:** Ensure the LoadBalancer or NodePort services have the correct configuration.





## Azure CLI

* Creating AKS using command line
    * az group create --name <Resourcegroup_name> --location <region>
    * az aks create --resource-group <Resourcegroup_name> --name library --node-count 1 --node-vm-size "Standard_B2ms" --generate-ssh-keys
    * az aks get-credentials --resource-group <Resourcegroup_name> --name library
* To Delete
    * az group delete --resource-group <Resourcegroup_name> --yes --no-wait


