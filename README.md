# Nexus Ecosystem

<p align="center">
  <img src="https://nestjs.com/img/logo_text.svg" alt="NestJS" height="40"/>
  <img src="https://raw.githubusercontent.com/docker-library/docs/master/docker/logo.png" alt="Docker" height="40"/>
  <img src="https://www.postgresql.org/media/img/about/press/elephant.png" alt="PostgreSQL" height="40"/>
  <img src="https://nats.io/img/nats-icon-color.png" alt="NATS" height="40"/>
  <img src="https://assets.vercel.com/image/upload/v1662130559/nextjs/Icon_dark_background.png" alt="Next.js" height="40"/>
</p>

<p align="center">
  <a href="https://nestjs.com"><img src="https://img.shields.io/badge/NestJS-EA2845?style=for-the-badge&logo=nestjs&logoColor=white"/></a>
  <a href="https://www.docker.com/"><img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white"/></a>
  <a href="https://www.prisma.io/"><img src="https://img.shields.io/badge/Prisma-2D3748?style=for-the-badge&logo=prisma&logoColor=white"/></a>
  <a href="https://www.postgresql.org/"><img src="https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white"/></a>
  <a href="https://nats.io/"><img src="https://img.shields.io/badge/NATS-1D8FCD?style=for-the-badge&logo=nats.io&logoColor=white"/></a>
  <a href="https://nextjs.org/"><img src="https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=next.js&logoColor=white"/></a>
  <a href="https://tailwindcss.com/"><img src="https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white"/></a>
  <a href="https://www.typescriptlang.org/"><img src="https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white"/></a>
</p>

Bienvenido al **Nexus Ecosystem**. Este repositorio sirve como el núcleo central para una arquitectura modular basada en microservicios y componentes reutilizables, gestionados a través de submódulos de Git.

## Arquitectura del Sistema

El sistema está diseñado con una arquitectura de microservicios moderna, donde un **API Gateway** centraliza el acceso y los microservicios backend manejan la lógica de negocio.

### Componentes Clave

1. **API Gateway** (`Services/ApiGateway`): Punto de entrada único para todo el tráfico externo

   - Maneja autenticación HTTP (guards, estrategias Passport)
   - Valida tokens JWT y permisos
   - Enruta requests a microservicios vía NATS
   - Expone endpoints: `/auth/*`, `/rbac/*`, `/health`

2. **AuthMS** (`Services/AuthMS`): Microservicio de autenticación, autorización y gestión de identidad

   - Lógica de negocio de autenticación
   - Gestión de usuarios, roles y permisos (RBAC)
   - Generación y validación de tokens JWT
   - Comunicación vía NATS (no expone HTTP directamente)

3. **Auth App** (`Apps/AuthMS`): Frontend de autenticación (Next.js)

### Separación de Responsabilidades

```
┌─────────────────┐
│   API Gateway   │ ← Autenticación HTTP, Validación JWT, Guards
│   (Port 3000)   │
└────────┬────────┘
         │ NATS
         ↓
┌─────────────────┐
│     AuthMS      │ ← Lógica de negocio, Base de datos
│   (Port 3001)   │
└─────────────────┘
```

## Estrategia de Puertos

Para evitar conflictos en desarrollo local y estandarizar la orquestación:

| Servicio         | Puerto Host | Tipo        | Descripción                          |
| :--------------- | :---------- | :---------- | :----------------------------------- |
| **API Gateway**  | `3000`      | **Público** | Punto de entrada principal (Ingress) |
| **AuthMS**       | `3001`      | Privado     | Servicio de Identidad y RBAC         |
| **NATS**         | `4222`      | Privado     | Message broker                       |
| **Plataforma A** | `3002`      | Privado     | Futuro sistema independiente         |
| **Plataforma B** | `3003`      | Privado     | Futuro sistema independiente         |

## Endpoints Disponibles

### Autenticación (`/auth`)

- `POST /auth/sign-up` - Registro de usuario
- `POST /auth/sign-in` - Inicio de sesión
- `POST /auth/log-out` - Cerrar sesión
- `POST /auth/refresh-token` - Refrescar tokens
- `POST /auth/forgot-password` - Recuperar contraseña
- `POST /auth/reset-password` - Restablecer contraseña
- `POST /auth/enable-2fa` - Habilitar autenticación 2FA
- `POST /auth/verify-2fa` - Verificar código 2FA
- `POST /auth/generate-2fa-secret` - Generar secreto 2FA

