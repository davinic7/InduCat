# Portal del Parque Industrial de Catamarca

Sistema integrado para la gestión del Parque Industrial de Catamarca, que incluye módulos de autenticación, administración, gestión de empresas y observatorio público.

## 🚀 Características

- **Autenticación y Autorización**: Sistema completo con JWT, refresh tokens y roles (admin, empresa, visitante)
- **Panel Administrativo**: Gestión de empresas, usuarios, validación de contenido y estadísticas
- **Panel de Empresa**: Gestión de perfil, publicación de noticias/proyectos, solicitud de servicios
- **Observatorio Público**: Visualización de datos, mapas interactivos con PostGIS, estadísticas
- **Arquitectura OOP**: Programación orientada a objetos con TypeScript
- **Patrón MVC + Repository**: Separación clara de responsabilidades
- **Seguridad**: Helmet, CORS, Rate Limiting, bcrypt para contraseñas
- **Documentación API**: Swagger/OpenAPI integrado

## 📋 Requisitos Previos

- Node.js >= 16.x
- PostgreSQL >= 13.x con extensión PostGIS
- npm o yarn

## 🛠️ Instalación

### 1. Clonar el repositorio

```bash
cd InduCat
```

### 2. Configurar Backend

```bash
cd Backend
npm install
```

### 3. Configurar Base de Datos

```bash
# Crear base de datos
createdb inducat_db

# Habilitar PostGIS
psql inducat_db -c "CREATE EXTENSION IF NOT EXISTS postgis;"

# Ejecutar script de inicialización
psql inducat_db < ../DB/init_db.sql
```

### 4. Configurar Variables de Entorno

```bash
cp .env.example .env
```

Editar `.env` con tus configuraciones:

```env
# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=inducat_db
DB_USER=postgres
DB_PASSWORD=tu_contraseña

# JWT Secrets (cambiar en producción)
JWT_SECRET=tu_secret_key_muy_seguro
JWT_REFRESH_SECRET=tu_refresh_secret_key_muy_seguro

# Server
PORT=3000
NODE_ENV=development
```

## 🏃 Ejecución

### Desarrollo - Backend

```bash
cd Backend
npm run dev
```

El servidor estará disponible en `http://localhost:3000`

### Producción - Backend

```bash
cd Backend
npm run build
npm start
```

## 📚 Documentación API

Una vez iniciado el servidor, accede a la documentación interactiva en:

```
http://localhost:3000/api-docs
```

## 🔑 Endpoints Principales

### Autenticación (`/api/v1/auth`)

- `POST /register` - Registrar nuevo usuario
- `POST /login` - Iniciar sesión
- `POST /refresh` - Refrescar access token
- `POST /logout` - Cerrar sesión
- `GET /me` - Obtener perfil del usuario actual
- `POST /forgot-password` - Solicitar restablecimiento de contraseña
- `POST /reset-password` - Restablecer contraseña

### Empresas (`/api/v1/empresas`)

- `GET /` - Listar empresas
- `POST /` - Crear empresa (Admin)
- `GET /:id` - Obtener empresa por ID
- `PUT /:id` - Actualizar empresa (Admin/Empresa propia)
- `DELETE /:id` - Eliminar empresa (Admin)
- `GET /estadisticas` - Estadísticas agregadas

### Usuarios (`/api/v1/usuarios`)

- `GET /` - Listar usuarios (Admin)
- `POST /` - Crear usuario (Admin)
- `GET /:id` - Obtener usuario por ID
- `PUT /:id` - Actualizar usuario (Admin)
- `DELETE /:id` - Eliminar usuario (Admin)

### Noticias/Proyectos (`/api/v1/noticias`)

- `GET /` - Listar noticias validadas
- `POST /` - Crear noticia (Empresa)
- `PUT /:id` - Actualizar noticia (Empresa propia)
- `PUT /:id/validar` - Validar/Rechazar noticia (Admin)
- `DELETE /:id` - Eliminar noticia (Admin)

### Servicios/Incentivos (`/api/v1/servicios`)

- `GET /` - Listar servicios
- `POST /` - Crear servicio (Admin)
- `PUT /:id` - Actualizar servicio (Admin)
- `POST /:id/asignar` - Asignar servicio a empresa (Admin)

## 🏗️ Estructura del Proyecto

