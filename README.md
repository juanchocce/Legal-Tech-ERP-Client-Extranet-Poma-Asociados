
# ⚖️ Legal-Tech ERP & Client Extranet | Poma & Asociados

Plataforma Integral de Gestión Jurídica, Control de Plazos y Trazabilidad Operativa para Consultoría Legal Boutique.

Diseñada bajo arquitectura de Monolito Modular Full-Stack, gobernanza de datos relacionales en PostgreSQL (3NF) y seguridad granular basada en roles (RBAC + RLS).

---


<img width="1366" height="768" src="https://github.com/juanchocce/Legal-Tech-ERP-Client-Extranet-Poma-Asociados/blob/main/img/poma1.png" />

---

## 💼 1. Diagnóstico Comercial y Desafío de Negocio

Los despachos jurídicos independientes y firmas boutique enfrentan fricciones operativas estructurales que impactan directamente su rentabilidad y tasa de retención:

* **Fuga de Rentabilidad (Horas vs. Honorarios):** Inexistencia de un registro sistemático del costo horario invertido por abogado versus las tarifas globales pactadas, impidiendo identificar qué materias o subtipos procesales generan pérdidas financieras.
* **Riesgo Crítico de Caducidad Procesal:** Pérdida de plazos perentorios y audiencias por dispersión de agendas individuales, gestionadas en canales informales sin alertas centralizadas de prioridad.
* **Fricción en Atención al Patrocinado:** Clientes llamando repetidamente para consultar el estado de sus expedientes, consumiendo tiempo valioso del equipo en tareas administrativas no facturables.
* **Falta de Trazabilidad y Riesgo de Confidencialidad:** Expedientes penales y familiares compartidos en carpetas locales o nubes genéricas sin auditoría de mutaciones, segregación de privilegios ni respaldos de seguridad contra borrado accidental.

---

<img width="1366" height="768" src="https://github.com/juanchocce/Legal-Tech-ERP-Client-Extranet-Poma-Asociados/blob/main/img/poma2.png" />

---

## 🎯 2. Solución Implementada: Monolito Modular con 3 Vistas

En lugar de fragmentar la infraestructura en tres aplicaciones dispersas, se diseñó una solución centralizada en Next.js 14 (App Router) gobernada por un motor único de base de datos relacional. La plataforma aísla la experiencia de usuario mediante Route Groups:

* **Front-End Corporativo /(marketing):** Posicionamiento SEO de alta gama para las áreas de Derecho Penal y Familia, optimizado para conversión directa hacia canal de triaje legal. Contiene la Landing Page Boutique, Identidad Institucional y Captación Conversacional.
* **Extranet del Patrocinado /(portal-cliente):** Interfaz restringida y transparente donde el cliente consulta exclusivamente los hitos procesales aprobados (`es_visible_cliente = true`) y descarga resoluciones autorizadas vía URLs firmadas con caducidad de 10 minutos. Incluye Dashboard Minimalista, Timeline Procesal Pública y Repositorio Seguro.
* **ERP Operativo /(erp-interno):** Centro de mando para los abogados y asistentes. Permite administrar el ciclo de vida del litigio, asignar personal, auditar actuaciones y conciliar pagos. Integra Grilla de Expedientes 3NF, Agenda y Semáforo de Plazos, Asignación Multi-Abogado y Control de Honorarios.


```text
                                [ Dominio: pomaasociados.com ]
                                              │
               ┌──────────────────────────────┼──────────────────────────────┐
               ▼                              ▼                              ▼
      /(marketing)                   /(portal-cliente)              /(erp-interno)
      ├── Landing Page Boutique      ├── Dashboard Minimalista      ├── ERP Legal Integral
      ├── Identidad Institucional    ├── Timeline Procesal Pública  ├── Grilla de Expedientes 3NF
      └── Captación Conversacional   └── Repositorio Seguro         ├── Agenda y Semáforo de Plazos
                                                                    ├── Asignación Multi-Abogado
                                                                    └── Control de Honorarios
```

