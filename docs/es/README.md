# 🚀 TOP WEB STACK
## Pila de tecnologías web modernas. Desarrollo y despliegue en contenedores con AlmaLinux 9

**[🇷🇺 Versión en Ruso](/)** | **[🇬🇧 English version](/en/)** | **[🇪🇸 Versión en Español](/es/)**

---

## 📖 Sobre el Curso

Este curso proporciona formación práctica en desarrollo web moderno utilizando tecnologías empresariales y contenedorización.

### 🎯 Lo que se estudia:

- **🐧 AlmaLinux 9** — Distribución Linux lista para empresas
- **📦 Podman 5.4** — Contenedorización sin Docker
- **🚀 Node.js 26 LTS + Fastify** — Framework backend rápido
- **⚡ Vite 6 + React** — Construcción frontend ultrarrápida
- **🗄️ PostgreSQL 18 + Prisma** — ORM moderna y base de datos SQL
- **📊 SigNoz + OpenTelemetry** — Monitoreo y rastreo
- **🔐 Zitadel IAM** — Gestión de usuarios y autenticación
- **🧩 Komodo** — Gestión de infraestructura y despliegue

### 📚 Estructura del Curso:

El curso completo incluye 13 capítulos que cubren todo, desde la configuración de infraestructura hasta el despliegue en producción.

---

## 📁 Estructura del Proyecto

### Multilingüismo

El curso está disponible en tres idiomas según el estándar Docsify:

```
docs/
├── index.html           # Punto de entrada único
├── _coverpage.md        # Portada en ruso
├── _sidebar.md          # Menú en ruso
├── index.md             # Página principal en ruso
├── README.md            # Este archivo
├── chapter-*.md         # Capítulos en ruso (por defecto)
│
├── en/                  # 🇬🇧 English version
│   ├── _coverpage.md
│   ├── _sidebar.md
│   ├── index.md
│   └── README.md
│
└── es/                  # 🇪🇸 Versión en Español  
    ├── _coverpage.md
    ├── _sidebar.md
    ├── index.md
    └── README.md
```

### Archivos Principales

- **[index.md](index.md)** — página principal del curso con contenido completo
- **[README.md](README.md)** — descripción del curso e inicio rápido (este archivo)
- **[RESOURCES-AND-LINKS.md](../RESOURCES-AND-LINKS.md)** — todos los recursos, enlaces y documentación
- **[_sidebar.md](_sidebar.md)** — menú de navegación lateral
- **[_coverpage.md](_coverpage.md)** — portada del curso

### Capítulos del Curso

1. **[chapter-01-architecture.md](../chapter-01-architecture.md)** — Arquitectura del entorno de desarrollo
   - Arquitectura del proyecto futuro
   - Planificación de infraestructura
   - Selección de tecnologías
   - Diagramas de interacción

2. **[chapter-02-setup.md](../chapter-02-setup.md)** — Configuración del lugar de trabajo del desarrollador
   - Instalación de VSCode en macOS
   - Configuración de SSH y Remote Development
   - Creación de proyecto en servidor remoto
   - Configuración de Dev Container

3. **[chapter-03-almalinux.md](../chapter-03-almalinux.md)** — AlmaLinux y fundamentos del sistema
   - Qué es AlmaLinux
   - Instalación y configuración
   - DNF, Firewalld, Systemd
   - Interfaz web Cockpit

4. **[chapter-04-podman.md](../chapter-04-podman.md)** — Podman: contenedorización sin Docker
   - Arquitectura Podman vs Docker
   - Instalación y configuración
   - Comandos básicos
   - Podman Compose
   - Contenedores Rootless

5. **[chapter-05-git.md](../chapter-05-git.md)** — Git: sistema de control de versiones
   - Fundamentos de trabajo con Git
   - Configuración
   - Git workflow y mejores prácticas
   - Integración con GitHub/GitLab

6. **[chapter-06-komodo.md](../chapter-06-komodo.md)** — Komodo: gestión de infraestructura
   - Instalación de Komodo Core
   - Gestión de servidores
   - Stacks y Deployments
   - Builds y automatización
   - Monitoreo y alertas

