# Helm Quick Reference - Ride Sharing Platform

## 🚀 Common Commands

### **Deploy to Development**
```bash
kubectl create namespace ride-sharing-dev  # First time only
helm upgrade --install ride-sharing . -f values-dev.yaml -f secrets-dev.yaml
```

### **Deploy to Production**
```bash
kubectl create namespace ride-sharing-prod  # First time only
helm upgrade --install ride-sharing . -f values-prod.yaml -f secrets-prod.yaml
```

### **Preview Changes (Dry Run)**
```bash
helm template ride-sharing . -f values-dev.yaml
# or
helm upgrade --install ride-sharing . -f values-dev.yaml --dry-run
```

### **Check Status**
```bash
helm list -n ride-sharing-dev
kubectl get all -n ride-sharing-dev
```

### **View Logs**
```bash
kubectl logs -f deployment/api-gateway -n ride-sharing-dev
```

### **Rollback**
```bash
helm history ride-sharing -n ride-sharing-dev
helm rollback ride-sharing -n ride-sharing-dev
```

### **Uninstall**
```bash
helm uninstall ride-sharing -n ride-sharing-dev
```

---

## 🎛️ Customization Examples

### **Scale a Service**
```bash
helm upgrade ride-sharing . -f values-dev.yaml --set apiGateway.replicas=3
```

### **Disable a Service**
```bash
helm upgrade ride-sharing . -f values-dev.yaml --set paymentService.enabled=false
```

### **Change Image Tag**
```bash
helm upgrade ride-sharing . -f values-dev.yaml --set apiGateway.image.tag=v1.2.3
```

### **Multiple Overrides**
```bash
helm upgrade ride-sharing . -f values-dev.yaml \
  --set apiGateway.replicas=3 \
  --set web.replicas=2 \
  --set jaeger.enabled=false
```

---

## 🔍 Debugging

### **Check Pod Status**
```bash
kubectl get pods -n ride-sharing-dev
kubectl describe pod <pod-name> -n ride-sharing-dev
```

### **View Generated YAML**
```bash
helm template ride-sharing . -f values-dev.yaml | less
```

### **Validate Chart**
```bash
helm lint .
```

---

## 📝 Before Deploying

1. ✅ Update secrets in `values-dev.yaml`:
   - MongoDB URI
   - Stripe keys
   - RabbitMQ credentials (if changed)

2. ✅ Ensure namespace exists:
   ```bash
   kubectl create namespace ride-sharing-dev
   ```

3. ✅ Verify chart is valid:
   ```bash
   helm lint .
   ```

4. ✅ Preview what will be deployed:
   ```bash
   helm template ride-sharing . -f values-dev.yaml
   ```

---

## 🎯 Development Workflow

1. **Make code changes** in ride-sharing-backend
2. **Build Docker images**:
   ```bash
   # From ride-sharing-backend
   make build  # or your build command
   ```
3. **Upgrade Helm release**:
   ```bash
   # From ride-sharing-platform
   helm upgrade ride-sharing . -f values-dev.yaml
   ```
4. **Watch pods restart**:
   ```bash
   kubectl get pods -n ride-sharing-dev -w
   ```

---

## 🔐 Secrets Management

**Current approach:** Separated configuration
- `values-dev.yaml` / `values-prod.yaml`: Non-sensitive config (Git safe)
- `secrets-dev.yaml` / `secrets-prod.yaml`: Sensitive data (Git ignored)

**⚠️ IMPORTANT:**
- Never commit actual secrets to Git
- Use environment variables in CI/CD
- Consider using Sealed Secrets or External Secrets Operator for production

---

## 📊 Monitoring

### **Jaeger (Tracing)**
```bash
kubectl port-forward svc/jaeger 16686:16686 -n ride-sharing-dev
# Open http://localhost:16686
```

### **RabbitMQ Management**
```bash
kubectl port-forward svc/rabbitmq 15672:15672 -n ride-sharing-dev
# Open http://localhost:15672
# Login: guest/guest
```

### **API Gateway**
```bash
kubectl port-forward svc/api-gateway 8081:8081 -n ride-sharing-dev
# Open http://localhost:8081
```

### **Web Frontend**
```bash
kubectl port-forward svc/web 3000:3000 -n ride-sharing-dev
# Open http://localhost:3000
```

---

## 🆘 Troubleshooting

### **Pods in CrashLoopBackOff**
```bash
kubectl logs <pod-name> -n ride-sharing-dev
kubectl describe pod <pod-name> -n ride-sharing-dev
```

### **ImagePullBackOff**
- Check `imagePullPolicy` in values file
- For local dev, should be `Never`
- Ensure images are built locally

### **Secrets not found**
- Verify secrets are created:
  ```bash
  kubectl get secrets -n ride-sharing-dev
  ```
- Check values in `values-dev.yaml`

### **Service not accessible**
- Check service type (LoadBalancer vs ClusterIP)
- Use port-forward for ClusterIP services
- Check if Minikube tunnel is running (for LoadBalancer)

---

## 📚 Resources

- [Helm Documentation](https://helm.sh/docs/)
- [Kubernetes Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- Project README: `README.md`
- Migration Guide: `MIGRATION.md`
