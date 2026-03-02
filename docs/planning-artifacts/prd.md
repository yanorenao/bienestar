---
stepsCompleted: ["step-01-init", "step-02-discovery", "step-02b-vision", "step-02c-executive-summary", "step-03-success", "step-04-journeys", "step-05-domain"]
inputDocuments: ["docs/planning-artifacts/product-brief-prueba-bienestar-2026-03-02.md", "docs/brainstorming/brainstorming-session-2026-03-02-094927.md"]
briefCount: 1
researchCount: 0
brainstormingCount: 1
projectDocsCount: 0
classification:
  projectType: web_app
  domain: fintech
  complexity: high
  projectContext: greenfield
---

# Product Requirements Document (PRD)

## Executive Summary

La Plataforma Web de Bienestar Financiero (prueba-bienestar) es una solución diseñada para el mercado colombiano, orientada a ayudar a jóvenes profesionales (tanto formales como independientes con ingresos variables) a gestionar, entender y liquidar sus deudas. A través de un enfoque centrado en la educación financiera práctica, amigable y libre de juicios, la plataforma transforma la ansiedad financiera en control y tranquilidad. A largo plazo, el producto evolucionará hacia un modelo B2B2C, ofreciéndose como un beneficio corporativo (salario emocional) para reducir el estrés financiero en las empresas.

### What Makes This Special

A diferencia de las aplicaciones bancarias tradicionales o las hojas de cálculo que generan culpa o frustración al enfocarse solo en "recortar gastos", esta plataforma opera bajo el principio de "Simplicidad Radical" y "Educación Empática". El momento crítico de valor (Aha! moment) ocurre cuando el usuario piensa: *"¡Al fin entiendo mi deuda y sé cómo pagarla!"*. Esto se logra conectando la teoría con la acción mediante simuladores interactivos (ej. método de la bola de nieve), eliminando la jerga financiera y enfocándose en mostrar un progreso visual y motivador hacia la libertad financiera.

## Project Classification

- **Project Type:** Web App (con evolución planificada a SaaS B2B2C)
- **Domain:** Fintech / Edtech
- **Complexity:** High (Manejo de datos sensibles, cálculos financieros)
- **Project Context:** Greenfield (Desarrollo desde cero)

## Success Criteria

### User Success
El éxito para el usuario se define por la transición de la ansiedad a la claridad financiera. 
- **Momento "Aha!":** Cuando el usuario ve su dashboard o descarga su reporte y piensa *"¡Al fin entiendo mi deuda y sé cómo pagarla!"*, sin sentirse juzgado.
- **Comportamiento:** Lograr y mantener una "racha de ahorro" o cumplimiento de su plan de pagos por al menos 3 meses consecutivos.
- **Reducción real:** Disminución medible en el saldo total de sus deudas tras 6 meses de uso de la plataforma.

### Business Success
Para el negocio, el éxito inicial se centra en la tracción y validación del modelo sin fricciones:
- **Adquisición:** Alcanzar los primeros 1,000 usuarios registrados en el plan gratuito en los primeros 3 meses post-lanzamiento.
- **Validación para escalar:** Demostrar retención y *engagement* suficiente para justificar la inversión hacia la futura iteración B2B2C (venta a empresas).

### Technical Success
Como plataforma Fintech, el éxito técnico requiere confianza absoluta:
- **Precisión:** Cálculos financieros (ej. método bola de nieve y proyecciones de intereses) 100% exactos y sin errores.
- **Rendimiento:** Generación inmediata de **reportes PDF** de estado de cuenta/plan de pagos.
- **Seguridad:** Protección robusta de los datos financieros ingresados por los usuarios, cumpliendo con la normativa de protección de datos.

### Measurable Outcomes
- **CAC (Costo de Adquisición de Clientes):** Mantener un CAC mínimo mediante crecimiento orgánico.
- **MAU (Monthly Active Users):** Porcentaje de los 1,000 usuarios iniciales que retornan mensualmente para actualizar saldos o consumir tips.
- **Tasa de Retención a 90 días:** % de usuarios que alcanzan su hito de 3 meses de racha.

## Product Scope

### MVP - Minimum Viable Product
Lo esencial para validar el problema y aportar valor inmediato:
- Autenticación simple (Email, opcional Google/Apple).
- Formulario amigable para ingreso manual de saldos, tasas y pagos mínimos. (**Sin conexión bancaria en esta fase**).
- Dashboard Visual ("termómetro de deudas") con proyección de pagos.
- **Exportación de Reportes en PDF:** Descarga del plan de pagos o estado de cuenta resumido.
- Centro de Tips: Micro-lecciones de educación financiera sin jerga técnica.
- Soporte para escenarios en pareja (dashboard compartido simple).
- Proceso explícito de **Aceptación de Política de Datos Personales** durante el registro.

### Growth Features (Post-MVP)
Funcionalidades para escalar la competitividad (fuera del MVP):
- Integración automática con bancos (Open Banking / Scraping).
- Aplicaciones móviles nativas (iOS / Android).
- Gamificación avanzada y recompensas.

### Vision (Future)
El modelo a largo plazo:
- **Modelo B2B2C:** La plataforma se vende a empresas como "salario emocional" para sus empleados.
- **Hub Financiero:** Refinanciamiento de deudas con tasas preferenciales negociadas en bloque para los empleados.

