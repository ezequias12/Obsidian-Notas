# Instructivo – PostgreSQL con Docker

> **Objetivo:** montar y poner en funcionamiento un contenedor de **PostgreSQL 17** usando **Docker Compose**, con datos persistentes, variables en `.env`, y scripts de inicialización (usaremos **Sakila** para PostgreSQL). Este instructivo será el **primer contacto** con Docker en la materia.

## Introducción breve a Docker

### ¿Por qué Docker?

Docker es una plataforma que permite empaquetar aplicaciones junto con todas sus dependencias en contenedores. Esto simplifica enormemente la instalación, despliegue y ejecución de software, asegurando que funcione igual en cualquier entorno (desarrollo, testing o producción).

Entre sus funcionalidades principales se destacan:

- **Ejecución de contenedores livianos y portables**: permite aislar procesos sin la sobrecarga de una máquina virtual completa, ocupando pocos recursos y arrancando rápidamente.
- **Aislamiento entre aplicaciones**: cada contenedor corre en su propio entorno, evitando conflictos de dependencias o configuraciones entre servicios.
- **Definición y configuración por archivos de texto**: Docker permite definir el comportamiento de servicios y entornos completos mediante archivos de texto como `Dockerfile` y `docker-compose.yml`, lo que facilita la reproducción exacta de configuraciones y mejora la trazabilidad en equipos de trabajo.
- **Reutilización de imágenes y componentes**: se pueden descargar imágenes oficiales o personalizadas desde Docker Hub y combinarlas, reduciendo el tiempo de configuración.
- **Despliegue reproducible con `docker-compose`**: al definir múltiples servicios y sus relaciones en un solo archivo, se pueden levantar entornos completos de forma automática y coherente en cualquier equipo.

### En Resumen

- **Imagen**: plantilla inmutable (ej. `postgres:17`) que define qué software trae el contenedor.
- **Contenedor**: instancia en ejecución de una imagen. Podemos tener varios contenedores de la misma imagen.
- **Volumen**: carpeta persistente fuera del contenedor. Evita perder datos al borrar/recrear el contenedor.
- **Puerto**: mapeo de red para acceder desde el host (ej. `5432:5432`).
- **Docker Compose**: archivo `docker-compose.yml` que define servicios, volúmenes y redes para levantar todo con un solo comando.

> 🧠 Idea fuerza: **contenedor ≠ máquina virtual**. Es un proceso aislado que comparte el kernel del anfitrión.

Este instructivo guía la instalación de Docker en distintos sistemas operativos y la posterior creación y configuración de un contenedor de **PostgreSQL**, para utilizarlo como **Sistema de Administración de Base de Datos** de los Microservicios de Backend.

Incluye la inicialización de la base de datos de ejemplo Sakila para utilizarla en ejemplos del backend.

## 🐳 1. Instalación de Docker

A continuación, se detallan los pasos para instalar Docker en cada sistema operativo.

### 1.1 En Windows 10/11

1. Descargar Docker Desktop desde: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
2. Ejecutar el instalador y seguir los pasos.
3. Al finalizar, reiniciar la PC si es necesario.
4. Verificar desde terminal (PowerShell o CMD):

```bash
docker --version
docker compose version
```

> ✅ Si Docker está correctamente instalado, los comandos anteriores deberían devolver algo como:
>
> - `Docker version 28.3.x, build abc1234`
> - `Docker Compose version v2.38.x`
>
> 💡 Requiere tener habilitado **WSL2** (Subsistema de Windows para Linux versión 2), que permite ejecutar un entorno Linux directamente sobre Windows.
> Se recomienda instalar **Ubuntu** como distro por su compatibilidad, soporte extendido y facilidad de uso.
> Docker Desktop guía automáticamente en la instalación y configuración inicial si aún no está configurado.