```
InduCat/
├── Backend/                    # Backend unificado en TypeScript
│   ├── src/
│   │   ├── config/            # Configuración (DB, Swagger)
│   │   ├── models/            # Modelos de dominio (OOP)
│   │   ├── repositories/      # Capa de acceso a datos
│   │   ├── services/          # Lógica de negocio
│   │   │   └── AuthService.ts # Servicio de autenticación
│   │   ├── controllers/       # Controladores REST
│   │   │   ├── AuthController.ts
│   │   │   ├── EmpresaController.ts
│   │   │   ├── UsuarioController.ts
│   │   │   ├── NoticiaProyectoController.ts
│   │   │   └── ServicioIncentivoController.ts
│   │   ├── middleware/        # Middleware
│   │   │   ├── authMiddleware.ts
│   │   │   ├── errorHandler.ts
│   │   │   └── validation.ts
│   │   ├── routes/            # Definición de rutas
│   │   │   ├── index.ts       # Router principal
│   │   │   ├── auth.ts        # Rutas de autenticación
│   │   │   ├── empresas.ts
│   │   │   ├── usuarios.ts
│   │   │   ├── noticias.ts
│   │   │   └── servicios.ts
│   │   ├── types/             # Tipos TypeScript
│   │   ├── app.ts             # Configuración Express
│   │   └── server.ts          # Punto de entrada
│   ├── migrations/            # Migraciones de base de datos
│   ├── tests/                 # Tests
│   ├── package.json
│   └── tsconfig.json
│
├── DB/                        # Scripts de base de datos
│   └── init_db.sql           # Script de inicialización
│
├── Panel Admin/              # Módulo de administración (legacy)
├── Panel Empresa/            # Módulo de empresas (legacy)
├── Observatorio Público/     # Módulo observatorio (legacy)
└── Sistema de Autenticación/ # Módulo de auth (legacy - integrado)
```

## 🔐 Roles y Permisos

### Admin (Ministerio)

- Gestión completa de empresas
- Gestión de usuarios
- Validación de noticias/proyectos
- Gestión de servicios e incentivos
- Acceso a estadísticas

### Empresa

- Ver y editar su propia información
- Crear y editar noticias/proyectos
- Ver servicios disponibles
- Solicitar servicios

### Visitante

- Ver información pública de empresas
- Ver noticias validadas
- Ver servicios disponibles

## 🧪 Testing

```bash
cd Backend

# Ejecutar todos los tests
npm test

# Tests con watch mode
npm run test:watch

# Tests de integración
npm run test:integration

# Coverage
npm test -- --coverage
```

## 🔧 Tecnologías Utilizadas

### Backend

- **Node.js** - Runtime de JavaScript
- **TypeScript** - Superset tipado de JavaScript
- **Express** - Framework web
- **PostgreSQL** - Base de datos relacional
- **PostGIS** - Extensión geoespacial para PostgreSQL
- **pg** - Cliente PostgreSQL para Node.js
- **bcrypt** - Hash de contraseñas
- **jsonwebtoken** - Autenticación JWT
- **express-validator** - Validación de datos
- **Swagger/OpenAPI** - Documentación de API
- **Jest** - Framework de testing
- **Helmet** - Seguridad HTTP
- **Morgan** - Logging de requests
- **Winston** - Logging avanzado

## 📝 Scripts Disponibles

```bash
# Desarrollo
npm run dev          # Inicia servidor en modo desarrollo con hot-reload

# Producción
npm run build        # Compila TypeScript a JavaScript
npm start            # Inicia servidor en modo producción

# Testing
npm test             # Ejecuta tests con coverage
npm run test:watch   # Tests en modo watch
npm run test:integration  # Solo tests de integración

# Calidad de código
npm run lint         # Ejecuta ESLint
npm run format       # Formatea código con Prettier

# Base de datos
npm run migrate      # Ejecuta migraciones
```

## 🚀 Deployment

### Usando Docker (Recomendado)

```bash
# Construir imagen
docker build -t inducat-backend ./Backend

# Ejecutar contenedor
docker run -p 3000:3000 --env-file .env inducat-backend
```

### Manual

1. Configurar servidor con Node.js y PostgreSQL
2. Clonar repositorio
3. Instalar dependencias: `npm install --production`
4. Configurar variables de entorno
5. Compilar: `npm run build`
6. Iniciar: `npm start`

## 🔒 Seguridad

- Contraseñas hasheadas con bcrypt (10 rounds)
- JWT con tokens de acceso y refresh
- Rate limiting para prevenir ataques de fuerza bruta
- Helmet para headers de seguridad HTTP
- CORS configurado
- Validación de entrada con express-validator
- SQL injection prevention con prepared statements

## 📞 Contacto

Ministerio de Producción de Catamarca  
Email: contacto@catamarca.gob.ar

## 📄 Licencia

MIT

---

## 🔄 Estado de Integración

✅ **Completado:**
- Backend unificado en TypeScript
- Módulo de autenticación integrado
- Rutas consolidadas
- Configuración centralizada
- Documentación API

🚧 **En Progreso:**
- Integración de Panel Admin
- Integración de Panel Empresa
- Integración de Observatorio Público
- Frontend unificado

📋 **Pendiente:**
- Tests de integración completos
- Deployment automatizado
- Documentación de arquitectura detallada