### Control de Acceso (`/rbac`)

- `POST /rbac/role` - Crear rol
- `GET /rbac/role` - Obtener roles del tenant
- `POST /rbac/role/:roleId/permissions` - Asignar permisos a rol
- `DELETE /rbac/role/:id` - Eliminar rol
- `POST /rbac/permission` - Crear permiso
- `GET /rbac/permission` - Obtener permisos

### Health Check (`/health`)

- `GET /health` - Verificar estado del sistema

## Integración Continua (CI/CD)

El repositorio cuenta con un pipeline centralizado (`.github/workflows/ecosystem-ci.yml`) que valida automáticamente la integridad de todos los submódulos.

- **Checkout Recursivo**: Descarga todos los submódulos
- **Validación**: Ejecuta linting, compilación y pruebas unitarias para cada servicio

## Comenzando

### Prerrequisitos

- Node.js 18+ y npm
- Docker y Docker Compose
- Git instalado en tu sistema

### Clonar el Repositorio

Para clonar este repositorio junto con todos sus submódulos:

```bash
git clone --recursive https://github.com/JonathanHerSa/nexus-ecosystem.git
```

Si ya has clonado el repositorio sin la opción `--recursive`:

```bash
git submodule update --init --recursive
```

### Iniciar el Ecosistema

1. **Configurar variables de entorno**:

   ```bash
   # En Services/ApiGateway/.env
   JWT_SECRET=tu_secret_aqui
   JWT_REFRESH_SECRET=tu_refresh_secret_aqui

   # En Apps/AuthMS/.env
   DATABASE_URL=postgresql://...
   ```

2. **Iniciar servicios con Docker Compose**:

   ```bash
   docker-compose up -d
   ```

3. **O iniciar manualmente**:

   ```bash
   # Terminal 1 - AuthMS
   cd Apps/AuthMS
   npm install
   npm run start:dev

   # Terminal 2 - API Gateway
   cd Services/ApiGateway
   npm install
   npm run start:dev
   ```

## Gestión de Submódulos

### Agregar un Nuevo Submódulo

**Ejemplo para un Servicio (Backend):**

```bash
git submodule add https://github.com/usuario/nuevo-servicio-api.git Services/NuevoServicio
```

**Ejemplo para una App (Frontend):**

```bash
git submodule add https://github.com/usuario/nueva-app-web.git Apps/NuevaApp
```

### Actualizar Submódulos

Para actualizar todos los submódulos:

```bash
git submodule update --remote
```

Para actualizar un submódulo específico:

```bash
cd Apps/AuthMS
git pull origin main
```

## Estructura del Proyecto

```
nexus-ecosystem/
├── Services/              # Microservicios backend
│   └── ApiGateway/       # API Gateway (NestJS)
├── Apps/                  # Aplicaciones y microservicios
│   └── AuthMS/           # Servicio de autenticación (NestJS)
├── compose.yml           # Docker Compose para orquestación
└── .github/
    └── workflows/
        └── ecosystem-ci.yml  # CI/CD centralizado
```

## Módulos Actuales

### [API Gateway](./Services/ApiGateway)

Punto de entrada HTTP del ecosistema. Maneja autenticación, validación y enrutamiento.

- **Tecnologías**: NestJS, Passport, JWT, NATS
- **Puerto**: 3000
- **Ubicación**: `./Services/ApiGateway`

### [AuthMS](./Apps/AuthMS)

Microservicio de autenticación, autorización y gestión de identidad.

- **Tecnologías**: NestJS, Prisma, PostgreSQL, NATS
- **Puerto**: 3001 (privado)
- **Ubicación**: `./Apps/AuthMS`
- **Características**:
  - Autenticación con JWT (Access + Refresh tokens)
  - RBAC (Roles y Permisos)
  - Multi-tenancy
  - Autenticación de dos factores (2FA)
  - Auditoría de sesiones

---

Desarrollado por [JonathanHerSa](https://github.com/JonathanHerSa)
