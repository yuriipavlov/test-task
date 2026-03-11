# debug-task

## Run

ConfigMap from `app.py` (patched code is loaded without rebuilding the image):

```bash
kubectl create configmap app-code --from-file=app.py --dry-run=client -o yaml | kubectl apply -f -
```

Deploy:

```bash
kubectl apply -f manifest.yaml
kubectl get pods -l app=debug-task-app
```

Wait for `READY 2/2`, then:

```bash
kubectl port-forward svc/debug-task-service 8080:8081
```

In another terminal:

```bash
curl -X POST -F "file=@anyfile.txt" http://localhost:8080/upload
curl http://localhost:8080/uploads/anyfile.txt
curl -X POST -F "file=@anotherfile.txt" http://localhost:8080/upload
curl http://localhost:8080/uploads/anotherfile.txt
```
