# Log Generator Helm Chart

This Helm chart deploys a **log generator** on Kubernetes using either a **Deployment** or a **CronJob**. It also includes an **ArgoCD Application** manifest for GitOps-based deployment.

## 📁 Folder Structure

```
.
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── deployment.yaml
│   ├── cronjob.yaml
│   ├── configmap.yaml
├── log-generator-argo.yaml
```

## ⚙️ How It Works

### 1. **ConfigMap**

Stores the `log-generator.sh` script that creates logs inside a pod.

### 2. **Deployment**

Runs a pod continuously generating logs at a defined interval.

### 3. **CronJob**

Runs the log generator script on a schedule.

### 4. **ArgoCD Application**

Used to deploy this Helm chart from GitHub in a GitOps workflow.

---

## 🔧 Configuration (values.yaml)

You can modify log size and interval:

```yaml
log:
  sizeKB: "100"         # Log file size
  intervalSeconds: "300" # Log creation interval

image:
  repository: busybox
  tag: latest
```

---

## 🚀 Installing the Chart

### **Using Helm Locally**

```
helm install log-generator .
```

### **Upgrade**

```
helm upgrade log-generator .
```

### **Uninstall**

```
helm uninstall log-generator
```

---

## 🚀 Deploy Using ArgoCD

Apply the ArgoCD Application:

```
kubectl apply -f log-generator-argo.yaml
```

ArgoCD will automatically sync changes from the GitHub repo.

---

## 📜 Script Details (`log-generator.sh`)

* Converts KB → bytes
* Generates a log file of exact size
* Appends timestamp
* Sleeps for given interval

---

## 📤 Logs Location

Logs are stored inside the pod at:

```
/logs/generated.log
```

View logs:

```
kubectl exec -it <pod-name> -- cat /logs/generated.log
```

---

## 🙋 Troubleshooting

### Pod not generating logs?

* Check script permissions
* Ensure ConfigMap mounted correctly
* Run inside pod:

```
sh /scripts/log-generator.sh
```

### ArgoCD not syncing?

* Ensure repo URL is correct
* Check ArgoCD Application logs

---

## 📘 License

MIT
