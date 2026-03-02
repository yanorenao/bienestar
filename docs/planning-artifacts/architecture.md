---
stepsCompleted: [1, 2, 3, 4, 5, 6, 7, 8]
workflowType: 'architecture'
lastStep: 8
status: 'complete'
completedAt: '2026-03-02'
---

# System Architecture: Plataforma Web de Bienestar Financiero (prueba-bienestar)

## 1. Executive Summary & Context
La Plataforma Web de Bienestar Financiero es una solución diseñada para ayudar a jóvenes profesionales en Colombia a gestionar, entender y liquidar sus deudas. Este documento define la arquitectura técnica para el Minimum Viable Product (MVP), el cual se enfoca en el registro manual de deudas, visualización mediante un "termómetro de deudas", educación financiera libre de juicios (Centro de Tips) y la generación inmediata de reportes de plan de pagos en PDF. La arquitectura está diseñada para ser segura, precisa en sus operaciones financieras y escalable hacia un futuro modelo corporativo B2B2C.

## 2. Architecture Decisions (Tech Stack)
- **Backend Framework:** Django (Python) - Seleccionado por su robustez, seguridad integrada y su panel de administración nativo (ideal para el CMS ligero de Tips).
- **Base de Datos:** PostgreSQL - Seleccionado por su integridad referencial, soporte sólido para transacciones financieras y escalabilidad.
- **Paradigma de UI:** Server-Side Rendering (SSR) con plantillas nativas de Django, potenciado con TailwindCSS o Bootstrap para diseño rápido, y Vanilla JS para interactividad ligera (Polling).
- **Tareas Asíncronas:** Celery + Redis - Esencial para descargar procesos pesados del hilo principal de las vistas, específicamente la generación de archivos.
- **Motor de Reportes (PDF):** WeasyPrint - Convierte plantillas HTML/CSS en documentos PDF, permitiendo reusar componentes visuales.
- **Infraestructura Base:** Docker y Cookiecutter Django - Provee un andamiaje estandarizado, contenerizado y preparado para un despliegue sin fricciones (CI/CD).

## 3. Implementation Patterns & Consistency Rules

### Naming & Structure Patterns
**Estándar Estricto Django:**
- **Apps & Archivos:** `snake_case` (ej. `debt_manager`, `services.py`).
- **Modelos:** `PascalCase` y en **singular** (ej. `Debt`, `PaymentPlan`). Prohibido nombrar modelos en plural.
- **Campos de Base de Datos:** `snake_case`. Las llaves foráneas deben llevar el sufijo automático implícito o explícito `_id` (ej. `user_id`, no `userId`).
- **Vistas y URLs:** Basadas en clases (CBV) usando el sufijo `View` (ej. `DebtDashboardView`). URLs en `kebab-case` (ej. `/debt-dashboard/`).
- **Plantillas (Templates):** Deben residir bajo el namespace de la app: `app_name/templates/app_name/model_action.html`.

### Data Access & Security Patterns
**Aislamiento Multi-tenant (Uso Exclusivo de Custom Managers):**
- **Regla Estricta:** Ningún modelo que contenga datos financieros o información de usuarios puede ser consultado directamente usando el manager por defecto `objects.all()`.
- **Implementación:** Todos los modelos sensibles deben implementar un Custom Manager (`TenantManager`) que obligue a aplicar un filtro basado en el usuario (ej. `def for_user(self, user): return super().get_queryset().filter(user=user)`).
- **Validación en Vistas:** Toda vista debe sobreescribir el método `get_queryset()` para consumir exclusivamente el `TenantManager`.
- **Precisión Financiera (Cero Tolerancia):** Se requiere el uso obligatorio de `decimal.Decimal` en lógica de Python y `DecimalField` en la base de datos para dinero y porcentajes. Está prohibido el uso de `float` o `FloatField`.

### Process & Communication Patterns
**Comunicación Asíncrona (Estado en Base de Datos / Polling):**
- **Contexto:** La generación de PDFs con WeasyPrint y Celery es un proceso asíncrono.
- **Patrón a Implementar:** El frontend se comunicará mediante **Polling** (no WebSockets en el MVP).
- **Flujo:** 
  1. El usuario solicita descargar su mapa de ruta (PDF).
  2. El backend crea un registro `DocumentTask` en base de datos con estado `PENDING` y devuelve un `task_id` al navegador.
  3. El worker asíncrono de Celery procesa los cálculos, compila el HTML, genera el PDF y actualiza el estado a `COMPLETED`.
  4. El frontend realiza peticiones periódicas (polling) a un endpoint ligero con su `task_id`.
  5. Al recibir el estado `COMPLETED`, el frontend detiene el polling y descarga automáticamente el documento.