## User Journeys

### 1. Camila obtiene su "Mapa de Ruta" (Primary User - Success Path)
**Opening Scene:** Camila (nuestra freelancer) ha terminado de ingresar manualmente los saldos de sus tres tarjetas de crédito. Se siente un poco abrumada por el total, pero la plataforma le muestra un mensaje empático y un gráfico de "bola de nieve" animándola. 
**Rising Action:** Ella necesita algo tangible, algo que pueda pegar en su nevera para no perder el foco durante los meses en que sus ingresos bajan. En el centro de su dashboard, ve un botón claro y llamativo: *"Descargar mi plan de pagos"*.
**Climax:** Camila hace clic en el botón. Sin tiempos de espera largos ni configuraciones complicadas, el sistema genera y descarga instantáneamente un reporte en PDF con un diseño limpio.
**Resolution:** Al abrir el PDF, ve su ruta exacta mes a mes. Suspira aliviada y piensa *"¡Al fin entiendo mi deuda y sé cómo pagarla!"*. Tiene el control.

### 2. Andrés se topa con un bloqueo técnico (Primary User - Edge Case / Frustration)
**Opening Scene:** Andrés está intentando registrar el crédito de su carro. El formulario le pide la "Tasa de Interés Efectiva Anual", pero él no tiene idea de cuál es. Empieza a sentirse ansioso y a pensar que "las finanzas no son lo suyo". Está a punto de cerrar la pestaña.
**Rising Action:** Justo antes de abandonar, nota un pequeño botón amigable junto al campo que dice: *"¿No sabes cómo calcularlo o dónde encontrarlo?"*.
**Climax:** Al hacer clic, no recibe un bloque de texto legal, sino una ayuda visual muy sencilla: una imagen que le muestra exactamente en qué parte de su extracto bancario buscar ese número, y una calculadora rápida integrada en caso de que solo sepa el monto de su cuota.
**Resolution:** Andrés respira, encuentra el dato en su app del banco, lo ingresa y continúa. La plataforma lo ayudó a superar el bloqueo sin hacerlo sentir culpable ni ignorante.

### 3. El Equipo de Contenido actualiza los "Tips" (Admin / Operations User)
**Opening Scene:** Laura, la creadora de contenido del equipo de "prueba-bienestar", se da cuenta de que es época de pago de primas (diciembre) y necesita subir tips urgentes sobre cómo no gastarse todo el dinero extra.
**Rising Action:** Inicia sesión en la plataforma con su cuenta de Administradora. No necesita llamar a ningún desarrollador; entra directamente a su panel de gestión ("Gestor de Tips").
**Climax:** Laura crea una nueva "tarjeta" de tip, escribe un título atractivo, pega el texto corto, le asigna la categoría "Ingresos Extra" y presiona "Publicar".
**Resolution:** Inmediatamente, la base de datos se actualiza y el nuevo tip aparece en el *feed* educativo de todos los usuarios registrados. Laura mantiene la plataforma viva y contextualizada con el momento del año.

### Journey Requirements Summary
Estas narrativas revelan capacidades técnicas específicas que debemos construir:
1. **Motor de Generación PDF:** Un microservicio o librería capaz de tomar los datos del dashboard y renderizarlos rápidamente en un documento PDF bien diseñado a través de un solo botón.
2. **Sistema de Ayuda Contextual (Tooltips/Modales):** Componentes de interfaz de usuario en los formularios que ofrezcan información educativa "Justo a tiempo" (imágenes explicativas, micro-calculadoras) para evitar el abandono.
3. **Panel de Administración (CMS ligero):** Un módulo con control de acceso (Autenticación basada en Roles - RBAC) que permita al equipo interno ejecutar operaciones CRUD (Crear, Leer, Actualizar, Borrar) sobre los Tips Educativos.

## Domain-Specific Requirements

Al ser una plataforma que maneja datos financieros ( Fintech ), existen consideraciones cruciales incluso para este MVP simplificado:

### Compliance & Regulatory
- **Protección de Datos (Ley 1581 en Colombia):** Es obligatorio un proceso de *opt-in* explícito. El usuario debe aceptar la política de tratamiento de datos personales durante el registro antes de poder ingresar cualquier saldo.
- **Auditoría Básica:** Registro inmutable de cuándo y desde qué IP un usuario aceptó los términos y condiciones.

### Technical Constraints
- **Seguridad de Datos en Reposo:** Todos los datos financieros ingresados manualmente (saldos, tasas) deben estar cifrados en la base de datos (Ej. AES-256).
- **Aislamiento de Entornos (Tenant Model):** Asegurar a nivel de base de datos que un usuario jamás pueda consultar los registros financieros de otro (Row-Level Security o filtros estrictos en las consultas).

### Integration Requirements
- **Desconexión Bancaria Consciente (MVP):** Se confirma explícitamente que en la Fase 1 (MVP) **NO habrá conexión, integración vía API, ni web scraping con ninguna entidad bancaria**. Todos los datos se ingresarán y actualizarán de forma manual por el usuario.

### Risk Mitigations
- **Riesgo:** Un usuario ingresa datos falsos o erróneos que dañan la proyección de su PDF.
- **Mitigación:** Proveer validaciones de formulario (ej. tasas de interés no pueden superar la tasa de usura vigente) y permitir la edición fácil posterior a la creación.
