# Tarea 8: Ingress, Health Probes y Escalado Automático
---

## Kubernetes Two-Tier Application

Despliegue de una aplicación de 2 capas en Kubernetes con configuraciones de producción.

## 📋 Descripción del Proyecto

### Stack Desplegado
- **Frontend**: Nginx servidor web
- **Backend**: Nginx con endpoint `/api`
- **Orquestación**: Kubernetes con Minikube

### Conceptos Aplicados
- **Ingress**: Path-based routing para traffic management
- **Health Probes**: Liveness y readiness para high availability
- **HPA**: Horizontal Pod Autoscaler para auto-scaling
- **Resource Management**: Limits y requests para resource optimization

## Instrucciones de Despliegue

### Prerrequisitos
- Minikube instalado y configurado
- kubectl configurado
- Docker (opcional, dependiendo del driver)

### 1. Habilitar Addons Requeridos
```bash
minikube addons enable ingress
minikube addons enable metrics-server
```
### 2. Aplicar Manifest de Kubernetes
```bash
# Aplicar todos los recursos
kubectl apply -f k8s/

# O aplicar individualmente
kubectl apply -f k8s/frontend-deployment.yaml
kubectl apply -f k8s/frontend-service.yaml
kubectl apply -f k8s/backend-deployment.yaml
kubectl apply -f k8s/backend-service.yaml
kubectl apply -f k8s/ingress.yaml
kubectl apply -f k8s/hpa.yaml
```

### 3. Verificar Recursos Desplegados
```bash
# Ver estado general
kubectl get all

# Ver pods específicos
kubectl get pods -l app=frontend
kubectl get pods -l app=backend

# Esperar a que los pods estén ready
kubectl wait --for=condition=ready pod -l app=frontend --timeout=60s
kubectl wait --for=condition=ready pod -l app=backend --timeout=60s
```

### 4. Probar Ingress
```bash
# Obtener IP del Ingress
kubectl get ingress app-ingress

# Probar routing (requiere minikube tunnel)
minikube tunnel
# En otra terminal:
curl http://192.168.49.2/
curl http://192.168.49.2/api/
```

### 5. Probar HPA con Carga
```bash
# Monitorear HPA
kubectl get hpa backend-hpa --watch

# En otra terminal, generar carga
kubectl run load-generator --image=busybox:1.28 --rm -it --restart=Never -- \
  /bin/sh -c "while sleep 0.01; do wget -q -O- http://backend-service; done"

# Monitorear escalado de pods
kubectl get pods -l app=backend --watch
```

## Estructura de directorios
```bash
kubernetes-two-tier-app/
├── k8s/
│   ├── frontend-deployment.yaml
│   ├── frontend-service.yaml
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── ingress.yaml
│   └── hpa.yaml
├── screenshots/
│   ├── ingress-functioning.png
│   ├── health-probes-configured.png
│   ├── hpa-idle.png
│   ├── hpa-scaling.png
│   └── pods-scaled.png
└── README.md
```

## Comandos de Verificación
### Estado General del Cluster
```bash
kubectl get all
```

### Verificar Ingress
```bash
kubectl get ingress
kubectl describe ingress app-ingress
```

### Verificar HPA
```bash
kubectl get hpa
kubectl describe hpa backend-hpa
```

### Verificar Métricas
```bash
kubectl top pods
kubectl top nodes
```

### Verificar Health Probes
```bash
kubectl describe pod -l app=backend | grep -A 5 "Liveness\|Readiness"
kubectl logs -l app=backend --tail=10
```

### Verificar Services y Endpoints
```bash
kubectl get services
kubectl get endpoints
```
## Capturas de pantalla

- [Ingress funcionando](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/blob/main/clase8/screenshots/ingress-functioning.png)
- [Health probes configurados](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/blob/main/clase8/screenshots/health-probes-configured.png)
- [HPA en reposo](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/blob/main/clase8/screenshots/hpa-idle.png)
- [HPA escalando bajo carga](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/blob/main/clase8/screenshots/hpa-scaling.png)
- [Pods escalados](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/blob/main/clase8/screenshots/pods-scaled.png)


### 1. Ingress Funcionando

kubectl get ingress
# Debe mostrar ADDRESS asignado (ej: 192.168.49.2)

### 2. Health Probes Configurados

kubectl describe pod -l app=backend
# Buscar secciones Liveness y Readiness

### 3. HPA en Reposo

kubectl get hpa backend-hpa
# Debe mostrar TARGETS 0%/50% o similar

### 4. HPA Escalando Bajo Carga

kubectl get hpa backend-hpa --watch
# Durante carga debe mostrar TARGETS >50%

### 5. Pods Escalados

kubectl get pods -l app=backend
# Debe mostrar incremento de 2 a 4-5 pods
 
