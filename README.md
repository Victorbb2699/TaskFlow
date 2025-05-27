# TaskFlow

TaskFlow es un sistema colaborativo estilo Trello donde los usuarios pueden:

* Crear **tableros (boards)**
* Crear y mover **tareas** entre columnas
* Asignar tareas a usuarios
* Recibir **notificaciones** y mantener **auditoría de cambios**

---

## 🧱 Arquitectura de Microservicios Propuesta

| Microservicio        | Funcionalidad Principal                       | Tecnología                |
| :------------------- | :-------------------------------------------- | :-------------------------- |
| `user-service`       | Gestión de usuarios, roles, autenticación JWT | Spring Boot + JWT           |
| `board-service`      | CRUD de tableros, listas/columnas             | Spring Boot                 |
| `task-service`       | CRUD de tareas, asignación, estados           | Spring Boot                 |
| `notification-service` | Envía notificaciones push/email en base a eventos | Spring Boot + Kafka         |
| `audit-service`      | Almacena un historial de todos los eventos de cambio | Spring Boot + Kafka         |
| `api-gateway`        | Entrada común (opcional)                      | Spring Cloud Gateway        |
| `config-server`      | Gestión de configuración externa (opcional)   | Spring Cloud Config         |

---

## 🔄 Eventos Kafka (Arquitectura Reactiva)

Cada vez que se crea o modifica una tarea, se publican eventos como:

* `TASK_CREATED`
* `TASK_UPDATED`
* `TASK_ASSIGNED`
* `TASK_MOVED`
* `TASK_COMPLETED`

**Productores:**

* `task-service`

**Consumidores:**

* `notification-service` (envía alertas)
* `audit-service` (guarda logs históricos)

---

## 🧪 TDD & Testing

Todos los servicios deben tener:

* **Unit tests** (JUnit 5, Mockito)
* **Integration tests** (Testcontainers para Kafka y BDs)
* **Acceptance tests** (Spring Boot Test + REST Assured)
* Pipeline local con Maven + GitHub Actions (si aplicable)

---

## ☁️ Despliegue en AWS (siguiente fase)

* Docker para cada servicio
* Docker Compose para entorno local
* Terraform o AWS CLI para ECS o EC2 (opcional)
* S3 para logs de auditoría o archivos adjuntos (opcional)

---

## 💡 Fases del Desarrollo

### 🔹 Fase 1: Core funcional en local

* Crear estructura del proyecto (multi-módulo o microservicios independientes)
* Desarrollar `user-service`, `board-service` y `task-service`
* Implementar eventos básicos con Kafka
* Tests unitarios e integración
* Documentar con Swagger

### 🔹 Fase 2: Servicios secundarios

* Agregar `notification-service` y `audit-service`
* Probar consumo de eventos
* Hacer tests de aceptación
* Refactorizar usando lambdas y streams donde sea posible

### 🔹 Fase 3: Despliegue y mejoras

* Docker Compose
* Subida a AWS (o render/Vercel temporal)
* Autenticación con JWT
* GitHub repo con instrucciones claras
