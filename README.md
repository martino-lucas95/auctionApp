# Auction App

Sistema de remates/subastas desarrollado en Spring Boot que permite la gestión de remates tanto asíncronos como en vivo.

## Características Principales

- Remates asíncronos (tipo eBay)
- Remates en vivo con WebSockets
- Sistema de pujas en tiempo real
- Gestión de usuarios y roles
- Procesamiento de pagos
- Reportes y análisis
- Panel de control para rematadores

## Planificación del Proyecto

### Sprint 1: Fundamentos y Autenticación
**Story Points: 21**
- Configuración inicial del proyecto Spring Boot
- Implementación de entidades base (Usuario, Remate, Lote, Puja)
- Sistema de autenticación con JWT y roles (REMATADOR, COMPRADOR, ADMIN)

### Sprint 2: Remates Asíncronos
**Story Points: 34**
- CRUD completo de remates con validaciones
- CRUD de lotes con manejo de imágenes
- Sistema de pujas asíncronas con validaciones
- Notificaciones por email

### Sprint 3: Remates en Vivo - Parte 1
**Story Points: 29**
- Configuración de WebSockets con STOMP
- Sistema de mensajería en tiempo real
- Sistema de recuperación ante fallos
- Manejo de estados del remate

### Sprint 4: Remates en Vivo - Parte 2
**Story Points: 29**
- Panel de control del rematador
- Sistema de pujas en tiempo real
- Interfaz de usuario para remates en vivo
- Manejo de concurrencia y conflictos

### Sprint 5: Pagos y Reportes
**Story Points: 34**
- Sistema de pagos y facturación
- Sistema de reportes y análisis
- Características avanzadas:
  - Sistema de recomendaciones
  - Optimizaciones de rendimiento
  - Características sociales

### Sprint 6: Seguridad y Despliegue
**Story Points: 34**
- Mejoras de seguridad y auditoría
- Pruebas exhaustivas y calidad
- Despliegue y monitoreo
- Documentación completa

## Arquitectura Técnica

### Backend
- Spring Boot
- Spring Security + JWT
- Spring Data JPA
- WebSocket + STOMP
- PostgreSQL

### Frontend
- React.js
- Material-UI
- STOMP.js para WebSocket
- Redux para estado

### Infraestructura
- Docker + Kubernetes
- AWS/GCP para hosting
- CI/CD con GitHub Actions
- Monitoreo con ELK Stack

## Estados del Sistema

### Estados de Remate
1. PENDIENTE
2. EN_VIVO
3. PAUSADO
4. FINALIZADO
5. CANCELADO

### Estados de Lote
1. PENDIENTE
2. EN_SUBASTA
3. VENDIDO
4. NO_VENDIDO
5. RETIRADO

### Estados de Puja
1. PENDIENTE
2. ACEPTADA
3. SUPERADA
4. RECHAZADA
5. GANADORA

## API Endpoints

### Autenticación
- POST /api/auth/login
- POST /api/auth/register
- POST /api/auth/refresh-token

### Remates
- GET /api/remates
- POST /api/remates
- GET /api/remates/{id}
- PUT /api/remates/{id}
- DELETE /api/remates/{id}

### Lotes
- GET /api/lotes
- POST /api/lotes
- GET /api/lotes/{id}
- PUT /api/lotes/{id}
- DELETE /api/lotes/{id}

### Pujas
- GET /api/pujas
- POST /api/pujas
- GET /api/pujas/{id}
- PUT /api/pujas/{id}

### WebSocket
- /ws/remate/{remateId}
- /topic/remate/{remateId}
- /app/puja
- /app/control

## Consideraciones de Escalabilidad

- Caché distribuida con Redis
- Base de datos particionada
- Balanceo de carga
- Microservicios para componentes críticos
- CDN para imágenes y assets

## Seguridad

- Autenticación JWT
- 2FA para acciones críticas
- Rate limiting
- Validación de inputs
- Auditoría de acciones
- Encriptación de datos sensibles

## Estructura del Proyecto

