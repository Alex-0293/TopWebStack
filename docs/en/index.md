# 🚀 TOP WEB STACK
## Modern web technology stack. Container development and deployment on AlmaLinux 9

## Preface

This course is designed for developers who want to master the modern container development stack based on enterprise-ready technologies. You will learn to deploy, configure, and integrate components of production-ready infrastructure.

**Target audience:**
- Backend developers with Node.js experience
- DevOps engineers transitioning to Podman
- System administrators learning containerization
- Fullstack developers working with microservices

**What you'll get:**
- Practical skills with Podman 5.4
- Experience deploying Node.js 26 LTS applications
- PostgreSQL 18 setup in containers
- Monitoring integration via SigNoz
- Centralized authentication with Zitadel IAM
- Production-ready infrastructure

**Requirements:**
- macOS 16+ with VSCode installed
- Basic Linux and command line knowledge
- Understanding of HTTP/REST API
- Experience with Git

**Duration:** 40-50 hours of practice

---

## Table of Contents

### [Chapter 1. Development Environment Architecture](../chapter-01-architecture.md)
- 1.1. Installing VSCode on macOS
- 1.2. Installing necessary VSCode extensions
- 1.3. Setting up SSH access to AlmaLinux server
- 1.4. Connecting VSCode to remote server
- 1.5. Setting up environment on remote server
- 1.6. Creating project structure on server
- 1.7. Configuring Dev Container for development
- 1.8. Configuring VSCode for productive work
- 1.9. First commit

### [Chapter 2. Developer Workstation Setup](../chapter-02-setup.md)
- 2.1. What is AlmaLinux
- 2.2. Installation and initial configuration
- 2.3. Package management via DNF
- 2.4. Firewalld and network security
- 2.5. Systemd: service management
- 2.6. Cockpit: web interface for server management

### [Chapter 3. AlmaLinux and System Basics](../chapter-03-almalinux.md)
- 3.1. What is Podman and how it differs from Docker
- 3.2. Installing Podman 5.4
- 3.3. Basic Podman commands
- 3.4. Working with images and containers
- 3.5. Podman Compose: container orchestration
- 3.6. Rootless containers and security

### [Chapter 4. Podman: Containerization Without Docker](../chapter-04-podman.md)
- 4.1. What is Git and why you need it
- 4.2. Installing and initial Git setup
- 4.3. Git basics
- 4.4. Working with remote repositories
  - SSH keys for GitHub/GitLab
  - Fine-grained Personal Access Tokens
  - Remote repositories and synchronization
- 4.5. Git Workflow for our project
- 4.6. .gitignore for our stack
- 4.7. Git hooks and automation
- 4.8. Git integration with Komodo
- 4.9. Practical scenarios and troubleshooting
- 4.10. Practical assignment
- 4.11. Knowledge check and conclusion

### [Chapter 5. Git: Version Control System](../chapter-05-git.md)
- 5.1. What is Komodo
- 5.2. Installing Komodo
- 5.3. Adding servers to Komodo
- 5.4. Working with Stacks (Docker Compose)
- 5.5. Deployments: Git-based deployment
- 5.6. Builds: building Docker images
- 5.7. Actions and Procedures: automation
- 5.8. Alerts and monitoring

### [Chapter 6. Komodo: Infrastructure and Deployment Management](../chapter-06-komodo.md)
- 6.1. Node.js 26 LTS: what's new
- 6.2. Installing Node.js via NVM
- 6.3. Creating a simple API with Fastify
- 6.4. Containerizing Node.js application
- 6.5. Multi-stage builds for optimization

### [Chapter 7. Node.js: Modern Development](../chapter-07-nodejs.md)
- 7.1. Vite: why you need it and how it works
- 7.2. Creating React application with Vite
- 7.3. Frontend containerization
- 7.4. Nginx for production

### [Chapter 8. Vite: Modern Frontend](../chapter-08-vite.md)
- 8.1. PostgreSQL 18: new features
- 8.2. Deploying PostgreSQL in container
- 8.3. Database initialization
- 8.4. Connecting from Node.js via Prisma
- 8.5. Backup and recovery

### [Chapter 9. PostgreSQL: Reliable Data Storage](../chapter-09-postgresql.md)
- 9.1. What is SigNoz and OpenTelemetry
- 9.2. Deploying SigNoz stack
- 9.3. Integration with Node.js application
- 9.4. Configuring dashboards and alerts
- 9.5. Analyzing traces and metrics