### 1.2 En Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
```

> 🔁 Reiniciar sesión para que se aplique el grupo `docker`

Verificar:

```bash
docker --version
docker compose version
```

### 1.3 En macOS

1. Descargar Docker Desktop desde: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
2. Abrir el archivo `.dmg` y arrastrar Docker a Aplicaciones.
3. Ejecutar Docker Desktop y completar configuración.

Verificar:

```bash
docker --version
docker compose version
```

## 🚀 2. Lanzar un contenedor de PostgreSQL

### Estructura de carpetas sugerida

```txt
postgres-compose/
├─ .env
├─ docker-compose.yml
├─ initdb/           # scripts de inicialización (se ejecutan al primer arranque)
│  ├─ 01_schema.sql  # aquí pondrás el schema de Sakila
│  ├─ 02_data.sql    # aquí pondrás los inserts de Sakila
│  └─ 03_extras.sql  # opcional: índices, vistas, grants
└─ pgdata/           # datos persistentes (NO subir a git)
```

> 📝 Los scripts dentro de `initdb/` se ejecutan **solo la primera vez** que se inicializa el volumen de datos.

### Archivo `.env`

Creamos un archivo `.env` en la raíz con:

```env
POSTGRES_VERSION=17
POSTGRES_USER=postgres
POSTGRES_PASSWORD=pg@dm1n
POSTGRES_DB=sakila
TZ=America/Argentina/Cordoba
```

> Se puede cambiar usuario/clave/DB según la práctica, sin embargo en este caso el usuario está referenciado en los scripts de inicialización por lo que de cambiar aquí también se debe reemplazar en los scripts de inicialización. **No** se deben contraseñas reales a repos públicos.

### `docker-compose.yml`

```yaml
services:
  postgres:
    image: postgres:${POSTGRES_VERSION}
    container_name: postgres
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
      TZ: ${TZ}
      POSTGRES_INITDB_ARGS: "--encoding=UTF8 --locale=C"
    ports:
      - "5432:5432"
    volumes:
      - ./pgdata:/var/lib/postgresql/data
      - ./initdb:/docker-entrypoint-initdb.d
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