---

## 📊 3. Modelado de Datos para Business Intelligence (BI)

El modelo de datos fue normalizado en Tercera Forma Normal (3NF) dentro de PostgreSQL, desacoplando los compromisos comerciales de los flujos de caja y garantizando integridad referencial mediante 10 tablas estratégicas: `usuarios`, `clientes`, `casos`, `asignaciones_caso`, `actuaciones_hitos`, `documentos`, `agenda_plazos`, `contratos_honorarios`, `transacciones_pago` y `horas_laborales`.


```text
[usuarios] ──< [clientes] ──< [casos] ──< [asignaciones_caso] >── [usuarios]
                                 │
         ┌───────────────────────┼───────────────────────┬───────────────────────┐
         ▼                       ▼                       ▼                       ▼
  [actuaciones_hitos]      [documentos]          [agenda_plazos]        [contratos_honorarios]
                                                                                 │
                                                                                 ▼
                                                                        [transacciones_pago]
```


### Impacto en la Toma de Decisiones Gerenciales:

* **Margen Real por Proceso:** Cruce analítico entre la tabla `horas_laborales` y `transacciones_pago` para calcular la rentabilidad neta por hora-hombre según la materia (Penal vs. Familia).
* **Monitoreo de Embudo Judicial:** Detección de cuellos de botella en la tabla `casos` evaluando los tiempos de permanencia de un expediente en cada instancia (Fiscalía, Juzgado de Investigación Preparatoria, Sala Superior).
* **Control de Carga Laboral:** Balanceo dinámico de casos activos por abogado sénior vs. capacidad operativa de los asistentes.

---

<img width="1366" height="768" src="https://github.com/juanchocce/Legal-Tech-ERP-Client-Extranet-Poma-Asociados/blob/main/img/poma19.png" />

---



## 🔒 4. Matriz de Seguridad y Privacidad Legal (RBAC + RLS)

Tratándose de causas penales y disputas de derecho de familia, la seguridad de la información es un requerimiento no negociable:


* **SuperAdmin (Socia):** Ver Expedientes: Todos | Subir Actuaciones: Sí | Eliminar Documentos: Soft Delete / Purga | Gestión Financiera: Total | Configuración: Sí.
* **Abogado Asociado:** Ver Expedientes: Asignados | Subir Actuaciones: Sí | Eliminar Documentos: Restringido | Gestión Financiera: Solo Lectura | Configuración: No.
* **Asistente Legal:** Ver Expedientes: Asignados | Subir Actuaciones: Sí | Eliminar Documentos: Bloqueado | Gestión Financiera: Oculto | Configuración: No.
* **Cliente Patrocinado:** Ver Expedientes: Solo el Propio | Subir Actuaciones: No | Eliminar Documentos: Bloqueado | Gestión Financiera: Solo sus Recibos | Configuración: No.

### Políticas de Protección:

* **Row-Level Security (RLS):** Filtrado forzado a nivel de motor de base de datos. Si un cliente altera parámetros HTTP, PostgreSQL rechaza la consulta al verificar que el `cliente_id` no coincide con el token JWT autenticado.
* **Borrado Lógico (Soft Delete):** Ningún documento judicial puede destruirse físicamente por personal operativo. La columna `eliminado_en` (TIMESTAMPTZ) audita la baja lógica sin perder la evidencia forense ni el hash del archivo.



---

<img width="1366" height="768" src="https://github.com/juanchocce/Legal-Tech-ERP-Client-Extranet-Poma-Asociados/blob/main/img/poma10.png" />

---

## 📅 5. Integración Pragmática: Google Calendar RFC-5545

Para eliminar el riesgo de caducidad procesal sin incurrir en la fragilidad de tokens OAuth vencidos o infraestructura de sincronización costosa, se implementó un generador determinista bajo el estándar RFC-5545:

1. El ERP calcula fechas, horas perentorias y metadatos del juzgado.
2. Genera una URL parametrizada: `https://calendar.google.com/calendar/render?action=TEMPLATE...`
3. Con 1 solo clic, el abogado líder y el asistente sincronizan la audiencia en sus dispositivos móviles institucionales con alertas nativas preconfiguradas.


---

<img width="1366" height="768" src="https://github.com/juanchocce/Legal-Tech-ERP-Client-Extranet-Poma-Asociados/blob/main/img/poma15.png" />

---

## 🛠️ 6. Stack Tecnológico y Costo Operativo

* **Core Framework:** Next.js 14 (App Router) - Renderizado híbrido (SSR para marketing, Client Components para reactividad en ERP).
* **Lenguaje:** TypeScript (Strict) - Contratos de interfaz estrictos; reducción total de errores en tiempo de ejecución.
* **Base de Datos & Auth:** Supabase (PostgreSQL) - RLS nativo, gestión de sesiones JWT seguras y funciones transaccionales ACID.
* **Almacenamiento:** Supabase Storage - Buckets privados con generación de URLs firmadas temporales para expediente confidencial.
* **Diseño y UI:** Tailwind CSS + Lucide - Paleta institucional sobria: Burgundy (#7A3030), Oro (#C5A059) y Crema (#F5F1E9).
* **Costo Mensual de Infraestructura:** $0.00 USD / mes en fase de lanzamiento (tiers gratuitos de Vercel y Supabase soportando holgadamente hasta 100 clientes activos y 6 miembros del equipo).

---

## 📂 7. Estructura del Repositorio


```text
poma-asociados/
├── src/
│   ├── app/
│   │   ├── (marketing)/                # Portal público (Home, Firma, Especialidades, Contacto)
│   │   ├── (portal-cliente)/           # Extranet segura del patrocinado (Timeline, Descargas)
│   │   ├── erp-interno/                # ERP Legal (Expedientes, Agenda, Clientes, Finanzas)
│   │   ├── (auth)/login/               # Autenticación unificada con selector de roles demo
│   │   └── api/                        # Handlers backend y webhooks
│   ├── components/
│   │   ├── erp/                        # Componentes operativos (Sidebar, Tablas dinámicas, Modales)
│   │   ├── portal/                     # Línea de tiempo procesal y descargadores de resoluciones
│   │   └── marketing/                  # UI corporativa de alta conversión
│   ├── lib/
│   │   ├── actions/                    # Server Actions validadas con Zod (cases, documents, agenda)
│   │   ├── google-calendar.ts          # Utilidad algorítmica RFC-5545
│   │   └── supabase/                   # Clientes SSR y Middleware de sesión
│   └── types/                          # Tipado autogenerado de la base de datos
├── supabase/
│   ├── schema.sql                      # DDL relacional (10 tablas, ENUMs y políticas RLS)
│   └── seed.sql                        # Datos maestros de abogados, casos y estructura base
├── middleware.ts                       # Control de navegación y enrutamiento RBAC
└── tailwind.config.ts                  # Tokens de colorimetría institucional
```

---

## 🚀 8. Puesta en Marcha Local

### Clonar e instalar dependencias:

```text
git clone https://github.com/juanchocce/Legal-Tech-ERP-Client-Extranet-Poma-Asociados.git
cd poma-asociados-legaltech
pnpm install
```

### Variables de entorno (.env.local):
Configurar las siguientes claves en el archivo raíz:
```text
NEXT_PUBLIC_SUPABASE_URL=https://tu-proyecto.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=tu-anon-key
SUPABASE_SERVICE_ROLE_KEY=tu-service-role-key
NEXT_PUBLIC_SITE_URL=http://localhost:3000
```

### Migraciones:
1. Ejecutar `supabase/schema.sql` en el SQL Editor de Supabase.
2. Ejecutar `supabase/seed.sql` para poblar el equipo y datos iniciales.

### Ejecución:

```text
npx tsc --noEmit     # Verificación estricta de tipos
pnpm run dev        # Servidor local en http://localhost:3000
```
