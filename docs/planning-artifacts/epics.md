---
stepsCompleted: [1, 2, 3, 4]
inputDocuments: ["docs/planning-artifacts/prd.md", "docs/planning-artifacts/architecture.md"]
---

# prueba-bienestar - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for prueba-bienestar, decomposing the requirements from the PRD, UX Design if it exists, and Architecture requirements into implementable stories.

## Requirements Inventory

### Functional Requirements

FR1: Autenticación simple (Registro e inicio de sesión con email, opcional Google/Apple).
FR2: Ingreso manual de deudas a través de un formulario amigable (saldos, tasas, pagos mínimos), sin conexión bancaria.
FR3: Dashboard visual ("termómetro de deudas") con resumen de deuda y proyección de pagos.
FR4: Motor de generación y exportación de reportes PDF (estado de cuenta resumido / mapa de ruta).
FR5: Centro de Tips educativos con micro-lecciones financieras.
FR6: Proceso explícito de aceptación de Política de Tratamiento de Datos Personales (Ley 1581) durante el registro.
FR7: Panel de administración (CMS ligero con RBAC) para gestionar y publicar los tips educativos.
FR8: Sistema de ayuda contextual (tooltips, modales, imágenes) en los formularios de ingreso.
FR9: Soporte básico para escenarios en pareja (dashboard compartido).
FR10: Validación estricta de formularios financieros (ej. límites de tasas de usura).

### NonFunctional Requirements

NFR1: [Precisión] Cálculos financieros y proyecciones de intereses 100% exactos (uso de Decimal).
NFR2: [Rendimiento] Generación de reportes PDF rápida y asíncrona (Celery + Redis + Polling).
NFR3: [Seguridad - Reposo] Cifrado AES-256 para datos financieros en base de datos.
NFR4: [Seguridad - Aislamiento] Aislamiento de inquilinos (Tenant Model) estricto en BD.
NFR5: [Cumplimiento] Ley 1581 de protección de datos (Colombia).
NFR6: [Auditoría] Registro inmutable de fecha e IP al aceptar términos.

### Additional Requirements

- [Arquitectura] Starter Template: Uso de Docker y Cookiecutter Django para generar el andamiaje del proyecto.
- [Arquitectura] Paradigma UI: Server-Side Rendering (SSR) nativo + TailwindCSS/Bootstrap y Vanilla JS.
- [Arquitectura] Data Access: Uso exclusivo de Custom Managers (`TenantManager`) para modelos sensibles.
- [Arquitectura] Precisión: Uso obligatorio de `decimal.Decimal` en Python. Prohibido punto flotante.

### FR Coverage Map

FR1: Epic 1 - Autenticación simple y segura
FR2: Epic 2 - Formulario de ingreso de deudas
FR3: Epic 3 - Dashboard visual consolidado
FR4: Epic 4 - Exportación de reportes en PDF asíncronos
FR5: Epic 5 - Feed interactivo de tips educativos
FR6: Epic 1 - Aceptación explícita de Ley 1581
FR7: Epic 5 - Gestor de contenido educativo (CMS)
FR8: Epic 2 - Sistema de ayuda contextual
FR9: Epic 3 - Invitación y vinculación de pareja
FR10: Epic 2 - Validaciones de formularios financieros

## Epic List

### Epic 1: Onboarding Seguro y Cumplimiento Normativo
Permitir a los usuarios registrarse e iniciar sesión de manera sencilla, garantizando al mismo tiempo que el sistema cumpla con las leyes colombianas de protección de datos y que la infraestructura base esté correctamente configurada.
**FRs covered:** FR1, FR6

### Epic 2: Ingreso de Deudas y Empatía Educativa
Permitir a los usuarios registrar manualmente la información de sus deudas de forma segura, precisa y sin frustraciones, apoyándose en ayudas visuales contextuales y validaciones estrictas.
**FRs covered:** FR2, FR8, FR10

### Epic 3: Visualización de Salud Financiera y Colaboración
Proporcionar al usuario un "termómetro" claro de su panorama financiero general, e introducir la capacidad de vincular a su pareja para la gestión conjunta de deudas.
**FRs covered:** FR3, FR9

