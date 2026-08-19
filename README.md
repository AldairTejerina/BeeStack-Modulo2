# Agenda Telefónica API

Backend REST en JavaScript (Node.js + Express) para una aplicación móvil de agenda telefónica, documentado con OpenAPI 3.0.

## Requisitos

- Node.js 18+

## Instalación

```bash
npm install
npm run seed   # Carga datos de demo
npm start      # Inicia el servidor en http://localhost:3000
```

Modo desarrollo con recarga automática:

```bash
npm run dev
```

## Documentación OpenAPI

- **Swagger UI:** http://localhost:3000/api/docs
- **Spec YAML:** http://localhost:3000/api/docs/openapi.yaml

## Usuario demo

| Campo      | Valor              |
|------------|--------------------|
| Email      | demo@agenda.com    |
| Contraseña | demo1234           |

## Endpoints

### Autenticación (`/api/auth`)

| Método | Ruta                | Descripción              |
|--------|---------------------|--------------------------|
| POST   | `/register`         | Registrar usuario        |
| POST   | `/login`            | Iniciar sesión           |
| GET    | `/me`               | Obtener perfil           |
| PUT    | `/me`               | Actualizar perfil        |
| PUT    | `/me/password`      | Cambiar contraseña       |
| GET    | `/me/stats`         | Estadísticas             |

### Contactos (`/api/contacts`)

| Método | Ruta                | Descripción                        |
|--------|---------------------|------------------------------------|
| GET    | `/`                 | Listar (búsqueda, filtros, paginación) |
| POST   | `/`                 | Crear contacto                     |
| GET    | `/:id`              | Obtener contacto                   |
| PUT    | `/:id`              | Actualizar contacto                |
| DELETE | `/:id`              | Eliminar contacto                  |
| PATCH  | `/:id/favorite`     | Alternar favorito                  |
| GET    | `/favorites`        | Listar favoritos                   |
| GET    | `/recent`           | Contactos recientes                |
| GET    | `/index`            | Índice alfabético                  |
| GET    | `/sync?since=`      | Sincronización incremental         |

### Grupos (`/api/groups`)

| Método | Ruta                | Descripción              |
|--------|---------------------|--------------------------|
| GET    | `/`                 | Listar grupos            |
| POST   | `/`                 | Crear grupo              |
| GET    | `/:id`              | Obtener grupo            |
| PUT    | `/:id`              | Actualizar grupo         |
| DELETE | `/:id`              | Eliminar grupo           |
| GET    | `/:id/contacts`     | Contactos del grupo      |

### Health

| Método | Ruta           | Descripción    |
|--------|----------------|----------------|
| GET    | `/api/health`  | Health check   |

## Autenticación

Tras el login, incluir el token JWT en cada petición:

```
Authorization: Bearer <token>
```

## Ejemplo rápido

```bash
# Login
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"demo@agenda.com","password":"demo1234"}'

# Listar contactos
curl http://localhost:3000/api/contacts \
  -H "Authorization: Bearer <TOKEN>"

# Crear contacto
curl -X POST http://localhost:3000/api/contacts \
  -H "Authorization: Bearer <TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "Juan",
    "lastName": "Pérez",
    "phones": [{"type": "mobile", "number": "+54 11 1234-5678", "isPrimary": true}]
  }'
```

## Estructura del proyecto

```
src/
├── app.js              # Configuración Express + Swagger
├── server.js           # Punto de entrada
├── controllers/        # Controladores HTTP
├── services/           # Lógica de negocio
├── routes/             # Rutas REST
├── middleware/         # Auth, errores
├── validators/         # Validación de entrada
└── db/                 # SQLite + seed
openapi.yaml            # Especificación OpenAPI 3.0
data/                   # Base de datos JSON (generada)
```

## Variables de entorno

| Variable     | Default                          | Descripción        |
|--------------|----------------------------------|--------------------|
| PORT         | 3000                             | Puerto del servidor |
| JWT_SECRET   | agenda-dev-secret-change-in-production | Secreto JWT |
