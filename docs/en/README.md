# 🚀 TOP WEB STACK
## Modern web technology stack. Container development and deployment on AlmaLinux 9

## 📖 About the Course

This course provides practical training in modern web development using enterprise-ready technologies and containerization.

### 🎯 What's covered:

- **🐧 AlmaLinux 9** — Enterprise-ready Linux distribution
- **📦 Podman 5.4** — Containerization without Docker
- **🚀 Node.js 26 LTS + Fastify** — Fast backend framework
- **⚡ Vite 6 + React** — Lightning-fast frontend build
- **🗄️ PostgreSQL 18 + Prisma** — Modern ORM and SQL database
- **📊 SigNoz + OpenTelemetry** — Application monitoring and tracing
- **🔐 Zitadel IAM** — User management and authentication
- **🧩 Komodo** — Infrastructure and deployment management

### 📚 Course Structure:

A comprehensive course including 13 chapters covering everything from infrastructure setup to production deployment.

---

## 📁 Project Structure

### Multi-language Support

The course is available in three languages following Docsify standards:

```
docs/
├── index.html           # Single entry point
├── _coverpage.md        # Russian coverpage
├── _sidebar.md          # Russian menu
├── index.md             # Russian homepage
├── README.md            # This file
├── chapter-*.md         # Russian chapters (default)
│
├── en/                  # 🇬🇧 English version
│   ├── _coverpage.md
│   ├── _sidebar.md
│   ├── index.md
│   └── README.md
│
└── es/                  # 🇪🇸 Spanish version  
    ├── _coverpage.md
    ├── _sidebar.md
    ├── index.md
    └── README.md
```

### Main Files

- **[index.md](index.md)** — course homepage with full content
- **[README.md](README.md)** — course description and quick start (this file)
- **[RESOURCES-AND-LINKS.md](RESOURCES-AND-LINKS.md)** — all resources, links and documentation
- **[_sidebar.md](_sidebar.md)** — navigation sidebar
- **[_coverpage.md](_coverpage.md)** — course cover page

### Course Chapters

1. **[chapter-01-architecture.md](../chapter-01-architecture.md)** — Development Environment Architecture
   - Future project architecture
   - Infrastructure planning
   - Technology stack selection
   - Interaction diagrams

2. **[chapter-02-setup.md](../chapter-02-setup.md)** — Developer Workstation Setup
   - Installing VSCode on macOS
   - SSH and Remote Development setup
   - Creating a project on remote server
   - Dev Container configuration

3. **[chapter-03-almalinux.md](../chapter-03-almalinux.md)** — AlmaLinux and System Basics
   - What is AlmaLinux
   - Installation and configuration
   - DNF, Firewalld, Systemd
   - Cockpit web interface

4. **[chapter-04-podman.md](../chapter-04-podman.md)** — Podman: Containerization without Docker
   - Podman vs Docker architecture
   - Installation and configuration
   - Basic commands
   - Podman Compose
   - Rootless containers

5. **[chapter-05-git.md](../chapter-05-git.md)** — Git: Version Control System
   - Git basics
   - Setup and configuration
   - Git workflow and best practices
   - GitHub/GitLab integration

6. **[chapter-06-komodo.md](../chapter-06-komodo.md)** — Komodo: Infrastructure Management
   - Installing Komodo Core
   - Server management
   - Stacks and Deployments
   - Builds and automation
   - Monitoring and alerts

7. **[chapter-07-nodejs.md](../chapter-07-nodejs.md)** — Node.js: Modern Development
   - Installing Node.js via NVM
   - Creating API with Fastify
   - Application containerization
   - Multi-stage builds

8. **[chapter-08-vite.md](../chapter-08-vite.md)** — Vite: Modern Frontend
   - Creating React application
   - Frontend containerization
   - Nginx for production
   - Build optimization

9. **[chapter-09-postgresql.md](../chapter-09-postgresql.md)** — PostgreSQL: Reliable Storage
   - Container deployment
   - Database initialization
   - Prisma integration
   - Backup and recovery

10. **[chapter-10-signoz.md](../chapter-10-signoz.md)** — SigNoz: Monitoring and Observability
    - SigNoz architecture
    - Stack deployment
    - Node.js integration
    - OpenTelemetry
    - Dashboards and alerts

