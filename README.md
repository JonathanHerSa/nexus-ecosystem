# Nexus Ecosystem

Bienvenido al **Nexus Ecosystem**. Este repositorio sirve como el núcleo central para una arquitectura modular basada en microservicios y componentes reutilizables, gestionados a través de submodulos de Git.

## Arquitectura del Sistema

El sistema está diseñado para ser modular y escalable. Cada componente principal (como el servicio de Autenticación) reside en su propio repositorio y se integra aquí como un submódulo. Esto permite un desarrollo independiente de cada módulo mientras se mantiene una estructura cohesiva para el despliegue y la integración.

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

### [Auth](./Auth)

El servicio de Autenticación maneja el registro de usuarios, inicio de sesión y gestión de tokens.

- **Tecnologías**: NestJS, Prisma, PostgreSQL.
- **Ubicación**: `./Auth`

---

Desarrollado por [JonathanHerSa](https://github.com/JonathanHerSa)