### [Chapter 10. SigNoz: Monitoring and Observability](../chapter-10-signoz.md)
- 10.1. Zitadel: modern IAM
- 10.2. Deploying Zitadel
- 10.3. Configuring organizations and projects
- 10.4. OIDC/OAuth2 integration
- 10.5. Connecting to Node.js via Lucia Auth

### [Chapter 11. Zitadel IAM: Centralized Authentication](../chapter-11-zitadel.md)
- 11.1. Zitadel: modern IAM
- 11.2. Deploying Zitadel
- 11.3. Configuring organizations and projects
- 11.4. OIDC/OAuth2 integration
- 11.5. Connecting to Node.js via Lucia Auth

### [Chapter 12. Integrating All Components](../chapter-12-integration.md)
- 12.1. Final solution architecture
- 12.2. Podman Compose for entire stack
- 12.3. Container network communication
- 12.4. Secrets management
- 12.5. CI/CD with GitLab

### [Chapter 13. Production-Ready Practices](../chapter-13-production.md)
- 13.1. Logging and log rotation
- 13.2. Health checks and monitoring
- 13.3. Backup strategy
- 13.4. Component updates
- 13.5. Troubleshooting common issues


## Conclusion

Congratulations on completing the course! You now have all the necessary knowledge to develop and deploy modern container applications based on AlmaLinux 9 and Podman 5.

**What's next:**

1. **Practice** — create your own project from scratch
2. **Study** — deepen your knowledge in technologies of interest
3. **Share** — help other developers
4. **Grow** — follow technology updates

**Recommended next steps:**

- Learn Kubernetes for large-scale orchestration
- Master Terraform for Infrastructure as Code
- Dive into security best practices
- Study advanced observability patterns
- Set up full-fledged CI/CD pipeline

**Useful links for continued learning:**

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [CNCF Landscape](https://landscape.cncf.io/)
- [DevOps Roadmap](https://roadmap.sh/devops)
- [SRE Books](https://sre.google/books/)

---

**Happy developing!** 🚀

---

**Course Version:** 1.1  
**Publication Date:** October 2025  
**Feedback:** Create an issue in the course repository

---

## Feedback

Your opinion is very important for improving the course! Share your impressions, suggestions, or report errors.

<div class="feedback-widget">
  <h4>💬 Help improve TopWebStack course</h4>
  <p>Share your experience taking the course, suggest improvements, or report found errors.</p>
  <div class="feedback-buttons">
    <a href="https://github.com/Alex-0293/TopWebStack/issues/new?assignees=&labels=feedback%2C+positive&template=&title=%F0%9F%91%8D+Helpful+Course&body=**What+I+liked%3A**%0A%0A**Suggestions+for+improvement%3A**%0A%0A" 
       class="feedback-btn positive" target="_blank" rel="noopener">
       👍 Helpful Course
    </a>
    <a href="https://github.com/Alex-0293/TopWebStack/issues/new?assignees=&labels=bug%2C+needs-review&template=&title=%F0%9F%90%9B+Bug+in+Course&body=**Chapter%2FSection%3A**%0A%0A**Error+description%3A**%0A%0A**Expected+behavior%3A**%0A%0A" 
       class="feedback-btn negative" target="_blank" rel="noopener">
       🐛 Found a Bug
    </a>
    <a href="https://github.com/Alex-0293/TopWebStack/issues/new?assignees=&labels=enhancement%2C+suggestion&template=&title=%F0%9F%92%A1+Suggestion&body=**Suggestion%3A**%0A%0A**Justification%3A**%0A%0A**Additional+details%3A**%0A%0A" 
       class="feedback-btn suggestion" target="_blank" rel="noopener">
       💡 Have an Idea
    </a>
    <a href="https://github.com/Alex-0293/TopWebStack/discussions" 
       class="feedback-btn github" target="_blank" rel="noopener">
       💬 Discussions
    </a>
  </div>
  <p style="margin-top: 1rem; font-size: 0.85rem; color: #666;">
    ⭐ Star us on <a href="https://github.com/Alex-0293/TopWebStack" target="_blank" rel="noopener">GitHub</a> if the course was helpful!
  </p>
</div>