11. **[chapter-11-zitadel.md](../chapter-11-zitadel.md)** — Zitadel IAM: Authentication
    - Deploying Zitadel
    - Organization setup
    - OIDC/OAuth2 integration
    - Connection via Lucia Auth

12. **[chapter-12-integration.md](../chapter-12-integration.md)** — Component Integration
    - Full solution architecture
    - Podman Compose for entire stack
    - Network interaction
    - Secrets management
    - CI/CD with GitLab

13. **[chapter-13-production.md](../chapter-13-production.md)** — Production-ready Practices
    - Logging and rotation
    - Health checks
    - Backup strategy
    - Component updates
    - Troubleshooting

---

## 🌍 Multi-language Support

The course supports three languages through standard Docsify mechanism:

### URL Structure:

```
/                          → Russian version (default)
/en/                       → English version
/es/                       → Spanish version

/chapter-01-architecture   → Russian Chapter 1
/en/chapter-01-architecture → English Chapter 1 (fallback to Russian if not translated)
/es/chapter-01-architecture → Spanish Chapter 1 (fallback to Russian if not translated)
```

### Fallback System:

If a page is not found in the selected language, the Russian version (default language) is automatically loaded.

**Configuration:**
```javascript
fallbackLanguages: ['en', 'es']
```

---

## 🔗 Link Verification

### Internal Course Links

**Main Navigation:**
- ✅ [index.md](index.md) — homepage
- ✅ [README.md](README.md) — course description (this file)
- ✅ [RESOURCES-AND-LINKS.md](../RESOURCES-AND-LINKS.md) — resources
- ✅ [_sidebar.md](_sidebar.md) — sidebar
- ✅ [_coverpage.md](_coverpage.md) — cover page

**Multi-language Versions:**
- ✅ [🇷🇺 Russian version](/) — root (default)
- ✅ [🇬🇧 English version](/en/) — English version
- ✅ [🇪🇸 Spanish version](/es/) — Spanish version

**Chapters (sequence):**
- ✅ [Chapter 1](../chapter-01-architecture.md) → [Chapter 2](../chapter-02-setup.md)
- ✅ [Chapter 2](../chapter-02-setup.md) → [Chapter 3](../chapter-03-almalinux.md)
- ✅ [Chapter 3](../chapter-03-almalinux.md) → [Chapter 4](../chapter-04-podman.md)
- ✅ [Chapter 4](../chapter-04-podman.md) → [Chapter 5](../chapter-05-git.md)
- ✅ [Chapter 5](../chapter-05-git.md) → [Chapter 6](../chapter-06-komodo.md)
- ✅ [Chapter 6](../chapter-06-komodo.md) → [Chapter 7](../chapter-07-nodejs.md)
- ✅ [Chapter 7](../chapter-07-nodejs.md) → [Chapter 8](../chapter-08-vite.md)
- ✅ [Chapter 8](../chapter-08-vite.md) → [Chapter 9](../chapter-09-postgresql.md)
- ✅ [Chapter 9](../chapter-09-postgresql.md) → [Chapter 10](../chapter-10-signoz.md)
- ✅ [Chapter 10](../chapter-10-signoz.md) → [Chapter 11](../chapter-11-zitadel.md)
- ✅ [Chapter 11](../chapter-11-zitadel.md) → [Chapter 12](../chapter-12-integration.md)
- ✅ [Chapter 12](../chapter-12-integration.md) → [Chapter 13](../chapter-13-production.md)

**Navigation in chapters:**
- Each chapter has a link to [index.md](index.md) (table of contents)
- Each chapter has links to previous and next chapters
- The last chapter has a link only to the previous chapter

### External Links (examples)

**Official Documentation:**
- AlmaLinux: https://almalinux.org/
- Podman: https://podman.io/
- Komodo: https://komo.do/docs
- Node.js: https://nodejs.org/
- Vite: https://vitejs.dev/
- PostgreSQL: https://www.postgresql.org/
- Prisma: https://www.prisma.io/
- Fastify: https://fastify.dev/
- SigNoz: https://signoz.io/
- Zitadel: https://zitadel.com/

