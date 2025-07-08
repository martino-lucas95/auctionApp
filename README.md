# Sistema de Remates Online

Sistema backend para gestión de remates tanto asíncronos como en vivo vía streaming.

## Características Principales

### Tipos de Remates
- **Remates Asíncronos**: Similar a eBay, con período de tiempo definido
- **Remates en Vivo**: Streaming en tiempo real con rematador activo

### Roles de Usuario
- **Rematador**: Puede crear y gestionar remates
- **Comprador**: Puede participar en remates y realizar ofertas
- **Administrador**: Gestión completa del sistema

### Funcionalidades Core

#### Gestión de Remates
- Creación y programación de remates
- Gestión de lotes y sus características
- Control de estados del remate
- Manejo de imágenes y metadata
- Sistema de pujas y ofertas

#### Remates en Vivo
- Conexión en tiempo real vía WebSockets
- Panel de control para rematador
- Sistema de pujas sincrónicas
- Manejo de estados en tiempo real
- Recuperación ante fallos

#### Sistema de Pujas
- Validación de montos mínimos
- Control de incrementos
- Historial de ofertas
- Notificaciones en tiempo real

### Características Adicionales
- Sistema de notificaciones
- Watchlist/Favoritos
- Estadísticas para rematadores
- Búsqueda y filtrado avanzado

## Arquitectura Técnica

### Tecnologías Principales
- Spring Boot
- WebSocket (STOMP)
- JWT para autenticación
- Base de datos relacional
- Redis para caché y sesiones

### Componentes Principales

#### Remates Asíncronos
- API REST para operaciones CRUD
- Sistema de validación de pujas
- Manejo de estados y transiciones
- Procesamiento asíncrono de operaciones

#### Remates en Vivo
- Infraestructura WebSocket
- Sistema de mensajería en tiempo real
- Control de concurrencia
- Manejo de fallos y reconexiones

### Consideraciones de Seguridad
- Autenticación JWT
- Autorización basada en roles
- Rate limiting
- Validación de operaciones críticas
- Monitoreo de actividad sospechosa

## Estados del Sistema

### Estados de Remate
```
Asíncrono:
PROGRAMADO -> EN_CURSO -> FINALIZADO
           -> CANCELADO

En Vivo:
PROGRAMADO -> EN_ESPERA -> EN_VIVO -> FINALIZADO
                       -> PAUSADO  -^
           -> CANCELADO
```

### Estados de Lote
```
Asíncrono:
DISPONIBLE -> VENDIDO
          -> RETIRADO

En Vivo:
PENDIENTE -> EN_REMATE -> VENDIDO
                      -> PASADO
```

## Tareas Pendientes

### Fase 1: Estructura Base
- [ ] Configuración inicial del proyecto
- [ ] Implementación de entidades base
- [ ] Sistema de autenticación y autorización
- [ ] CRUD básico de remates y lotes

### Fase 2: Sistema de Pujas Asíncronas
- [ ] Implementación de pujas
- [ ] Validaciones de negocio
- [ ] Sistema de notificaciones
- [ ] Gestión de estados

### Fase 3: Remates en Vivo
- [ ] Configuración de WebSockets
- [ ] Implementación de panel de rematador
- [ ] Sistema de pujas en tiempo real
- [ ] Manejo de concurrencia y conflictos

### Fase 4: Características Adicionales
- [ ] Sistema de favoritos
- [ ] Estadísticas
- [ ] Búsqueda avanzada
- [ ] Gestión de archivos

## API Endpoints

### Autenticación
- POST /api/auth/login
- POST /api/auth/refresh

### Remates
- GET /api/remates
- POST /api/remates
- GET /api/remates/{id}
- PUT /api/remates/{id}
- DELETE /api/remates/{id}

### Lotes
- GET /api/remates/{remateId}/lotes
- POST /api/remates/{remateId}/lotes
- GET /api/lotes/{id}
- PUT /api/lotes/{id}
- DELETE /api/lotes/{id}

### Pujas
- POST /api/lotes/{loteId}/pujas
- GET /api/lotes/{loteId}/pujas
- GET /api/usuarios/{userId}/pujas

### WebSocket Endpoints
- /ws/remate/{remateId}
- /topic/remate/{remateId}
- /topic/lote/{loteId}
- /user/queue/notifications

## Mensajes WebSocket

### Tipos de Mensajes
```json
// Puja en vivo
{
  "type": "BID",
  "loteId": "123",
  "userId": "456",
  "amount": 1000,
  "timestamp": "2024-03-21T15:30:00.123Z",
  "sequenceNumber": 45
}

// Control de rematador
{
  "type": "AUCTIONEER_ACTION",
  "action": "HAMMER_STRIKE",
  "loteId": "123",
  "timestamp": "2024-03-21T15:30:10.000Z"
}
```

## Consideraciones de Escalabilidad
- Manejo de conexiones WebSocket concurrentes
- Caché distribuida con Redis
- Rate limiting y protección contra DOS
- Monitoreo y logging
- Recuperación ante fallos 