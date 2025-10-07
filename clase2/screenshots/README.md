# Clase 2 - Docker Multi-Stage Build con Python/FastAPI

## Aplicación

**Lenguaje:** Python 3.11  
**Framework:** FastAPI  
**Descripción:** API REST optimizada con Docker Multi-Stage Build

**Endpoints:**
- `GET /` - Página HTML con información de la aplicación
- `GET /api/status` - Estado del servidor y información técnica
- `GET /api/info` - Información del sistema y características  
- `GET /health` - Health check para monitoreo Docker

## Dockerfile

\`\`\`dockerfile
# Stage 1: Build stage
FROM python:3.11-alpine AS builder

WORKDIR /app

# Instalar herramientas de compilación
RUN apk add --no-cache \
    gcc \
    musl-dev \
    python3-dev \
    libffi-dev \
    openssl-dev

# Crear requirements.txt internamente
RUN echo "fastapi==0.104.1" > requirements.txt && \
    echo "uvicorn[standard]==0.24.0" >> requirements.txt && \
    echo "python-multipart==0.0.6" >> requirements.txt

# Instalar dependencias
RUN pip install --user --no-cache-dir --no-warn-script-location -r requirements.txt

# Stage 2: Runtime stage
FROM python:3.11-alpine AS runtime

WORKDIR /app

# Crear usuario non-root para seguridad
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Instalar curl para healthcheck
RUN apk add --no-cache curl

# Copiar dependencias del builder
COPY --from=builder /root/.local /home/appuser/.local

# Copiar aplicación
COPY main.py .

# Configurar PATH y variables de entorno
ENV PATH="/home/appuser/.local/bin:${PATH}"
ENV PYTHONUNBUFFERED=1
ENV PORT=8000

EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=10s --retries=3 \
    CMD curl -f http://localhost:8000/health || exit 1

USER appuser

CMD ["python", "main.py"]
\`\`\`

**Explicación:**

| Stage | Propósito |
|-------|-----------|
| Build | Compilar e instalar dependencias con todas las herramientas de build (gcc, python3-dev, etc.) |
| Production | Solo runtime de Python y dependencias necesarias, sin herramientas de compilación |

## Build

\`\`\`bash
docker build -t mi-app-python:2.0 .
\`\`\`

**Salida:**
\`\`\`
[+] Building 95.5s (14/14) FINISHED
 => [builder 3/5] RUN apk add --no-cache gcc musl-dev python3-dev libffi-dev openssl-dev
 => [builder 5/5] RUN pip install --user --no-cache-dir --no-warn-script-location -r requirements.txt
 => => writing image sha256:66da0aac7db0afffee796bbff49f0fc4b804d3cb326f7d4bbc330887ce325ab2
 => => naming to docker.io/library/mi-app-python:2.0-multi-stage
\`\`\`

**Tamaño final:** 87.1MB

## Testing

### Comandos para ejecutar el container
\`\`\`bash
# Ejecutar contenedor
docker run -d -p 8000:8000 --name python-app mi-app-python:2.0

# Verificar que está corriendo
docker ps

# Probar endpoints
curl http://localhost:8003/health
curl http://localhost:8003/api/status
curl http://localhost:8003/api/info

# Ver logs
docker logs python-app

# Verificar usuario non-root
docker exec python-app whoami
\`\`\`

### Screenshots
- [Docker Images](screenshots/docker_images.Png)
- [Container Running](screenshots/docker_ps.Png) 
- [Health Check Response](screenshots/EP_03.Png)
- [API Status Response](screenshots/EP_02.Png)
- [API Status Response](screenshots/EP_01.Png)
- [Phyton-app](screenshots/phyton-app.Png)
- [Application Logs](screenshots/docker_logs.Png)

## Docker Hub

**URL:** https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/tree/main?tab=readme-ov-file

**Comandos de publicación:**
\`\`\`bash
docker tag mi-app-python:2.0 tu-usuario/mi-app-python:2.0
docker push tu-usuario/mi-app-python:2.0
\`\`\`

## Optimizaciones

- **Multi-stage build**: Redujo imagen de 97.8MB a 87.1MB (11% reducción)
- **Usuario non-root**: Ejecuta como \`appuser\` en lugar de root para mayor seguridad
- **Alpine Linux**: Base image minimalista
- **Health checks**: Monitoreo automático con curl integrado
- **Sin herramientas build**: Eliminación de gcc, python3-dev en imagen final
- **.dockerignore**: Excluye archivos innecesarios del contexto de build

## Conclusiones

**Aprendí a optimizar imágenes Docker mediante:**
- Multi-stage build para separar entorno de build y producción
- Uso de usuarios non-root para mejorar seguridad
- Implementación de health checks para monitoreo automático
- Selección de base images minimalistas (Alpine Linux)
- Eliminación de herramientas innecesarias en runtime

**Dificultades superadas:**
- Problemas de networking en WSL2 (solucionado usando IP específica)
- Health checks fallidos por falta de curl en Alpine (solucionado instalando curl)
- Issues de permisos entre WSL y Docker (solucionado trabajando en filesystem Linux nativo)

**Comparación con Clase 1:**
- **Clase 1**: Build simple, imagen de 139MB, usuario root, sin health checks
- **Clase 2**: Multi-stage build, imagen de 87.1MB, usuario non-root, con health checks integrados
EOF
