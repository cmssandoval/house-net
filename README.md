# House Net

House Net es una aplicación web para administrar información y recursos del hogar. El proyecto contiene el frontend en React y el backend/API en Express dentro de un único repositorio.

## Funcionalidades

- Registro, inicio de sesión y manejo de sesiones.
- Acceso basado en roles para invitados, usuarios y administradores.
- Inventario con búsqueda, filtros por categoría y operaciones CRUD.
- Administración de categorías, ubicaciones y usuarios.
- Recursos públicos y herramientas privadas.
- Vista de tareas y calendario; estas funcionalidades siguen en desarrollo.
- Tema claro y oscuro.

## Tecnologías

### Frontend

- React + TypeScript
- Vite
- React Router
- Axios
- Zod
- Bootstrap y React Bootstrap
- Vitest y Testing Library

### Backend

- Node.js y Express
- Prisma y PostgreSQL
- Argon2 para contraseñas
- JSON Web Token
- Zod
- Helmet y CORS
- Jest y Supertest
- PM2 para la ejecución en producción

## Estructura del proyecto

```text
frontend/   Aplicación web de React
backend/    API de Express y esquema de Prisma
```

## Requisitos

- Node.js 26 o posterior
- npm 12 o posterior
- PostgreSQL

## Configuración

### 1. Clonar e instalar

```powershell
git clone https://github.com/cmssandoval/house-net.git
cd house-net
npm --prefix frontend install
npm --prefix backend install
```

### 2. Configurar la base de datos

Crea una base de datos PostgreSQL llamada `housenet`, o usa otro nombre y refléjalo en `DATABASE_URL`.

Luego copia el archivo de ejemplo:

```powershell
Copy-Item backend\.env.example backend\.env
```

Configura `backend/.env`:

```dotenv
NODE_ENV=development
PORT=3000
HOST=0.0.0.0
DATABASE_URL=postgresql://usuario:contraseña@localhost:5432/housenet
JWT_SECRET=una_clave_aleatoria_de_al_menos_32_caracteres
```

Puedes generar una clave segura con:

```powershell
node -e "console.log(require('crypto').randomBytes(128).toString('hex'))"
```

Aplica las migraciones de Prisma:

```powershell
npm --prefix backend exec -- prisma migrate deploy
```

Si no tienes migraciones disponibles, puedes crear la estructura con:

```powershell
npm --prefix backend exec -- prisma db push
```

### 3. Configurar el frontend

Copiar el archivo de ejemplo:

```powershell
Copy-Item frontend\.env.example frontend\.env.development
```

Durante el desarrollo separado, configura `frontend/.env.development`:

```dotenv
VITE_API_BASE_URL=http://localhost:3000/api/
```

## Ejecutar en desarrollo

En dos terminales, desde la raíz del repositorio:

```powershell
npm --prefix backend run dev
```

```powershell
npm --prefix frontend run dev
```

Vite mostrará la URL local del frontend, normalmente `http://localhost:5173`. La API estará disponible en `http://localhost:3000`.

Las solicitudes permitidas desde Vite ya están configuradas en `backend/src/config/allowedOrigins.js`, puedes agregar orígenes permitidos si deseas extender la aplicación.

## Ejecutar como una sola aplicación

También puedes servir el frontend compilado desde Express:

```powershell
npm --prefix frontend run build
```

Esto genera `frontend/dist`. Copia el contenido a `backend/dist`:

```powershell
New-Item backend\dist -ItemType Directory -Force
Copy-Item frontend\dist\* backend\dist\ -Recurse -Force
```

Después, configura `NODE_ENV=production` en `backend/.env` y inicia el servidor:

```powershell
npm --prefix backend run start
```

El backend estará disponible en `http://localhost:3000`, y servirá la build del frontend en la raíz, y la API en `http://localhost:3000/api`.

## Comandos

### Frontend

```powershell
npm --prefix frontend run dev
npm --prefix frontend run build
npm --prefix frontend run preview
npm --prefix frontend run lint
npm --prefix frontend test
```

### Backend

```powershell
npm --prefix backend run dev
npm --prefix backend run start
npm --prefix backend run stop
npm --prefix backend test
npm --prefix backend exec -- prisma migrate deploy
```

## Ejecución automática con PM2

Para ejecutar el backend automáticamente al iniciar el servidor:

```powershell
npm install -g pm2
pm2 --version
pm2 startup
cd backend
npm run start
pm2 save
```

El comando generado por `pm2 startup` debe ejecutarse con los permisos adecuados para el sistema operativo.

Comandos útiles:

```powershell
pm2 status
pm2 stop backend-house-net
pm2 restart backend-house-net
pm2 logs backend-house-net
```

Si cambias la versión de Node mediante NVM, actualiza el inicio automático:

```powershell
pm2 unstartup
pm2 startup
pm2 save
```

## Seguridad de red

House Net está diseñado principalmente para una red local (LAN). Antes de exponerlo fuera de esa red:

- Usa contraseñas y claves aleatorias únicas.
- Mantén PostgreSQL, Node.js y el sistema operativo actualizados.
- Configura reglas de firewall explícitas.
- No uses redirección de puertos en el router.
- Para acceso remoto, considera una solución segura como Tailscale.
- Usa HTTPS y restringe los orígenes CORS a los dominios reales.

## Próximas funcionalidades

- Calendario compartido y fechas importantes del hogar.
- Sistema completo de tareas compartidas.
- Artículos para almacenar información académica o de investigación.
- Publicaciones para compartir información interna familiar.
- Herramientas digitales privadas que eviten depender de servicios externos.

## Contacto

Si deseas contactarme, envíame un mensaje:

- **Email: <a href='mailto:carlossan.16.mc@gmail.com'>Enviar un email</a>**
- **GitHub: https://github.com/cmssandoval/**
