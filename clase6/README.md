# Mi Aplicación Web en Kubernetes

Aplicación web desplegada en cluster Kubernetes demostrando conceptos fundamentales de orquestación de contenedores.

## Stack Tecnológico

- **Aplicación:** Nginx Alpine con HTML personalizado
- **Orquestación:** Kubernetes (minikube)
- **Runtime:** Docker
- **Réplicas:** 3 pods
- **Service:** NodePort para exposición

## Prerrequisitos

- Minikube instalado y configurado
- kubectl configurado
- Docker Desktop o Docker Engine

## Cómo Ejecutar

### 1. Ejecución 

```bash
   git clone https://github.com/ccrrmmrr/mi-app-kubernetes.git
   cd mi-app-kubernetes
```

### 2. Desplegar:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### 3. Acceder:

```bash
URL: http://:30200
O usar: minikube service webapp-service
```

### 4. Cómo Probar

## Verificación

1. Ver recursos:
```bash
   kubectl get all
2. Acceder a la web: http://:30200

3. Escalar:

   kubectl scale deployment webapp-deployment --replicas=5
   kubectl get pods
```

## Capturas de Pantalla

## Screenshots

### Recursos desplegados
-[kubectl get all](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/tree/main/clase6/screenshots/resources.png)

### Aplicación funcionando
-[webapp](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/tree/main/clase6/screenshots/aplicacion.png)

### Escalado a 3 réplicas
-[scaling](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/tree/main/clase6/screenshots/scaling.png)
