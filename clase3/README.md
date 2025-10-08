# Docker Compose Practice - Aplicación Multi-Contenedor

**Curso:** Docker & Kubernetes - Clase 3
**Estudiante:** Carlos Roberto MArtinez Rivadeneira


Aplicación multi-contenedor con frontend, backend API y base de datos PostgreSQL, demostrando orquestación con Docker Compose.

## Stack

- **App:** Python con FastAPI
- **Base de datos:** PostgreSQL
- **Frontend:** Nginx con HTML/JavaScript
- **Admin:** pgAdmin

## Ejecución

1. Clonar:
\`\`\`bash
git clone https://github.com/ccrrmmrr/docker-compose-practice.git
cd docker-compose-practice
\`\`\`

2. Levantar servicios:
\`\`\`bash
docker compose up -d
\`\`\`

3. Acceder:
- Frontend: http://localhost:8082
- API: http://localhost:8001
- API Docs: http://localhost:8001/docs
- pgAdmin: http://localhost:8081

## Verificación

1. Servicios corriendo:
\`\`\`bash
docker compose ps
\`\`\`

2. Acceder a la web: http://localhost:8082

3. Verificar volumen persiste:
\`\`\`bash
docker compose down
docker compose up -d
docker volume ls  # debe seguir existiendo
\`\`\`

## Screenshots

### Servicios corriendo
![Docker Compose PS](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/tree/main/clase3/screenshots/docker_compose_ps.PNG)

### API funcionando
![API Docs](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/tree/main/clase3/screenshots/API_Docs.PNG)

### Aplicación web funcionando
![Frontend](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/tree/main/clase3/screenshots/Frontend.PNG)

### Volumen persistente
![Docker Volumes](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/tree/main/clase3/screenshots/docker_volumes.PNG)

### Red custom
![Docker Network](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/tree/main/clase3/screenshots/docker_network.PNG)

### Comunicación entre servicios
![Service Communication](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/tree/main/clase3/screenshots/comun_servicios.PNG)

## Conceptos Docker

- **Docker Compose con 4 servicios**: frontend, backend, database, pgadmin
- **Red custom**: \`app-network\` - todos los servicios conectados
- **Volumen**: \`postgres_data\` (persistencia de base de datos)
- **Variables de entorno**: Configuración mediante archivo .env
- **Health checks**: Verificación automática de dependencias
- **Comunicación entre servicios**: DNS interno y conectividad verificada

## Estructura del Proyecto
\`\`\`
docker-compose-practice/
├── docker-compose.yml
├── .env
├── README.md
├── backend/
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
├── frontend/
│   ├── Dockerfile
│   └── index.html
├── nginx/
│   └── nginx.conf
├── init.sql
└── screenshots/
    ├── docker-compose-ps.png
    ├── api-docs.png
    ├── frontend.png
    ├── docker-volumes.png
    ├── docker-network.png
    └── service-communication.png
\`\`\`

## Comandos de Verificación Adicionales

### Verificar red custom:
\`\`\`bash
docker network ls
\`\`\`

### Probar comunicación entre servicios:
\`\`\`bash
docker compose exec backend ping database
\`\`\`

### Verificar persistencia de datos:
\`\`\`bash
# Agregar dato de prueba
curl -X POST http://localhost:8001/tasks \\
  -H "Content-Type: application/json" \\
  -d '{"title": "Test persistencia", "description": "Dato de prueba"}'

# Reiniciar y verificar
docker compose down
docker compose up -d
curl http://localhost:8001/tasks
\`\`\`

---

**¡Aplicación multi-contenedor implementada exitosamente con Docker Compose!**