### Epic 4: Generación del "Mapa de Ruta" Tangible
Brindar tranquilidad al usuario permitiéndole descargar instantáneamente un reporte PDF con su plan de pagos, procesado asíncronamente.
**FRs covered:** FR4

### Epic 5: Centro de Educación Financiera y Panel de Contenido
Entregar micro-lecciones de educación financiera a los usuarios y dotar al equipo interno de un administrador CMS ligero y seguro.
**FRs covered:** FR5, FR7

## Epic 1: Onboarding Seguro y Cumplimiento Normativo

Permitir a los usuarios registrarse e iniciar sesión de manera sencilla, garantizando al mismo tiempo que el sistema cumpla con las leyes colombianas de protección de datos y que la infraestructura base esté correctamente configurada.

### Story 1.1: Configuración del Proyecto y Andamiaje Inicial

Como desarrollador,
Quiero configurar el proyecto base usando el starter template de Cookiecutter Django,
So that el equipo cuente con un andamiaje estandarizado, contenerizado (Docker) y preparado para CI/CD.

**Acceptance Criteria:**

**Given** la necesidad de iniciar el proyecto
**When** se ejecuta el comando de creación
**Then** se debe generar la estructura base con Django, PostgreSQL, Celery y Redis usando Docker
**And** el proyecto debe levantar correctamente en el entorno local

### Story 1.2: Registro de Cuenta y Aceptación de Política de Datos (Ley 1581)

Como nuevo usuario,
Quiero registrarme usando mi correo electrónico y aceptar explícitamente la política de datos,
So that pueda crear mi cuenta cumpliendo con la normativa colombiana.

**Acceptance Criteria:**

**Given** que un usuario está en la página de registro
**When** intenta enviar el formulario sin marcar la casilla de aceptación de la Ley 1581
**Then** el sistema debe mostrar un error de validación
**And** no se debe crear la cuenta

**Given** que el usuario ingresa datos válidos y acepta la política
**When** envía el formulario
**Then** el sistema debe crear la cuenta y cifrar la contraseña
**And** registrar un log de auditoría inmutable con la fecha/hora y dirección IP

**Given** que el usuario ha completado el registro
**When** la cuenta es creada
**Then** el sistema debe iniciar sesión automáticamente y redirigirlo al dashboard vacío

### Story 1.3: Inicio y Cierre de Sesión Seguro

Como usuario registrado,
Quiero poder iniciar y cerrar sesión usando mi correo electrónico y contraseña,
So that pueda acceder a mis datos financieros de forma segura.

**Acceptance Criteria:**

**Given** que un usuario no autenticado intenta acceder a una ruta protegida
**When** el sistema procesa la petición
**Then** debe redirigirlo automáticamente a la página de login

**Given** que el usuario está en la página de inicio de sesión
**When** ingresa credenciales válidas
**Then** el sistema debe iniciar su sesión, generar la cookie SSR y redirigirlo al panel de control

**Given** que un usuario autenticado hace clic en "Cerrar sesión"
**When** el sistema procesa la acción
**Then** se debe invalidar la sesión y redirigir al usuario al login

## Epic 2: Ingreso de Deudas y Empatía Educativa

Permitir a los usuarios registrar manualmente la información de sus deudas de forma segura, precisa y sin frustraciones, apoyándose en ayudas visuales contextuales y validaciones estrictas.

### Story 2.1: Estructura Segura y Aislamiento de Deudas (Core Backend)

Como usuario,
Quiero que mis datos financieros se guarden con máxima seguridad, precisión y privacidad,
So that tenga la tranquilidad de que nadie más puede ver mis deudas y los cálculos sean exactos.

**Acceptance Criteria:**

**Given** que se crea el modelo de `Debt`
**When** se definen los campos financieros (saldo, tasa, cuota)
**Then** deben implementarse estrictamente usando `DecimalField` en BD y `decimal.Decimal` en Python

