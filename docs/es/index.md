# 🚀 TOP WEB STACK
## Pila de tecnologías web moderna. Desarrollo y despliegue en contenedores con AlmaLinux 9

## Prefacio

Este curso está diseñado para desarrolladores que desean dominar la pila moderna de desarrollo en contenedores basada en tecnologías listas para empresas. Aprenderás a implementar, configurar e integrar componentes de infraestructura listos para producción.

**Audiencia objetivo:**
- Desarrolladores Backend con experiencia en Node.js
- Ingenieros DevOps que migran a Podman
- Administradores de sistemas que estudian contenedorización
- Desarrolladores Fullstack que trabajan con microservicios

**Lo que obtendrás:**
- Habilidades prácticas con Podman 5.4
- Experiencia en el despliegue de aplicaciones Node.js 26 LTS
- Configuración de PostgreSQL 18 en contenedores
- Integración de monitoreo a través de SigNoz
- Autenticación centralizada con Zitadel IAM
- Infraestructura lista para producción

**Requisitos:**
- macOS 16+ con VSCode instalado
- Conocimientos básicos de Linux y línea de comandos
- Comprensión de HTTP/REST API
- Experiencia trabajando con Git

**Duración:** 40-50 horas de práctica

---

## Contenido

### [Capítulo 1. Arquitectura del entorno de desarrollo](../chapter-01-architecture.md)
- 1.1. Instalación de VSCode en macOS
- 1.2. Instalación de extensiones necesarias de VSCode
- 1.3. Configuración de acceso SSH al servidor AlmaLinux
- 1.4. Conexión de VSCode al servidor remoto
- 1.5. Configuración del entorno en el servidor remoto
- 1.6. Creación de la estructura del proyecto en el servidor
- 1.7. Configuración de Dev Container para desarrollo
- 1.8. Configuración de VSCode para trabajo productivo
- 1.9. Primer commit

### [Capítulo 2. Configuración del espacio de trabajo del desarrollador](../chapter-02-setup.md)
- 2.1. Qué es AlmaLinux
- 2.2. Instalación y configuración inicial
- 2.3. Gestión de paquetes a través de DNF
- 2.4. Firewalld y seguridad de red
- 2.5. Systemd: gestión de servicios
- 2.6. Cockpit: interfaz web para gestión del servidor

### [Capítulo 3. AlmaLinux y fundamentos del sistema](../chapter-03-almalinux.md)
- 3.1. Qué es Podman y en qué se diferencia de Docker
- 3.2. Instalación de Podman 5.4
- 3.3. Comandos básicos de Podman
- 3.4. Trabajo con imágenes y contenedores
- 3.5. Podman Compose: orquestación de contenedores
- 3.6. Contenedores Rootless y seguridad

### [Capítulo 4. Podman: contenedorización sin Docker](../chapter-04-podman.md)
- 4.1. Qué es Git y por qué es necesario
- 4.2. Instalación y configuración inicial de Git
- 4.3. Fundamentos de trabajo con Git
- 4.4. Trabajo con repositorios remotos
  - Claves SSH para GitHub/GitLab
  - Fine-grained Personal Access Tokens
  - Repositorios remotos y sincronización
- 4.5. Git Workflow para nuestro proyecto
- 4.6. .gitignore para nuestra pila
- 4.7. Git hooks y automatización
- 4.8. Integración de Git con Komodo
- 4.9. Escenarios prácticos y solución de problemas
- 4.10. Tarea práctica
- 4.11. Verificación de conocimientos y conclusión

### [Capítulo 5. Git: sistema de control de versiones](../chapter-05-git.md)
- 5.1. Qué es Komodo
- 5.2. Instalación de Komodo
- 5.3. Agregar servidores a Komodo
- 5.4. Trabajo con Stacks (Docker Compose)
- 5.5. Deployments: despliegue basado en Git
- 5.6. Builds: construcción de imágenes Docker
- 5.7. Actions y Procedures: automatización
- 5.8. Alerts y monitoreo

### [Capítulo 6. Komodo: gestión de infraestructura y despliegue](../chapter-06-komodo.md)
- 6.1. Node.js 26 LTS: novedades
- 6.2. Instalación de Node.js a través de NVM
- 6.3. Creación de una API simple con Fastify
- 6.4. Contenedorización de aplicación Node.js
- 6.5. Multi-stage builds para optimización

### [Capítulo 7. Node.js: desarrollo moderno](../chapter-07-nodejs.md)
- 7.1. Vite: por qué es necesario y cómo funciona
- 7.2. Creación de aplicación React con Vite
- 7.3. Contenedorización del frontend
- 7.4. Nginx para producción

### [Capítulo 8. Vite: frontend moderno](../chapter-08-vite.md)
- 8.1. PostgreSQL 18: nuevas características
- 8.2. Despliegue de PostgreSQL en contenedor
- 8.3. Inicialización de base de datos
- 8.4. Conexión desde Node.js a través de Prisma
- 8.5. Backup y restauración

