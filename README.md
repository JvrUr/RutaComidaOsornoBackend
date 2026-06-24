# OsornoGourmet API - Backend

API RESTful para la aplicación OsornoGourmet, una guía gastronómica de la ciudad de Osorno. Desarrollada en Kotlin con el framework Ktor.

## Tecnologías

- **Kotlin** + **Ktor** (Framework web)
- **Exposed** (ORM para base de datos)
- **PostgreSQL** en **Neon.tech** (Base de datos serverless)
- **HikariCP** (Pool de conexiones)
- **BCrypt** (Hashing de contraseñas)
- **JWT** (Autenticación con JSON Web Tokens)
- **Kotlinx Serialization** (Serialización JSON)

## Estructura del Proyecto

```
src/main/kotlin/osornogourmet/
├── Application.kt                 # Punto de entrada del servidor
├── DatabaseSeeder.kt              # Datos iniciales (22 locales de Osorno)
├── Routing.kt                     # Configuración de rutas
├── config/
│   ├── AuthConfig.kt              # Configuración de autenticación JWT
│   ├── DatabaseConfig.kt          # Conexión a Neon (PostgreSQL)
│   ├── ErrorConfig.kt             # Manejo global de errores
│   └── JwtConfig.kt               # Parámetros JWT
├── data/
│   ├── database/                  # Definición de tablas (Exposed)
│   │   ├── UsersTable.kt          # Tabla "usuarios"
│   │   ├── FoodPlacesTable.kt     # Tabla "lugares_comida"
│   │   └── RoutesTable.kt         # Tabla "rutas"
│   ├── dto/                       # Data Transfer Objects
│   ├── mapper/                    # Conversión Entity <-> Domain
│   └── repository/                # Implementación de repositorios
├── domain/
│   ├── model/                     # Modelos de dominio (FoodPlace, Route, User)
│   └── repository/                # Interfaces de repositorios
├── routes/
│   ├── AuthRoutes.kt              # POST /api/auth/login, /register
│   ├── FoodPlaceRoutes.kt         # CRUD /api/food-places
│   └── RouteRoutes.kt             # CRUD /api/routes
├── security/
│   ├── JwtTokenService.kt         # Generación de tokens JWT
│   └── PasswordHasher.kt          # Hashing con BCrypt
└── service/
    ├── AuthService.kt             # Lógica de autenticación
    ├── FoodPlaceService.kt        # Lógica de locales de comida
    └── RouteService.kt            # Lógica de rutas gastronómicas
```

## Endpoints

### Autenticación
| Método | Ruta                | Descripción          | Protegido |
|--------|---------------------|----------------------|-----------|
| POST   | `/api/auth/register`| Registrar usuario    | No        |
| POST   | `/api/auth/login`   | Iniciar sesión       | No        |

### Locales de Comida
| Método | Ruta                             | Descripción               | Protegido |
|--------|----------------------------------|---------------------------|-----------|
| GET    | `/api/food-places`               | Listar todos los locales  | No        |
| GET    | `/api/food-places/{id}`          | Obtener local por ID      | No        |
| GET    | `/api/food-places/category/{cat}`| Filtrar por categoría     | No        |
| POST   | `/api/food-places/batch`         | Obtener varios por IDs    | No        |
| POST   | `/api/food-places`               | Crear local               | Sí (JWT)  |
| PUT    | `/api/food-places/{id}`          | Actualizar local          | Sí (JWT)  |
| DELETE | `/api/food-places/{id}`          | Eliminar local            | Sí (JWT)  |

### Rutas Gastronómicas
| Método | Ruta               | Descripción           | Protegido |
|--------|--------------------|-----------------------|-----------|
| GET    | `/api/routes`      | Listar todas las rutas| No        |
| GET    | `/api/routes/{id}` | Obtener ruta por ID   | No        |
| POST   | `/api/routes`      | Crear ruta            | Sí (JWT)  |
| PUT    | `/api/routes/{id}` | Actualizar ruta       | Sí (JWT)  |
| DELETE | `/api/routes/{id}` | Eliminar ruta         | Sí (JWT)  |

## Base de Datos (Neon - PostgreSQL)

Las tablas están definidas en español:

- **`usuarios`** — correo, nombre, contrasena_hash
- **`lugares_comida`** — nombre, descripcion, categoria, direccion, latitud, longitud, calificacion, url_imagen, creado_por_usuario_id
- **`rutas`** — nombre, descripcion, ids_lugares_comida, creado_por_usuario_id, duracion_estimada, distancia_estimada

## Cómo Ejecutar

1. Abrir el proyecto en **IntelliJ IDEA** o **Android Studio**.
2. Sincronizar dependencias de Gradle.
3. Ejecutar `Application.kt`.
4. El servidor inicia en `http://localhost:8080`.

> La base de datos está alojada en Neon.tech, no se requiere instalar PostgreSQL localmente.

## Datos Precargados

El seeder incluye **22 locales reales de Osorno**, incluyendo: Bavaria, Club Alemán, Café del Centro, Wufehr, Bitte Brot, Pistacho Coffee, Fogón del Sur, Kumey Sushi, entre otros.