## 4. Project Structure & Boundaries

### Complete Project Directory Structure
```text
bienestar/
├── .envs/                    # Variables de entorno por ambiente
├── .github/                  # CI/CD Workflows y Configuración BMAD
├── compose/                  # Configuraciones de Docker (Django, Postgres, Redis, Celery)
├── config/                   # Configuración global de Django (settings, urls, wsgi/asgi)
├── docs/                     # BMAD Project Knowledge y Artefactos
├── requirements/             # Dependencias base, local y prod
│
├── bienestar/                # Código fuente principal
│   ├── static/               # CSS global, imágenes, JS
│   ├── templates/            # Plantillas globales (base.html, componentes compartidos)
│   │
│   ├── users/                # [APP] Autenticación, Perfil y Auditoría
│   │   ├── models.py         # Custom User, Auditoría Ley 1581 Opt-in
│   │   ├── forms.py          # Formularios de registro
│   │   └── views.py
│   │
│   ├── debts/                # [APP] Core Financiero
│   │   ├── models.py         # Debt, Payment, Goal
│   │   ├── managers.py       # Aislamiento: TenantManager
│   │   ├── services.py       # Lógica financiera y algoritmos (Bola de Nieve)
│   │   └── views.py          # Dashboard Termómetro, Formularios de ingreso
│   │
│   ├── education/            # [APP] Centro de Educación Financiera
│   │   ├── models.py         # Tip, Category
│   │   ├── admin.py          # CMS ligero para el equipo de contenido
│   │   └── views.py          # Visualización de micro-lecciones
│   │
│   └── reports/              # [APP] Motor de Generación PDF y Tareas
│       ├── tasks.py          # Celery Workers (WeasyPrint)
│       ├── models.py         # DocumentTask (para Polling)
│       ├── views.py          # Endpoints API para polling y descarga
│       └── templates/reports/# Diseño HTML/CSS exclusivo del PDF impreso
│
├── docker-compose.local.yml
├── manage.py
└── README.md
```

### Architectural Boundaries
- **Aislamiento de Lógica Compleja:** Las vistas (`views.py`) de la app `debts` actúan solo como controladores HTTP. La lógica matemática del método bola de nieve se delega a `debts/services.py` para permitir un testing exhaustivo e independiente por el equipo de QA.
- **Protección de Workers:** La app `reports` (específicamente Celery) debe realizar todas sus consultas de bases de datos utilizando los Custom Managers permitidos, y jamás debe bloquear los hilos principales de WSGI/ASGI.

## 5. Architecture Validation Results

- **Coherencia (Coherence):** ✅ El stack (Django + PostgreSQL + Celery) forma un ecosistema maduro y probado, 100% compatible con el enfoque *Server-Side Rendering*.
- **Cobertura de Requerimientos (Coverage):** ✅ Todas las historias principales del PRD están mapeadas: el ingreso de saldos (`debts`), el CMS para Laura (`education`), el reporte para Camila (`reports`), y el cumplimiento de la Ley 1581 (`users`).
- **Seguridad y Precisión:** ✅ El uso mandatario de `TenantManager` mitiga fugas de datos de inquilinos cruzados. El uso de `Decimal` evita errores de punto flotante en las proyecciones.
- **Gap Analysis (Brechas Menores):** Como medida adicional de calidad, el equipo debe configurar un linter estricto (`ruff` o `flake8`) en el pipeline de CI/CD que bloquee Pull Requests si detecta la palabra clave `float` en modelos o servicios financieros.

## 6. Implementation Handoff

**Estado de la Arquitectura:** READY FOR IMPLEMENTATION 🚀

**AI Agent Guidelines (Para agentes de desarrollo):**
1. Siga todas las decisiones arquitectónicas de este documento.
2. Comience la implementación generando el andamiaje del proyecto con `cookiecutter-django`.
3. Inicie el ciclo de desarrollo por la app `users` (Custom User y Ley 1581) antes de proceder con el Core Financiero (`debts`).
