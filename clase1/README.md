# Tarea 1 - Configuración de Repositorio y Primer Desafío

# Parte 1: Configuración de Repositorio

## ✅ Estado del Ejercicio: COMPLETADO

### Estructura Implementada:

# Parte 2: Desafío Técnico con Docker - Apache HTTP Server

## Documentación Requerida

### Nombre de la Aplicación
**Apache HTTP Server (httpd)**

---

## Comandos Ejecutados

### 1. Comando docker run completo

docker run -d --name mi-apache -p 8081:80 httpd

### 2. Verificar containers en ejecución

docker ps

### 3. Ver logs del container

docker logs mi-apache

### 4. Limpieza

## Detener container
docker stop mi-apache

## Eliminar container
docker rm mi-apache

## Verificar que ya no existe
docker ps -a

### 5. Screenshots
- [Apache-browser](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/blob/main/clase1/screenshots/apache-browser.PNG)
- [docker-ps-after-cleanup](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/blob/main/clase1/screenshots/docker-ps-after-cleanup.PNG) 
- [docker-ps-apache](https://github.com/ccrrmmrr/curso-docker-kubernetes-tareas/blob/main/clase1/screenshots/docker-ps-apache.PNG)

## Explicación de Flags

-d → Ejecutar en segundo plano (detached mode)

--name mi-apache → Asignar nombre personalizado al container

-p 8081:80 → Mapear puerto 8081 del host al puerto 80 del container

httpd → Imagen oficial de Apache HTTP Server

