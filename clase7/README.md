# Task Manager Kubernetes - Aplicación de 2 Capas

Aplicación completa de gestión de tareas con backend Node.js + PostgreSQL, desplegada en Kubernetes con arquitectura de 2 capas.

## 🏗️ Arquitectura del Sistema

```ascii
┌─────────────────────────────────────────────────────────────────┐
│                    KUBERNETES CLUSTER                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────┐        ┌─────────────────────────────────┐ │
│  │   API BACKEND   │        │        POSTGRESQL               │ │
│  │   (Deployment)  │        │       (StatefulSet)             │ │
│  │                 │        │                                 │ │
│  │  ┌─────────────┐│        │  ┌───────────────────────────┐  │ │
│  │  │   Pod-1     ││        │  │         Pod-0             │  │ │
│  │  │  Node.js    │┼─────┼─►│  │       PostgreSQL          │  │ │
│  │  │  Express    ││        │  │                           │  │ │
│  │  └─────────────┘│        │  └───────────────────────────┘  │ │
│  │                 │        │                 ▲               │ │
│  │  ┌─────────────┐│        │                 │               │ │
│  │  │   Pod-2     ││        │          ┌─────────────┐        │ │
│  │  │  Node.js    │┼─────┼─►│          │    PVC      │        │ │
│  │  │  Express    ││        │          │  (1Gi)      │        │ │
│  │  └─────────────┘│        │          └─────────────┘        │ │
│  │                 │        │                                 │ │
│  │         ▲       │        └─────────────────────────────────┘ │
│  │         │       │                                            │
│  │  ┌─────────────┐│                                            │
│  │  │  Service    ││                                            │
│  │  │ (NodePort)  ││                                            │
│  │  └─────────────┘│                                            │
│  │         ▲       │                                            │
│  └─────────┼───────┘                                            │
│            │                                                    │
│    ┌───────────────┐                                            │
│    │   ConfigMap   │   ┌─────────────┐   ┌──────────────────┐   │
│    │  api-config   │   │   ConfigMap │   │     Secret       │   │
│    │               │   │ postgres-   │   │  postgres-secret │   │
│    │ NODE_ENV      │   │   config    │   │                  │   │
│    │ API_PORT      │   │ database    │   │ username         │   │
│    │ CORS_ORIGIN   │   │ host        │   │ password         │   │
│    └───────────────┘   │ port        │   └──────────────────┘   │
│                        └─────────────┘                          │
└─────────────────────────────────────────────────────────────────┘


## 📋 Stack Tecnológico
- Backend: Node.js + Express
- Base de Datos: PostgreSQL 15
- Orquestación: Kubernetes (Minikube)
- Almacenamiento: Persistent Volume Claim (1Gi)
- Configuración: ConfigMaps + Secrets

## 🗂️ Estructura del Proyecto

```bash
task-manager-kubernetes/
├── k8s/
│   ├── configs.yaml              # ConfigMaps y Secrets
│   └── postgres-statefulset-fixed.yaml  # PostgreSQL StatefulSet
├── manifests/
│   ├── api-deployment-fixed.yaml # Backend API Deployment
│   └── services.yaml             # Kubernetes Services
├── docs/                         # Documentación
├── screenshots/                  # Capturas de pantalla
└── README.md                     # Este archivo
```

