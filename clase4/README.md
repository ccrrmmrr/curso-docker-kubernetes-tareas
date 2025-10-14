# 🛍️ E-commerce Microservicios

## 🎯 1. Descripción del Proyecto
Una aplicación de e-commerce básica construida con **arquitectura de microservicios** usando Docker, Redis, PostgreSQL y Nginx. Este proyecto demuestra patrones modernos de desarrollo y despliegue de aplicaciones distribuidas.

## 🛠️ Tecnologías Utilizadas

### **Containerización & Orquestación**
- 🐳 **Docker** - Containerization platform
- 🐳 **Docker Compose** - Multi-container orchestration
- 🌐 **Custom Docker Network** - Comunicación entre servicios

### **Backend & Microservicios**
- ⚡ **Node.js** - Runtime de JavaScript
- 🚀 **Express.js** - Framework web para APIs
- 🔴 **Redis** - Cache en memoria para productos
- 🐘 **PostgreSQL** - Base de datos relacional
- 🌐 **Nginx** - API Gateway y reverse proxy

### **Frontend**
- 📄 **HTML5** - Estructura semántica
- 🎨 **CSS3** - Estilos y diseño responsive
- ⚡ **JavaScript** - Lógica del cliente

### **Infraestructura**
- 🔧 **Health Checks** - Monitoreo de servicios
- 💾 **Named Volumes** - Persistencia de datos
- 🔄 **DNS Automático** - Service discovery

## 🏗️ 2. Arquitectura
- **Arquitectura de Microservicios**: Servicios independientes y especializados
- **API REST**: Endpoints para gestión de productos y carritos
- **Redis**: Cache en memoria para optimización de rendimiento
- **PostgreSQL**: Base de datos relacional para persistencia
- **Nginx**: API Gateway para enrutamiento y load balancing
- **Docker**: Containerización completa de todos los servicios
- **Docker Compose**: Orquestación y despliegue multi-contenedor

### 📜 Diagrama General

```


┌──────────────────────────────────────────────────────────────────────────────┐
│                              🌐 CLIENTE WEB                                  │
│                           (Navegador / Browser)                              │
│                       http://localhost:8080                                  │
└───────────────────────────────────┬──────────────────────────────────────────┘
                                    │
                                    ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│                              🚪 API GATEWAY                                  │
│                            Nginx (puerto :8080)                              │
│                      contenedor: mi-microservicios-gateway-1                 │
├────────────────────┬────────────────────┬────────────────────┬───────────────┤
│ /api/products      │ /api/cart          │ /                  │ /gateway/health│
│ → products-service:3001 │ → cart-service:3002 │ → frontend:80 │ → Health Check │
└────────────────────┴────────────────────┴────────────────────┴───────────────┘
                                    │
                    ┌───────────────┼────────────────┐
                    │               │                │
          ┌─────────▼─────────┐ ┌───▼────────┐ ┌────▼────────────┐
          │  📦 PRODUCTS       │ │  🛒 CART   │ │  🎨 FRONTEND     │
          │     SERVICE        │ │  SERVICE  │ │  HTML + CSS + JS │
          │ Node.js + Express  │ │ Node.js + Express │  (static)  │
          │ (:3101 → 3001)     │ │ (:3102 → 3002)   │ (:3100 → 80)│
          │ mi-microservicios- │ │ mi-microservicios-│ mi-microservicios-│
          │ products-service-1 │ │ cart-service-1    │ frontend-1        │
          └─────────┬─────────┘ └──────┬────────────┘ └───────────────┘
                    │                 │
        ┌───────────┼───────────┐     │
        │           │           │     │
┌───────▼──────┐ ┌──▼────────┐ ┌──────▼─────────┐
│ 🔴 REDIS     │ │ 🐘 POSTGRES│ │ 🐘 POSTGRES    │
│ CACHE        │ │ (products) │ │ (cart_items)  │
│ (:6380 → 6379)│ │  table     │ │  table        │
│ mi-microservicios-│ mi-microservicios-│ mi-microservicios-│
│ redis-1      │ │ postgres-1 │ │ postgres-1    │
└──────────────┘ └────────────┘ └───────────────┘




                           ┌────────────────────────────┐
                           │      🐳 DOCKER NETWORK     │
                           │     microservices-net      │
                           │         (bridge)           │
                           │     DNS Automático         │
                           │                            │
                           │  gateway:80                │
                           │  products-service:3001     │
                           │  cart-service:3002         │
                           │  frontend:80               │
                           │  redis:6379                │
                           │  postgres:5432             │
                           └────────────────────────────┘

```

## 📊 3. Servicios