```
src/
├── main/
│   ├── java/
│   │   └── com/
│   │       └── mkstudios/
│   │           └── auction_app/
│   │               ├── AuctionAppApplication.java
│   │               ├── config/                 # Configuraciones de la aplicación
│   │               │   ├── security/           # Configuración de seguridad
│   │               │   ├── websocket/          # Configuración de WebSocket
│   │               │   └── cache/             # Configuración de caché
│   │               │
│   │               ├── domain/                # Capa de dominio
│   │               │   ├── model/             # Entidades de dominio
│   │               │   ├── repository/        # Interfaces de repositorio
│   │               │   ├── service/           # Servicios de dominio
│   │               │   └── exception/         # Excepciones de dominio
│   │               │
│   │               ├── infrastructure/        # Capa de infraestructura
│   │               │   ├── persistence/       # Implementaciones JPA
│   │               │   ├── security/          # Implementaciones de seguridad
│   │               │   ├── messaging/         # Servicios de mensajería
│   │               │   └── storage/           # Servicios de almacenamiento
│   │               │
│   │               ├── application/           # Capa de aplicación
│   │               │   ├── dto/               # Objetos de transferencia
│   │               │   ├── mapper/            # Mappers DTO-Entidad
│   │               │   ├── service/           # Servicios de aplicación
│   │               │   └── validator/         # Validadores
│   │               │
│   │               ├── presentation/          # Capa de presentación
│   │               │   ├── controller/        # Controladores REST
│   │               │   ├── websocket/         # Controladores WebSocket
│   │               │   └── handler/           # Manejadores de eventos
│   │               │
│   │               └── common/                # Componentes compartidos
│   │                   ├── annotation/        # Anotaciones personalizadas
│   │                   ├── util/              # Utilidades
│   │                   ├── constant/          # Constantes
│   │                   └── logging/           # Configuración de logs
│   │
│   └── resources/
│       ├── application.properties            # Propiedades principales
│       ├── application-dev.properties        # Propiedades de desarrollo
│       ├── application-prod.properties       # Propiedades de producción
│       ├── db/
│       │   └── migration/                    # Migraciones Flyway
│       ├── static/                           # Recursos estáticos
│       └── templates/                        # Plantillas (si se usan)
│
└── test/
    └── java/
        └── com/
            └── mkstudios/
                └── auction_app/
                    ├── unit/                  # Tests unitarios
                    │   ├── domain/
                    │   ├── application/
                    │   └── infrastructure/
                    │
                    ├── integration/           # Tests de integración
                    │   ├── controller/
                    │   ├── repository/
                    │   └── service/
                    │
                    └── e2e/                   # Tests end-to-end
```

### Descripción de las Capas

#### 1. Domain (Capa de Dominio)
- Contiene la lógica de negocio core
- Entidades, agregados y value objects
- Interfaces de repositorio
- Servicios de dominio
- Eventos de dominio

#### 2. Infrastructure (Capa de Infraestructura)
- Implementaciones técnicas
- Adaptadores de persistencia
- Implementaciones de seguridad
- Servicios externos
- Configuraciones técnicas

#### 3. Application (Capa de Aplicación)
- Casos de uso
- DTOs y mappers
- Orquestación de servicios
- Validaciones
- Manejo de transacciones

#### 4. Presentation (Capa de Presentación)
- Controllers REST
- Controladores WebSocket
- Manejo de excepciones
- Validación de entrada
- Transformación de respuestas

#### 5. Common (Componentes Compartidos)
- Utilidades
- Constantes
- Configuraciones
- Aspectos transversales

### Patrones Implementados

- **Clean Architecture**: Separación clara de responsabilidades
- **DDD (Domain-Driven Design)**: Enfoque en el dominio del negocio
- **CQRS**: Separación de comandos y consultas
- **Repository Pattern**: Abstracción de la persistencia
- **DTO Pattern**: Objetos de transferencia de datos
- **Factory Pattern**: Creación de objetos complejos
- **Strategy Pattern**: Comportamientos intercambiables
- **Observer Pattern**: Para eventos y WebSockets

## Instalación y Configuración

```bash
# Clonar el repositorio
git clone https://github.com/tu-usuario/auction-app.git

# Instalar dependencias
mvn install

# Configurar variables de entorno
cp .env.example .env

# Ejecutar la aplicación
mvn spring-boot:run
```

## Contribución

1. Fork el proyecto
2. Crea tu Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push al Branch (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## Licencia

Este proyecto está licenciado bajo la Licencia MIT - ver el archivo LICENSE.md para más detalles. 