### [Capítulo 9. PostgreSQL: almacenamiento confiable de datos](../chapter-09-postgresql.md)
- 9.1. Qué es SigNoz y OpenTelemetry
- 9.2. Despliegue del stack de SigNoz
- 9.3. Integración con aplicación Node.js
- 9.4. Configuración de dashboards y alertas
- 9.5. Análisis de trazas y métricas

### [Capítulo 10. SigNoz: monitoreo y observabilidad](../chapter-10-signoz.md)
- 10.1. Zitadel: IAM moderno
- 10.2. Despliegue de Zitadel
- 10.3. Configuración de organización y proyectos
- 10.4. Integración OIDC/OAuth2
- 10.5. Conexión a Node.js a través de Lucia Auth

### [Capítulo 11. Zitadel IAM: autenticación centralizada](../chapter-11-zitadel.md)
- 11.1. Zitadel: IAM moderno
- 11.2. Despliegue de Zitadel
- 11.3. Configuración de organización y proyectos
- 11.4. Integración OIDC/OAuth2
- 11.5. Conexión a Node.js a través de Lucia Auth

### [Capítulo 12. Integración de todos los componentes](../chapter-12-integration.md)
- 12.1. Arquitectura de la solución final
- 12.2. Podman Compose para toda la pila
- 12.3. Interacción de red de contenedores
- 12.4. Gestión de secretos
- 12.5. CI/CD con GitLab

### [Capítulo 13. Prácticas listas para producción](../chapter-13-production.md)
- 13.1. Registro y rotación de logs
- 13.2. Health checks y monitoreo
- 13.3. Estrategia de backup
- 13.4. Actualización de componentes
- 13.5. Solución de problemas típicos


## Conclusión

¡Felicidades por completar el curso! Ahora tienes todos los conocimientos necesarios para desarrollar e implementar aplicaciones en contenedores modernas basadas en AlmaLinux 9 y Podman 5.

**¿Qué sigue?**

1. **Practica** — crea tu propio proyecto desde cero
2. **Estudia** — profundiza en las tecnologías que te interesan
3. **Comparte** — ayuda a otros desarrolladores
4. **Desarrolla** — mantente al tanto de las actualizaciones tecnológicas

**Próximos pasos recomendados:**

- Estudia Kubernetes para orquestación a gran escala
- Domina Terraform para Infrastructure as Code
- Profundiza en security best practices
- Estudia advanced observability patterns
- Configura un pipeline de CI/CD completo

**Enlaces útiles para continuar el aprendizaje:**

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [CNCF Landscape](https://landscape.cncf.io/)
- [DevOps Roadmap](https://roadmap.sh/devops)
- [SRE Books](https://sre.google/books/)

---

**¡Éxito en el desarrollo!** 🚀

---

**Versión del curso:** 1.1  
**Fecha de publicación:** Octubre 2025  
**Comentarios:** Crea un issue en el repositorio del curso

---

## Comentarios

¡Tu opinión es muy importante para mejorar el curso! Comparte tus impresiones, sugerencias o reporta errores.

<div class="feedback-widget">
  <h4>💬 Ayuda a mejorar el curso TopWebStack</h4>
  <p>Comparte tu experiencia tomando el curso, propón mejoras o informa sobre errores encontrados.</p>
  <div class="feedback-buttons">
    <a href="https://github.com/Alex-0293/TopWebStack/issues/new?assignees=&labels=feedback%2C+positive&template=&title=%F0%9F%91%8D+Curso+%C3%BAtil&body=**Lo+que+me+gust%C3%B3%3A**%0A%0A**Sugerencias+de+mejora%3A**%0A%0A" 
       class="feedback-btn positive" target="_blank" rel="noopener">
       👍 Curso útil
    </a>
    <a href="https://github.com/Alex-0293/TopWebStack/issues/new?assignees=&labels=bug%2C+needs-review&template=&title=%F0%9F%90%9B+Error+en+el+curso&body=**Cap%C3%ADtulo%2Fsecci%C3%B3n%3A**%0A%0A**Descripci%C3%B3n+del+error%3A**%0A%0A**Comportamiento+esperado%3A**%0A%0A" 
       class="feedback-btn negative" target="_blank" rel="noopener">
       �� Encontré un error
    </a>
    <a href="https://github.com/Alex-0293/TopWebStack/issues/new?assignees=&labels=enhancement%2C+suggestion&template=&title=%F0%9F%92%A1+Sugerencia&body=**Sugerencia%3A**%0A%0A**Justificaci%C3%B3n%3A**%0A%0A**Detalles+adicionales%3A**%0A%0A" 
       class="feedback-btn suggestion" target="_blank" rel="noopener">
       💡 Tengo una idea
    </a>
    <a href="https://github.com/Alex-0293/TopWebStack/discussions" 
       class="feedback-btn github" target="_blank" rel="noopener">
       💬 Discusiones
    </a>
  </div>
  <p style="margin-top: 1rem; font-size: 0.85rem; color: #666;">
    ⭐ Dale una estrella en <a href="https://github.com/Alex-0293/TopWebStack" target="_blank" rel="noopener">GitHub</a> si el curso te fue útil!
  </p>
</div>