**Given** que el modelo `Debt` guarda datos sensibles
**When** se almacena en la base de datos
**Then** los datos deben estar cifrados en reposo (AES-256)

**Given** que se consulta la lista de deudas en el sistema
**When** se invoca la lectura de datos
**Then** debe usarse obligatoriamente un `TenantManager` que filtre por el usuario actual

### Story 2.2: Formulario Interactivo de Ingreso de Deudas y Validaciones

Como usuario,
Quiero poder ingresar los detalles de mis deudas a través de un formulario amigable,
So that pueda registrar mi información financiera sin errores.

**Acceptance Criteria:**

**Given** que el usuario está en la vista de añadir deuda
**When** carga la página
**Then** debe ver un formulario con los campos: Nombre, Saldo Actual, Tasa de Interés y Pago Mínimo

**Given** que el usuario diligencia el formulario
**When** ingresa montos negativos o ilógicos
**Then** el formulario debe prevenir el envío y mostrar errores amigables

**Given** que el usuario ingresa la tasa de interés
**When** el valor supera el límite de usura legal (ej. > 40%)
**Then** el sistema debe mostrar una advertencia visual

**Given** que el usuario ingresa datos válidos
**When** envía el formulario
**Then** la deuda debe guardarse correctamente y redirigirlo a la lista de deudas

### Story 2.3: Sistema de Ayuda Contextual

Como usuario sin conocimientos financieros,
Quiero recibir ayudas visuales y explicaciones simples al momento de llenar el formulario,
So that sepa exactamente dónde encontrar los datos que me piden sin frustrarme.

**Acceptance Criteria:**

**Given** que el usuario está viendo el formulario de deudas
**When** observa campos técnicos como "Tasa de Interés"
**Then** debe ver un botón o icono de ayuda (?) junto al campo

**Given** que el usuario hace clic en el botón de ayuda
**When** la acción se activa
**Then** debe aparecer un modal o tooltip con una explicación simple y una imagen ilustrativa

**Given** que el modal de ayuda está abierto
**When** el usuario hace clic fuera de él o en la "X"
**Then** el modal debe cerrarse

## Epic 3: Visualización de Salud Financiera y Colaboración

Proporcionar al usuario un "termómetro" claro de su panorama financiero general, e introducir la capacidad de vincular a su pareja para la gestión conjunta de deudas.

### Story 3.1: Dashboard Consolidado "Termómetro de Deudas"

Como usuario con deudas registradas,
Quiero ver un panel de control con el resumen total de mis saldos y una gráfica visual,
So that entienda mi salud financiera de un solo vistazo.

**Acceptance Criteria:**

**Given** que el usuario no tiene ninguna deuda registrada
**When** ingresa al dashboard
**Then** debe ver un "Empty State" con un botón para agregar su primera deuda

**Given** que el usuario tiene deudas
**When** carga el dashboard
**Then** el sistema debe calcular y mostrar el Saldo Total y Pago Mínimo Total

**Given** que el dashboard se está renderizando
**When** se muestran los datos consolidados
**Then** debe incluir un termómetro/barra gráfica de progreso

**Given** que el sistema realiza los cálculos
**When** extrae los datos
**Then** debe usar el `TenantManager` para asegurar que solo se procesen datos del usuario

### Story 3.2: Invitación y Vinculación de Pareja

Como usuario principal,
Quiero poder invitar a mi pareja a la plataforma mediante su correo electrónico,
So that ambos podamos ver el mismo dashboard consolidado.

**Acceptance Criteria:**

**Given** que el usuario está en su perfil
**When** selecciona la opción "Invitar pareja" e ingresa un email
**Then** el sistema debe enviar un enlace de invitación único

**Given** que la pareja hace clic en el enlace
**When** completa su registro aceptando la Ley 1581
**Then** su cuenta queda vinculada al mismo Tenant/Grupo del usuario principal

**Given** que la pareja ya está vinculada
**When** inicia sesión y va a su dashboard
**Then** debe ver la misma vista de deudas consolidadas del usuario principal

## Epic 4: Generación del "Mapa de Ruta" Tangible

