# 🚀 Guía Rápida para Probar el Proyecto InduCat

## ✅ Pasos para Ejecutar el Proyecto

### 1. Configurar Variables de Entorno

Crea un archivo `.env` en la carpeta `Backend`:

```bash
cd Backend
copy .env.example .env
```

Luego edita el archivo `.env` y configura:
- `DB_PASSWORD`: Tu contraseña de PostgreSQL
- `JWT_SECRET`: Una clave secreta segura (puede ser cualquier string largo)
- `JWT_REFRESH_SECRET`: Otra clave secreta diferente

### 2. Configurar Base de Datos PostgreSQL

Asegúrate de tener PostgreSQL instalado con la extensión PostGIS.

#### Crear la base de datos:

```bash
# Conectarse a PostgreSQL
psql -U postgres

# Crear la base de datos
CREATE DATABASE inducat_db;

# Habilitar PostGIS
\c inducat_db
CREATE EXTENSION IF NOT EXISTS postgis;

# Salir
\q
```

### 3. Inicializar la Base de Datos

```bash
cd Backend
npx ts-node src/scripts/setup_db.ts
```

Este script creará todas las tablas y datos de prueba.

### 4. Iniciar el Servidor

```bash
cd Backend
npm run dev
```

El servidor estará disponible en: **http://localhost:3000**

### 5. Probar la API

#### Opción 1: Documentación Interactiva (Swagger)
Abre en tu navegador:
```
http://localhost:3000/api-docs
```

#### Opción 2: Probar Endpoints Manualmente

**Health Check:**
```bash
curl http://localhost:3000/health
```

**Login de Admin:**
```bash
curl -X POST http://localhost:3000/api/v1/auth/login ^
  -H "Content-Type: application/json" ^
  -d "{\"email\":\"admin@catamarca.gob.ar\",\"contrasena\":\"admin123\"}"
```

## 🔑 Credenciales de Prueba

### Administrador
- **Email:** `admin@catamarca.gob.ar`
- **Password:** `admin123`

### Empresas
Los usuarios de empresa se generan automáticamente con el formato:
- **Email:** `contacto@[nombre-empresa].com`
- **Password:** `admin123`

## 📚 Endpoints Principales

- **Autenticación:** `/api/v1/auth`
- **Empresas:** `/api/v1/empresas`
- **Usuarios:** `/api/v1/usuarios`
- **Noticias:** `/api/v1/noticias`
- **Servicios:** `/api/v1/servicios`

## 🔍 Verificación Rápida

1. ✅ Servidor corriendo en puerto 3000
2. ✅ Base de datos conectada
3. ✅ PostGIS habilitado
4. ✅ Swagger disponible en `/api-docs`
5. ✅ Login funcionando con credenciales de prueba

## ❌ Solución de Problemas

### Error de conexión a base de datos
- Verifica que PostgreSQL esté ejecutándose
- Verifica las credenciales en `.env`
- Verifica que la base de datos `inducat_db` exista

### Error "PostGIS extension not found"
Instala PostGIS en PostgreSQL:
- **Windows:** Instalar desde el instalador de PostgreSQL (seleccionar PostGIS)
- **Ubuntu/Debian:** `sudo apt-get install postgresql-14-postgis-3`

### Puerto 3000 en uso
Cambia el `PORT` en el archivo `.env` a otro puerto disponible.

