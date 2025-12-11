# 🚀 Instrucciones para Ejecutar el Proyecto InduCat

## ⚠️ PASO CRÍTICO: Configurar Contraseña de PostgreSQL

**ANTES de continuar**, debes configurar tu contraseña de PostgreSQL en el archivo `.env`:

1. Abre el archivo: `Backend\.env`
2. Busca la línea: `DB_PASSWORD=`
3. Agrega tu contraseña: `DB_PASSWORD=tu_contraseña_aqui`

### Si no conoces tu contraseña de PostgreSQL:
- **Windows**: Generalmente es la que configuraste durante la instalación
- Si no la recuerdas, puedes usar pgAdmin para cambiarla
- O reinstalar PostgreSQL con una contraseña conocida

---

## 📋 Pasos de Ejecución

### 1. Configurar Contraseña (OBLIGATORIO)
```bash
# Edita Backend\.env y configura DB_PASSWORD=tu_contraseña
```

### 2. Crear Base de Datos
```bash
cd Backend
node scripts/create_db.js
```

Este script:
- ✅ Crea la base de datos `inducat_db`
- ✅ Habilita la extensión PostGIS

### 3. Inicializar Base de Datos
```bash
cd Backend
npx ts-node src/scripts/setup_db.ts
```

Este script:
- ✅ Crea todas las tablas
- ✅ Inserta datos de prueba (admin, empresas, servicios)

### 4. Iniciar Servidor
```bash
cd Backend
npm run dev
```

El servidor estará disponible en: **http://localhost:3000**

---

## ✅ Verificación

### 1. Health Check
Abre en tu navegador o usa curl:
```
http://localhost:3000/health
```

### 2. Documentación API (Swagger)
```
http://localhost:3000/api-docs
```

### 3. Probar Login
```bash
curl -X POST http://localhost:3000/api/v1/auth/login ^
  -H "Content-Type: application/json" ^
  -d "{\"email\":\"admin@catamarca.gob.ar\",\"contrasena\":\"admin123\"}"
```

---

## 🔑 Credenciales de Prueba

### Administrador
- **Email:** `admin@catamarca.gob.ar`
- **Password:** `admin123`

### Empresas
Los usuarios de empresa se generan automáticamente:
- **Email:** `contacto@[nombre-empresa].com`
- **Password:** `admin123`

---

## 📚 Endpoints Principales

- **Autenticación:** `/api/v1/auth`
  - `POST /login` - Iniciar sesión
  - `POST /register` - Registrar usuario
  - `GET /me` - Perfil actual

- **Empresas:** `/api/v1/empresas`
  - `GET /` - Listar empresas
  - `POST /` - Crear empresa (Admin)
  - `GET /:id` - Obtener empresa

- **Noticias:** `/api/v1/noticias`
  - `GET /` - Listar noticias
  - `POST /` - Crear noticia (Empresa)

- **Servicios:** `/api/v1/servicios`
  - `GET /` - Listar servicios
  - `POST /` - Crear servicio (Admin)

---

## ❌ Solución de Problemas

### Error: "password authentication failed"
- Verifica que `DB_PASSWORD` en `.env` sea correcta
- Verifica que PostgreSQL esté ejecutándose
- Verifica que el usuario `postgres` exista

### Error: "database does not exist"
- Ejecuta: `node Backend/scripts/create_db.js`

### Error: "PostGIS extension not found"
- Instala PostGIS desde el instalador de PostgreSQL
- O ejecuta: `CREATE EXTENSION postgis;` en psql

### Puerto 3000 en uso
- Cambia `PORT` en `Backend\.env` a otro puerto (ej: 3001)

---

## 📝 Estado Actual del Proyecto

✅ **Completado:**
- Dependencias instaladas
- Archivo `.env` creado
- Hash de contraseña actualizado en seed.sql
- Scripts de setup creados

⏳ **Pendiente:**
- Configurar contraseña de PostgreSQL en `.env`
- Crear base de datos
- Inicializar base de datos
- Iniciar servidor

---

## 🎯 Próximos Pasos

1. **Configura la contraseña** en `Backend\.env`
2. **Ejecuta los comandos** en orden (crear BD → inicializar → iniciar)
3. **Prueba la API** usando Swagger en `/api-docs`

¡Listo para probar! 🚀