# Red por defecto (implícita). Más adelante podemos definir una red dedicada si agregamos más servicios.
```

#### ¿Qué hace cada parte?

- **image**: usa `postgres:17` (tomada de Docker Hub).
- **environment**: variables soportadas por la imagen oficial.
  - `POSTGRES_INITDB_ARGS` fija encoding/locale al crear el cluster.
- **ports**: expone el puerto 5432 del contenedor al 5432 del host.
- **volumes**:
  - `pgdata` guarda físicamente tus datos (persistencia).
  - `initdb` contiene scripts `.sql` que se ejecutan al primer arranque.
- **healthcheck**: verifica que Postgres acepte conexiones.

### Colocar los scripts de **Sakila**

Copia el **schema** y los **datos** de **Sakila (port a PostgreSQL)** dentro de `initdb/` con nombres tipo:

- `initdb/01_postgres-sakila-schema.sql`
- `initdb/02_postgres-sakila-insert.sql`
- `initdb/03_postgres-sakila-appuser.sql`

> El *entrypoint* oficial ejecuta todos los `.sql` ordenados alfabéticamente.

### Levantar y administrar Postgres con Docker Compose

En esta sección explicamos **cómo iniciar, detener y volver a levantar** el contenedor de PostgreSQL usando Docker Compose. También incluimos explicaciones detalladas de cada comando.

#### 1. Primera ejecución: `docker compose up`

La primera vez que levantemos el servicio conviene **no usar `-d`** (modo “detached”), para poder ver los mensajes de log en la misma consola, es decir para crear e iniciar el servicio por primera vez vamos a ejecutar:

```bash
docker compose up
```

- **Qué hace**: descarga la imagen si no existe, crea el contenedor y lo inicia en primer plano.
- **Qué vamos a ver**: líneas de log con la inicialización del clúster, ejecución de scripts de `initdb/` (Sakila), y finalmente un mensaje similar a:

  ```txt
  LOG:  database system is ready to accept connections
  ```

  Ese mensaje indica que Postgres ya está listo.

##### Cómo salir

- Presionamos <kbd>Ctrl</kbd>+<kbd>C</kbd> para cortar la ejecución.
- **Importante**: al hacer esto, el contenedor se apaga, pero los datos y la configuración **ya quedaron persistidos** en `./pgdata`.

#### 2. Levantar en segundo plano: `docker compose up -d`

Una vez que la base ya fue inicializada correctamente, podemos usar el modo “detached” para que el contenedor corra en segundo plano:

```bash
docker compose up -d
```

> Resultado:  
> ![picture 3](images/d8727802a06bfe0e661bdbf2d7b5e3d7ec4441a9bb793c69cae941f6d267b321.png)  

- **Qué hace**: levanta el servicio sin bloquear nuestra consola.
- Para verificar el estado:

  ```bash
  docker compose ps
  ```

  > Resultado:  
  > ![picture 0](images/041e409ec1f6a13b878e0b85a0a03ee110690631474a3a0a24d57db9ed688d83.png)  

- Para ver logs en vivo:

  ```bash
  docker compose logs -f postgres
  ```

  > Resultado:  
  > ![picture 1](images/9061e3ba179a98ba56a94dba4512f4ab29ee79e4ba04a95f0def928526c271e9.png)  

##### Alternativa combinada

Podemos levantar en background y engancharnos a los logs inmediatamente:

```bash
docker compose up -d && docker compose logs -f postgres
```

## 3. Detener el servidor

Para detener el contenedor sin eliminar datos ni configuración:

```bash
docker compose stop
```

> Resultado:  
> ![picture 2](images/8fd3d87a7a7313767e5d4170ea954f9a07d2ac9e0851bb8611d444e1ae5f919a.png)  

- **Qué hace**: apaga el proceso de Postgres dentro del contenedor, pero mantiene intactos los datos.

## 4. Volver a iniciar

La próxima vez que queramos usar Postgres:

```bash
docker compose start
```

> Resultado:  
> ![picture 4](images/11f237986b1fcc63d3729c979e56a55cb1f621c0515213f0e31053f97705766b.png)  

- **Qué hace**: inicia nuevamente el contenedor detenido con la misma configuración y los mismos datos.

## 5. Borrar y recrear todo (con precaución)

> [!WARNING]
> Se perderan todos los datos y cambios hechos a la base de datos luego de la inicialización.

Si necesitamos reinicializar completamente (volver a correr scripts de `initdb/` desde cero):

```bash
docker compose down
rm -rf ./pgdata
```

Luego:

```bash
docker compose up
```

Esto borra los datos persistidos, pero es útil en entornos de prueba o demos.

## Resumen práctico

- **Primera vez**: `docker compose up` → ver logs, esperar mensaje de “ready”.
- **Salir**: `Ctrl+C` → apaga el contenedor, pero deja datos configurados.
- **Usos posteriores**: `docker compose up -d` → modo background.
- **Ver estado**: `docker compose ps`.
- **Ver logs**: `docker compose logs -f postgres`.
- **Detener**: `docker compose stop`.
- **Reiniciar**: `docker compose start` o `docker compose up -d`.
- **Reinicializar limpio**: `docker compose down && rm -rf ./pgdata`.

Con estas variantes, siempre tendremos control sobre cómo levantar y administrar el contenedor de PostgreSQL en nuestras prácticas.

### Probar conexión

#### Opción A: desde el host (si tenés `psql` instalado)

```bash
PGPASSWORD=$POSTGRES_PASSWORD psql -h localhost -U $POSTGRES_USER -d $POSTGRES_DB -c "SELECT version();"
```

#### Opción B: dentro del contenedor

```bash
docker exec -it postgres psql -U $POSTGRES_USER -d $POSTGRES_DB -c "\dt"
```

#### Windows (PowerShell)

```powershell
$env:PGPASSWORD=$env:POSTGRES_PASSWORD; psql -h localhost -U $env:POSTGRES_USER -d $env:POSTGRES_DB -c "SELECT version();"
```

> Si ves tablas de Sakila (`actor`, `film`, `rental`, etc.), la inicialización fue exitosa.

### Mantenimiento básico

- **Detener**: `docker compose stop`
- **Reiniciar**: `docker compose restart`
- **Apagar y dejar contenedor**: `docker compose down`
- **Borrar TODO (incluye datos)**: `docker compose down -v`
  *(Esto elimina `pgdata`. Úsalo solo si querés re-inicializar desde cero.)*

### Troubleshooting

- **El puerto 5432 ya está en uso**: cambialo en `ports: "5433:5432"` y conecta a `localhost:5433`.
- **Los scripts no se ejecutan**: la carpeta `pgdata` ya existe; borra el volumen (con cuidado) y levanta de nuevo.
- **Encoding/acentos raros**: confirmá `SHOW SERVER_ENCODING;` → `UTF8`.
- **Hora/Timezone**: `SHOW TIMEZONE;` debería reflejar `${TZ}`.
- **Credenciales**: variables `.env` deben coincidir con lo que usás para conectarte.

### Buenas prácticas para clase

- No publicar `.env` con contraseñas reales.
- Agregar `pgdata/` al `.gitignore`.
- Para reproducibilidad, anclar imagen exacta (por ejemplo `postgres:17.0`).
- Documentar un comando de **sanity check** (`SELECT 1;`, conteo de tablas Sakila, etc.).

### (Opcional) Script de arranque/ayuda

Podés sumar un pequeño wrapper para ver logs o conectarte:

**`bin/pg-logs.sh`**

```bash
#!/usr/bin/env bash
docker compose logs -f postgres
```

**`bin/pg-psql.sh`**

```bash
#!/usr/bin/env bash
docker exec -it postgres psql -U ${POSTGRES_USER} -d ${POSTGRES_DB}
```

Dales permisos:

```bash
chmod +x bin/*.sh
```

## Reflexión Final

Con este contenedor tenemos el DBMS **PostgreSQL 17** corriendo con datos persistentes y **Sakila** inicializada automáticamente. Este entorno sirve como base para los ejemplos de **JDBC** y para el salto posterior a **JPA**.
