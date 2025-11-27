# Nexus Ecosystem

Bienvenido al **Nexus Ecosystem**. Este repositorio sirve como el núcleo central para una arquitectura modular basada en microservicios y componentes reutilizables, gestionados a través de submodulos de Git.

## Arquitectura del Sistema

El sistema está diseñado para ser modular y escalable, compuesto por plataformas independientes orquestadas por un API Gateway central.

### Componentes Clave

1.  **API Gateway (Futuro)**: Punto de entrada único para todo el tráfico externo.
2.  **AuthMS**: Microservicio central de autenticación e identidad.
3.  **Plataformas Independientes**: Sistemas autónomos que consumen AuthMS para validación de identidad pero operan de forma aislada.

## Estrategia de Puertos

Para evitar conflictos en desarrollo local y estandarizar la orquestación:

| Servicio         | Puerto Host | Tipo        | Descripción                          |
| :--------------- | :---------- | :---------- | :----------------------------------- |
| **Gateway**      | `3000`      | **Público** | Punto de entrada principal (Ingress) |
| **AuthMS**       | `3001`      | Privado     | Servicio de Identidad                |
| **Plataforma A** | `3002`      | Privado     | Futuro sistema independiente         |
| **Plataforma B** | `3003`      | Privado     | Futuro sistema independiente         |

## Integración Continua (CI/CD)

El repositorio cuenta con un pipeline centralizado (`.github/workflows/ecosystem-ci.yml`) que valida automáticamente la integridad de todos los submódulos.

- **Checkout Recursivo**: Descarga todos los submódulos.
- **Validación**: Ejecuta linting, compilación y pruebas unitarias para cada servicio (actualmente AuthMS).

## Comenzando

### Prerrequisitos

- Git instalado en tu sistema.

### Clonar el Repositorio

Para clonar este repositorio junto con todos sus submódulos, utiliza el siguiente comando:

```bash
git clone --recursive https://github.com/JonathanHerSa/nexus-ecosystem.git
```

Si ya has clonado el repositorio sin la opción `--recursive`, puedes inicializar y actualizar los submódulos con:

```bash
git submodule update --init --recursive
```

## Gestión de Submódulos

### Agregar un Nuevo Submódulo

Para añadir un nuevo servicio o componente al ecosistema:

```bash
git submodule add <url-del-repositorio> <ruta-de-destino>
# Ejemplo:
# git submodule add https://github.com/usuario/nuevo-servicio.git Services/NuevoServicio
```

### Actualizar Submódulos

Para actualizar todos los submódulos a su última versión en la rama remota configurada:

```bash
git submodule update --remote
```

Para actualizar un submódulo específico, navega a su directorio y haz un pull:

```bash
cd Auth
git pull origin main
```

## Módulos Actuales

### [Auth](./AuthMS)

El servicio de Autenticación maneja el registro de usuarios, inicio de sesión y gestión de tokens.

- **Tecnologías**: NestJS, Prisma, PostgreSQL.
- **Ubicación**: `./AuthMS`

---

Desarrollado por [JonathanHerSa](https://github.com/JonathanHerSa)