7. **[chapter-07-nodejs.md](../chapter-07-nodejs.md)** — Node.js: desarrollo moderno
   - Instalación de Node.js vía NVM
   - Creación de API con Fastify
   - Contenedorización de aplicación
   - Multi-stage builds

8. **[chapter-08-vite.md](../chapter-08-vite.md)** — Vite: frontend moderno
   - Creación de aplicación React
   - Contenedorización del frontend
   - Nginx para producción
   - Optimización de build

9. **[chapter-09-postgresql.md](../chapter-09-postgresql.md)** — PostgreSQL: almacenamiento confiable
   - Despliegue en contenedor
   - Inicialización de BD
   - Integración con Prisma
   - Backup y restauración

10. **[chapter-10-signoz.md](../chapter-10-signoz.md)** — SigNoz: monitoreo y observabilidad
    - Arquitectura SigNoz
    - Despliegue del stack
    - Integración con Node.js
    - OpenTelemetry
    - Dashboards y alertas

11. **[chapter-11-zitadel.md](../chapter-11-zitadel.md)** — Zitadel IAM: autenticación
    - Despliegue de Zitadel
    - Configuración de organizaciones
    - Integración OIDC/OAuth2
    - Conexión vía Lucia Auth

12. **[chapter-12-integration.md](../chapter-12-integration.md)** — Integración de componentes
    - Arquitectura de la solución completa
    - Podman Compose para todo el stack
    - Interacción de red
    - Gestión de secretos
    - CI/CD con GitLab

13. **[chapter-13-production.md](../chapter-13-production.md)** — Prácticas listas para producción
    - Logging y rotación
    - Health checks
    - Estrategia de backup
    - Actualización de componentes
    - Troubleshooting

---

## 🌍 Multilingüismo

El curso soporta tres idiomas a través del mecanismo estándar de Docsify:

### Estructura de URL:

```
/                          → Versión en ruso (por defecto)
/en/                       → English version
/es/                       → Versión en Español

/chapter-01-architecture   → Capítulo 1 en ruso
/en/chapter-01-architecture → Chapter 1 en inglés (fallback a ruso si no está traducido)
/es/chapter-01-architecture → Capítulo 1 en español (fallback a ruso si no está traducido)
```

### Sistema de Fallback:

Si la página no se encuentra en el idioma seleccionado, se carga automáticamente la versión en ruso (idioma por defecto).

**Configuración:**
```javascript
fallbackLanguages: ['en', 'es']
```

---

## 📊 Estadísticas del Curso

**Total de archivos de documentación principal:** 18
- Capítulos del curso: 13
- Archivos principales: 3 (index.md, README.md, RESOURCES-AND-LINKS.md)
- Archivos de servicio: 2 (index.html, _sidebar.md, _coverpage.md)

**Multilingüismo:**
- Idiomas: 3 (ruso, inglés, español)
- Estructura según estándar Docsify
- Fallback al idioma ruso

**Volumen total:**
- Aproximadamente 40-50 horas de práctica
- 13 capítulos con teoría y práctica
- Numerosos ejemplos de código
- Tareas prácticas en cada capítulo
- Diagramas Mermaid para visualización

---

## 🚀 Inicio Rápido

### Para estudiantes del curso:

1. **Elija el idioma:**
   - [🇷🇺 Русский](/) (por defecto)
   - [🇬🇧 English](/en/)
   - [🇪🇸 Español](/es/)

2. **Comience el aprendizaje:**
   - Abra [index.md](index.md) para el contenido completo
   - O comience directamente con el [Capítulo 1](../chapter-01-architecture.md)

3. **Siga el curso:**
   - Estudie los capítulos secuencialmente
   - Complete las tareas prácticas
   - Use [RESOURCES-AND-LINKS.md](../RESOURCES-AND-LINKS.md) como referencia

4. **Cambio de idiomas:**
   - Use el selector 🌐 en la esquina superior derecha
   - El idioma se guarda automáticamente
   - Si falta traducción, se muestra la versión en ruso

