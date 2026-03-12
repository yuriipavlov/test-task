# debug-task

## Structure

```
.
├── app/
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
├── k8s/
│   └── manifest.yaml
└── README.md
```

## Run

**1. Build the app image** (from repo root, use a version tag):

```bash
docker build -t debug-task-app:1.0.0 ./app
```

**2. Load the image into the cluster (kind, cluster name `alpacked`):**

```bash
kind load docker-image debug-task-app:1.0.0 --name alpacked
```

**3. Deploy** (resources go to namespace `debug-task`):

```bash
kubectl apply -f k8s/manifest.yaml
kubectl get pods -n debug-task
```

Wait until both deployments have pods `READY 1/1` (debug-task-app and debug-task-nginx).

**4. Port-forward** (leave running in one terminal):

```bash
kubectl port-forward svc/debug-task-service 8080:8081 -n debug-task
```

**5. Test** (in another terminal, from repo root so `anyfile.txt` / `anotherfile.txt` are found):

```bash
curl -X POST -F "file=@anyfile.txt" http://localhost:8080/upload
curl http://localhost:8080/uploads/anyfile.txt
curl -X POST -F "file=@anotherfile.txt" http://localhost:8080/upload
curl http://localhost:8080/uploads/anotherfile.txt
```

The manifest uses `image: debug-task-app:1.0.0`. Use the same tag when building; bump the version in both manifest and build when releasing.
