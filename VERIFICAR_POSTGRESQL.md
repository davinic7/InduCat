# 🔍 Guía para Verificar y Configurar PostgreSQL

## Problema Detectado
No se pudo conectar a PostgreSQL con las credenciales proporcionadas. Sigue estos pasos para verificar y solucionar.

---

## 1. Verificar que PostgreSQL esté Instalado

### Opción A: Buscar en el sistema
```powershell
# Buscar servicios de PostgreSQL
Get-Service | Where-Object {$_.Name -like "*postgres*"}

# Buscar procesos
Get-Process | Where-Object {$_.ProcessName -like "*postgres*"}

# Buscar en Program Files
Get-ChildItem "C:\Program Files" | Where-Object {$_.Name -like "*postgres*"}
Get-ChildItem "C:\Program Files (x86)" | Where-Object {$_.Name -like "*postgres*"}
```

### Opción B: Buscar pgAdmin
Si tienes pgAdmin instalado, PostgreSQL debería estar instalado también.

---

## 2. Verificar que PostgreSQL esté Ejecutándose

### Iniciar el servicio de PostgreSQL:

**Método 1: Servicios de Windows**
1. Presiona `Win + R`
2. Escribe `services.msc` y presiona Enter
3. Busca "postgresql" o "PostgreSQL"
4. Haz clic derecho → "Iniciar" (si está detenido)

**Método 2: PowerShell (como Administrador)**
```powershell
# Listar servicios de PostgreSQL
Get-Service | Where-Object {$_.Name -like "*postgres*"}

# Iniciar servicio (reemplaza "postgresql-x64-XX" con el nombre real)
Start-Service postgresql-x64-14
# o
Start-Service postgresql-x64-15
# o
Start-Service postgresql-x64-16
```

---

## 3. Verificar Usuario y Contraseña

### Opción A: Usar pgAdmin
1. Abre pgAdmin
2. Conecta al servidor (generalmente "PostgreSQL XX")
3. Si te pide contraseña, esa es la contraseña correcta
4. Verifica el usuario en la conexión (generalmente "postgres")

### Opción B: Usar psql desde la línea de comandos
```powershell
# Buscar psql.exe
Get-ChildItem -Path "C:\Program Files" -Recurse -Filter "psql.exe" -ErrorAction SilentlyContinue

# Una vez encontrado, conéctate:
# "C:\Program Files\PostgreSQL\XX\bin\psql.exe" -U postgres
```

### Opción C: Verificar archivo pg_hba.conf
El archivo de configuración de autenticación está generalmente en:
```
C:\Program Files\PostgreSQL\XX\data\pg_hba.conf
```

Verifica qué método de autenticación está configurado (md5, scram-sha-256, trust, etc.)

---

## 4. Probar Conexión Manualmente

### Si tienes psql disponible:
```bash
psql -U postgres -h localhost
# Te pedirá la contraseña
```

### Si tienes pgAdmin:
1. Abre pgAdmin
2. Crea una nueva conexión
3. Usa:
   - Host: localhost
   - Port: 5432
   - Database: postgres
   - Username: postgres
   - Password: [tu contraseña]

---

## 5. Soluciones Comunes

### Problema: "password authentication failed"
**Soluciones:**
1. **Resetear contraseña de postgres:**
   ```sql
   -- Conéctate como administrador y ejecuta:
   ALTER USER postgres WITH PASSWORD 'nueva_contraseña';
   ```

2. **Verificar usuario correcto:**
   - El usuario podría no ser "postgres"
   - Verifica en pgAdmin qué usuario usas normalmente

3. **Cambiar método de autenticación temporalmente:**
   - Edita `pg_hba.conf`
   - Cambia `md5` o `scram-sha-256` a `trust` (solo para desarrollo local)
   - Reinicia el servicio de PostgreSQL

### Problema: "connection refused" o "no se puede conectar"
**Soluciones:**
1. Verifica que el servicio esté ejecutándose
2. Verifica que el puerto 5432 esté disponible:
   ```powershell
   netstat -an | findstr 5432
   ```
3. Verifica el firewall de Windows

### Problema: PostgreSQL no está instalado
**Solución:**
1. Descarga PostgreSQL desde: https://www.postgresql.org/download/windows/
2. Durante la instalación:
   - Anota la contraseña que configures para el usuario "postgres"
   - Asegúrate de instalar PostGIS también
3. Una vez instalado, vuelve a intentar

---

## 6. Configuración Alternativa

Si no puedes usar el usuario "postgres", puedes:

1. **Crear un nuevo usuario:**
   ```sql
   CREATE USER inducat_user WITH PASSWORD 'tu_contraseña';
   ALTER USER inducat_user CREATEDB;
   ```

2. **Actualizar .env:**
   ```
   DB_USER=inducat_user
   DB_PASSWORD=tu_contraseña
   ```

---

## 7. Verificar Instalación de PostGIS

Una vez que puedas conectarte, verifica PostGIS:

```sql
CREATE EXTENSION IF NOT EXISTS postgis;
SELECT PostGIS_version();
```

Si PostGIS no está instalado:
- En Windows, reinstala PostgreSQL y selecciona PostGIS en los componentes
- O descarga el instalador de PostGIS para tu versión de PostgreSQL

---

## 📝 Información Necesaria

Para continuar, necesito saber:

1. ✅ ¿PostgreSQL está instalado? (Sí/No)
2. ✅ ¿El servicio está ejecutándose? (Sí/No)
3. ✅ ¿Cuál es el usuario correcto? (postgres/otro)
4. ✅ ¿Cuál es la contraseña correcta?
5. ✅ ¿Tienes pgAdmin instalado? (Sí/No)

---

## 🚀 Una vez Resuelto

Cuando tengas la conexión funcionando, ejecuta:

```bash
cd Backend
node scripts/create_db.js
npx ts-node src/scripts/setup_db.ts
npm run dev
```

