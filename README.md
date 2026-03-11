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

**1. Build the app image** (from repo root):

```bash
docker build -t debug-task-app:latest ./app
```

**2. Load the image into the cluster (kind):**

```bash
kind load docker-image debug-task-app:latest
```

**3. Deploy:**

```bash
kubectl apply -f k8s/manifest.yaml
kubectl get pods -l app=debug-task-app
```

Wait until pod is `READY 2/2`.

**4. Port-forward** (leave running in one terminal):

```bash
kubectl port-forward svc/debug-task-service 8080:8081
```

**5. Test** (in another terminal, from repo root so `anyfile.txt` / `anotherfile.txt` are found):

```bash
curl -X POST -F "file=@anyfile.txt" http://localhost:8080/upload
curl http://localhost:8080/uploads/anyfile.txt
curl -X POST -F "file=@anotherfile.txt" http://localhost:8080/upload
curl http://localhost:8080/uploads/anotherfile.txt
```

The manifest uses `image: debug-task-app:latest`; the same tag is used in step 1.