| Servicio | Tecnología | Puerto | Descripción |
|----------|------------|--------|-------------|
| 🚪 API Gateway | Nginx | `8080` | Enrutamiento y load balancing |
| 📦 Products Service | Node.js + Express | `3101` | Gestión de productos con cache Redis |
| 🛒 Cart Service | Node.js + Express | `3102` | Gestión de carritos de compra |
| 🎨 Frontend | HTML + CSS + JS | `3100` | Interfaz de usuario web |
| 🔴 Redis Cache | Redis | `6380` | Cache de productos (5min TTL) |
| 🐘 PostgreSQL | PostgreSQL | `5433` | Base de datos persistente |



## 🚀 4. Instrucciones de Uso

```bash
# Clonar repositorio
git clone https://github.com/ccrrmmrr/mi-microservicios

# Levantar servicios
docker compose up -d

# Verificar estado
docker compose ps

# Ver logs
docker compose logs -f

# Acceder a la aplicación
http://localhost:8080
```


## 📡 5. API Endpoints

| Método HTTP | Ruta | Descripción | Ejemplo Request/Response |
|-------------|------|-------------|--------------------------|
| **GET** | `/gateway/health` | Health check del API Gateway | **Response:**<br>```json<br>{"status": "healthy", "service": "gateway"}<br>``` |
| **GET** | `/api/products` | Listar productos (con cache Redis) | **Response:**<br>```json<br>{"source": "cache", "data": [{"id": 1, "name": "Laptop", "price": "999.99"}]}<br>``` |
| **GET** | `/api/products/:id` | Obtener producto por ID | **Request:**<br>`GET /api/products/1`<br>**Response:**<br>```json<br>{"id": 1, "name": "Laptop", "price": "999.99"}<br>``` |
| **POST** | `/api/products` | Crear producto (invalida cache) | **Request:**<br>```json<br>{"name": "Nuevo", "price": 199.99, "stock": 50}<br>``` |
| **GET** | `/api/cart` | Obtener carrito | **Request:**<br>`GET /api/cart?cartId=abc123`<br>**Response:**<br>```json<br>{"cartId": "abc123", "items": [], "total": 0}<br>``` |
| **POST** | `/api/cart/add` | Agregar al carrito | **Request:**<br>```json<br>{"cartId": "abc123", "productId": 1, "quantity": 2}<br>``` |
| **DELETE** | `/api/cart/remove` | Eliminar del carrito | **Request:**<br>```json<br>{"cartId": "abc123", "productId": 1}<br>``` |
| **GET** | `/health` | Health check servicios | **Response:**<br>```json<br>{"status": "healthy", "service": "products-service"}<br>``` |





## 🧪 6. Testing Commands - E-commerce Microservicios

### 1. 🎯 Cache Hit/Miss Testing

### First Request (Cache MISS - From Database)
```bash
curl http://localhost:8080/api/products
```
### Second Request (Cache HIT - From Redis)
```bash
curl http://localhost:8080/api/products
```
### 2. 🗑️ Cache Invalidation Testing

### Create New Product (Invalidates Cache)
```bash
curl -X POST http://localhost:8080/api/products \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Nuevo Producto Test",
    "price": 299.99,
    "description": "Producto de prueba para cache invalidation",
    "stock": 15
  }'
```
### Verify Cache Was Invalidated
```bash
curl http://localhost:8080/api/products
```
### 3. 💾 Data Persistence Testing

### Add Item to Cart
```bash
curl http://localhost:8080/api/products

curl -X POST http://localhost:8080/api/cart/add \
  -H "Content-Type: application/json" \
  -d '{
    "productId": 1,
    "quantity": 2
  }'
```

### Stop and Restart Services
```bash
Stop all services
docker-compose down

Start services again
docker-compose up -d

Wait for services to be healthy
sleep 30
```
### Verify Data Persistence
```bash
# Get the cartId from the previous response and use it here
curl "http://localhost:8080/api/cart?cartId=YOUR_CART_ID_HERE"

# Also verify products are still there
curl http://localhost:8080/api/products
```

### 🚪 4. Gateway Routing Testing

### Gateway Health Check
```bash
curl http://localhost:8080/gateway/health
```

### Expected Response:
```bash
{"status": "healthy", "service": "gateway"}
```

### Services Health Checks via Gateway
```bash
Products Service Health
curl http://localhost:3101/health

Cart Service Health  
curl http://localhost:3102/health
```

### Frontend Routing
```bash
curl http://localhost:8080/
```

### API Routes via Gateway
```bash
Products API through gateway
curl http://localhost:8080/api/products

Cart API through gateway
curl http://localhost:8080/api/cart
```