### Para contribuidores:

**Estructura de archivos para agregar traducción:**

```bash
# Para agregar un nuevo idioma (por ejemplo, alemán):
docs/
└── de/                    # Crear carpeta con código de idioma
    ├── _coverpage.md      # Traducir portada
    ├── _sidebar.md        # Traducir menú
    ├── index.md           # Traducir página principal
    └── README.md          # Traducir descripción
```

**Luego actualizar `docs/index.html`:**
1. Agregar a `fallbackLanguages: ['en', 'es', 'de']`
2. Agregar a `nameLink`
3. Agregar opción en Language Dropdown
4. Actualizar lógica `detectAndSetLanguage()`

---

## 📝 Reglas de Formato

### Cada capítulo debe contener:

1. **Enlace de navegación al inicio:**
   ```markdown
   [← Índice del curso](index.md)
   ```

2. **Título del capítulo:**
   ```markdown
   # Capítulo X. Título
   ```

3. **Secciones con numeración:**
   ```markdown
   ## X.1. Título de la sección
   ```

4. **Diagramas Mermaid** (cuando aplique):
   ```markdown
   ```mermaid
   graph TD
       A[Inicio] --> B[Proceso]
   ```
   ```

5. **Tareas prácticas al final:**
   ```markdown
   ## Tarea Práctica
   ```

6. **Verificación de conocimientos:**
   ```markdown
   ## Verificación de Conocimientos
   ```

7. **Navegación al final:**
   ```markdown
   **Navegación:**
   - [← Volver al contenido](index.md)
   - [← Capítulo X: Título](chapter-XX.md)
   - [→ Capítulo Y: Título](chapter-YY.md)
   ```

### Convenciones para enlaces:

- **Enlaces internos:** rutas relativas (`chapter-01-architecture.md`)
- **Enlaces al índice:** `index.md`
- **Enlaces a recursos:** `RESOURCES-AND-LINKS.md`
- **Enlaces externos:** URLs completas con https://
- **Enlaces entre idiomas:** `../chapter-XX.md` (fallback a ruso)

---

## 🛠️ Tecnologías de Documentación

**Generador del sitio:**
- [Docsify](https://docsify.js.org/) — generador de documentación sin compilación

**Estilización:**
- Docsify Themeable
- Estilos CSS personalizados
- Tema oscuro/claro

**Funcionalidad:**
- Multilingüismo (fallbackLanguages)
- Búsqueda en documentación
- Copia de código
- Zoom de imágenes
- Diagramas Mermaid
- Paginación
- Widget de retroalimentación

**Hosting:**
- GitHub Pages
- Despliegue automático desde rama dev

---

## 📞 Retroalimentación

### Formas de contacto:

1. **Issues en GitHub:**
   - [🐛 Reportar error](https://github.com/Alex-0293/TopWebStack/issues/new?labels=bug)
   - [💡 Sugerir mejora](https://github.com/Alex-0293/TopWebStack/issues/new?labels=enhancement)
   - [👍 Dejar comentario](https://github.com/Alex-0293/TopWebStack/issues/new?labels=feedback)

2. **Discussions:**
   - [💬 Discusiones](https://github.com/Alex-0293/TopWebStack/discussions)

3. **Pull Requests:**
   - Haga fork del repositorio
   - Realice cambios
   - Cree un PR

### Ayuda al proyecto:

- ⭐ Dé una estrella en [GitHub](https://github.com/Alex-0293/TopWebStack)
- 🌍 Ayude con traducción a otros idiomas
- 📝 Mejore la documentación
- 🐛 Reporte errores
- 💬 Comparta experiencia en Discussions

---

**Versión del curso:** 1.1  
**Fecha de actualización:** Octubre 2025  
**Estado:** ✅ Desarrollo activo  
**Multilingüismo:** 🇷🇺 🇬🇧 🇪🇸

**[→ Comenzar aprendizaje](index.md)** | **[→ Lista de recursos](../RESOURCES-AND-LINKS.md)**
