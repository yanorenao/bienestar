# Story 1.1: Configuración del Proyecto y Andamiaje Inicial

Status: ready-for-dev

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a desarrollador,
I want configurar el proyecto base usando el starter template de Cookiecutter Django,
so that el equipo cuente con un andamiaje estandarizado, contenerizado (Docker) y preparado para CI/CD.

## Acceptance Criteria

1. **Given** la necesidad de iniciar el proyecto
   **When** se ejecuta el comando de creación
   **Then** se debe generar la estructura base con Django, PostgreSQL, Celery y Redis usando Docker
   **And** el proyecto debe levantar correctamente en el entorno local

## Tasks / Subtasks

- [ ] Task 1: Inicializar proyecto con Cookiecutter Django (AC: 1)
  - [ ] Subtask 1.1: Ejecutar comando de cookiecutter para Django con opciones de PostgreSQL, Celery y Redis.
  - [ ] Subtask 1.2: Configurar variables de entorno locales en `.envs/`.
- [ ] Task 2: Configurar orquestación con Docker Compose (AC: 1)
  - [ ] Subtask 2.1: Verificar configuración de `docker-compose.local.yml`.
  - [ ] Subtask 2.2: Levantar y probar servicios localmente (`django`, `postgres`, `redis`, `celery`).

## Dev Notes

- **Technical Stack:** Backend en Django (Python), PostgreSQL, Celery + Redis para asincronismo, Docker para infraestructura base.
- **Code Structure:** Estándar estricto de Django. Apps y archivos en `snake_case`, modelos en `PascalCase` singular. Plantillas bajo el namespace de la app.
- **Security & Data:** (Para futuras apps) Uso de Custom Managers (`TenantManager`) y uso obligatorio de `decimal.Decimal` para montos financieros.

### Project Structure Notes

- Alignment with unified project structure (paths, modules, naming):
  - Debe respetarse la estructura definida en `docs/planning-artifacts/architecture.md` (ej. `/config`, `/compose`, `/bienestar`, `/requirements`).

### References

- [Source: docs/planning-artifacts/architecture.md#2. Architecture Decisions (Tech Stack)]
- [Source: docs/planning-artifacts/epics.md#Story 1.1: Configuración del Proyecto y Andamiaje Inicial]

## Dev Agent Record

### Agent Model Used

Copilot Agent

### Debug Log References

### Completion Notes List

### File List