Brindar tranquilidad al usuario permitiéndole descargar instantáneamente un reporte PDF con su plan de pagos, procesado asíncronamente.

### Story 4.1: Infraestructura de Generación Asíncrona (Celery)

Como sistema,
Quiero procesar la generación pesada de PDFs en segundo plano usando workers,
So that la aplicación web principal no se bloquee.

**Acceptance Criteria:**

**Given** que el frontend solicita generar un reporte
**When** se llama al endpoint
**Then** el sistema debe crear un registro `DocumentTask` en estado PENDING, encolar en Celery y devolver el task_id

**Given** que hay una tarea en la cola
**When** el worker de Celery la procesa
**Then** debe recopilar los datos del Tenant y usar WeasyPrint para generar el PDF

**Given** que WeasyPrint termina
**When** el archivo se guarda exitosamente
**Then** se debe actualizar `DocumentTask` a COMPLETED y almacenar la ruta del archivo

### Story 4.2: Experiencia de Descarga en el Navegador (Polling)

Como usuario,
Quiero ver que el sistema está preparando mi reporte y que se descargue automáticamente al terminar,
So that no tenga que recargar la página.

**Acceptance Criteria:**

**Given** que el usuario hace clic en "Descargar PDF"
**When** recibe el task_id
**Then** la UI debe mostrar un indicador de carga y bloquear el botón

**Given** que el indicador está activo
**When** pasan 2 segundos
**Then** el frontend debe hacer polling al endpoint de estado

**Given** que el estado consultado es COMPLETED
**When** el frontend recibe la respuesta
**Then** debe detener el polling, iniciar la descarga del PDF y restaurar el botón

### Story 4.3: Diseño y Maquetación del Reporte PDF

Como usuario,
Quiero que mi PDF descargado tenga un diseño profesional y claro,
So that pueda imprimirlo fácilmente como recordatorio de mi meta.

**Acceptance Criteria:**

**Given** que WeasyPrint genera el PDF
**When** procesa el documento
**Then** debe usar un template HTML optimizado para impresión (alto contraste, tipografía legible)

**Given** que el reporte incluye las deudas
**When** se renderiza
**Then** debe mostrar el Saldo Total, Pago Mínimo Total y tabla desglosada

**Given** que el PDF es un documento proyectado
**When** se genera
**Then** debe incluir fecha de generación y disclaimer legal

## Epic 5: Centro de Educación Financiera y Panel de Contenido

Entregar micro-lecciones de educación financiera a los usuarios y dotar al equipo interno de un administrador CMS ligero y seguro.

### Story 5.1: Gestor de Contenido Educativo (Django Admin CMS)

Como creador de contenido,
Quiero poder gestionar tips financieros desde un panel interno,
So that mantenga la plataforma actualizada sin depender de desarrolladores.

**Acceptance Criteria:**

**Given** que se crean los modelos `Tip` y `Category`
**When** se registran en Django Admin
**Then** deben permitir operaciones CRUD completas

**Given** que el administrador ingresa un nuevo Tip
**When** guarda el formulario
**Then** debe exigir obligatoriamente Título, Contenido, Categoría y Estado

**Given** que un usuario regular (cliente) intenta acceder a `/admin/`
**When** el sistema procesa la petición
**Then** debe bloquear el acceso con error 403 o redirigirlo

### Story 5.2: Feed Interactivo de Micro-lecciones

Como usuario registrado,
Quiero poder explorar un feed de tips financieros presentados de forma atractiva,
So that pueda educarme financieramente de forma ágil y desde mi celular.

**Acceptance Criteria:**

**Given** que el usuario navega a la sección de "Educación"
**When** la página carga
**Then** debe mostrar únicamente los Tips con estado "Publicado" ordenados por los más recientes

**Given** que se renderizan los tips
**When** se muestran en pantalla
**Then** deben tener formato de tarjetas (cards) limpias y responsivas

**Given** que hay varias categorías de tips
**When** el usuario selecciona una categoría
**Then** el feed debe actualizarse para mostrar solo los tips relevantes