Full list of external links see in [RESOURCES-AND-LINKS.md](../RESOURCES-AND-LINKS.md)

---

## 📊 Course Statistics

**Total files:** 15+
- Course chapters: 13
- Main files: 3
- Service files: 1 (index.html)
- Language versions: 3 (ru, en, es)

**Total volume:**
- Approximately 40-50 hours of practice
- 13 chapters with theory and practice
- Multiple code examples
- Practical assignments in each chapter
- Mermaid diagrams for visualization

---

## 🚀 Quick Start

1. Start with [README.md](README.md) for general overview
2. Open [index.md](index.md) for full content
3. Follow chapters sequentially, starting with [Chapter 1](../chapter-01-architecture.md)
4. Complete practical assignments
5. Use [RESOURCES-AND-LINKS.md](../RESOURCES-AND-LINKS.md) as reference

---

## 📝 For Authors and Contributors

### File Structure

```
docs/
├── index.html                    # Single entry point for all languages
├── _coverpage.md                 # Russian coverpage
├── _sidebar.md                   # Russian sidebar
├── index.md                      # Russian homepage
├── README.md                     # Russian course description
├── RESOURCES-AND-LINKS.md        # All resources and links
├── chapter-01-architecture.md    # Chapter 1
├── chapter-02-setup.md           # Chapter 2
├── chapter-03-almalinux.md       # Chapter 3
├── chapter-04-podman.md          # Chapter 4
├── chapter-05-git.md             # Chapter 5
├── chapter-06-komodo.md          # Chapter 6
├── chapter-07-nodejs.md          # Chapter 7
├── chapter-08-vite.md            # Chapter 8
├── chapter-09-postgresql.md      # Chapter 9
├── chapter-10-signoz.md          # Chapter 10
├── chapter-11-zitadel.md         # Chapter 11
├── chapter-12-integration.md     # Chapter 12
├── chapter-13-production.md      # Chapter 13
│
├── en/                           # English version
│   ├── _coverpage.md
│   ├── _sidebar.md
│   ├── index.md
│   └── README.md
│
└── es/                           # Spanish version
    ├── _coverpage.md
    ├── _sidebar.md
    ├── index.md
    └── README.md
```

### Formatting Rules

**Each chapter should contain:**
1. Link to table of contents at the beginning: `[← Course Table of Contents](index.md)`
2. Chapter title: `# Chapter X. Title`
3. Sections with numbering: `## X.1. Section Name`
4. Mermaid diagrams where applicable
5. Practical assignments at the end
6. Knowledge check
7. Navigation at the end:
   ```markdown
   **Navigation:**
   - [← Back to Contents](index.md)
   - [← Chapter X: Title](chapter-XX.md)
   - [→ Chapter Y: Title](chapter-YY.md)
   ```

### Link Conventions

- Internal links: relative paths (`../chapter-01-setup.md` from language folders)
- Links to table of contents: `index.md` (or `../index.md` from language folders)
- Links to resources: `RESOURCES-AND-LINKS.md` (or `../RESOURCES-AND-LINKS.md`)
- External links: full URLs with https://

### Translation Status

**Russian (default):** ✅ Complete
- All 13 chapters
- All main files
- Full documentation

**English:** 🔄 In Progress
- Main pages translated
- Chapters: fallback to Russian
- Translations welcome!

**Spanish:** 🔄 In Progress
- Main pages translated
- Chapters: fallback to Russian
- ¡Se aceptan traducciones!

---

## 🤝 Contributing

We welcome contributions in:
- 📝 Content improvements
- 🌍 Translations (especially chapter translations to English/Spanish)
- 🐛 Bug reports
- 💡 Suggestions and ideas

**To contribute:**
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

## 📞 Support and Feedback

- **GitHub Issues:** [Report bugs or suggest features](https://github.com/Alex-0293/TopWebStack/issues)
- **GitHub Discussions:** [Ask questions and discuss](https://github.com/Alex-0293/TopWebStack/discussions)
- **Give us a star:** ⭐ [Star on GitHub](https://github.com/Alex-0293/TopWebStack)

---

**Version:** 1.1  
**Last Updated:** October 2025  
**Status:** ✅ All links verified and working  
**Multi-language:** 🌍 3 languages supported (Russian, English, Spanish)
