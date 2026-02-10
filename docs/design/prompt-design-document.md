# Prompt: Generación del Documento de Diseño de GIIA

> **Propósito:** Este prompt está optimizado para Claude Opus. Debe ejecutarse proporcionando como contexto adjunto los documentos del SRS (`srs.md`) y las 20 historias de usuario (`HU-001.md` a `HU-020.md`).
>
> **Instrucción de uso:** Copia el contenido de la sección "PROMPT" completa y pégala en una conversación nueva con Opus, adjuntando los archivos mencionados.

---

## PROMPT

---

### ROL Y OBJETIVO

Eres un Arquitecto de Software Senior especializado en sistemas SaaS multi-tenant, con experiencia profunda en diseño de sistemas event-driven y dominios complejos como gestión de inventario DDMRP (Demand Driven MRP). Tu tarea es producir el **Documento de Diseño técnico completo** del sistema GIIA.

**Audiencia:** Exclusivamente desarrolladores de software.
**Idioma:** Español técnico.
**Nivel de detalle:** Máximo. Cada decisión de diseño debe estar justificada.

---

### CONTEXTO DEL SISTEMA

**GIIA** (Gestión de Inventario con Inteligencia Artificial) es un sistema SaaS multi-tenant de gestión de inventario basado en la metodología **DDMRP** (Demand Driven MRP), diseñado para comercios minoristas, distribuidores e importadores PYMES.

El sistema transforma la tarea de determinar **qué, cuánto y cuándo comprar** en un proceso visual mediante un **semáforo (Rojo/Amarillo/Verde)**. El MVP se enfoca en una **posición estratégica única** (almacén o punto de venta).

Los documentos adjuntos contienen:
- **SRS (`srs.md`):** 105 requerimientos funcionales (RF-001 a RF-105), 23 no funcionales (RNF-001 a RNF-023), 6 restricciones, 10 dependencias, 10 riesgos.
- **20 Historias de Usuario (`HU-001.md` a `HU-020.md`):** Con criterios de aceptación Given/When/Then, escenarios alternativos, y notas de aclaraciones del Product Owner.

**Debes leer exhaustivamente TODOS los documentos adjuntos antes de producir el diseño.** Cada decisión de diseño debe ser trazable a requerimientos específicos.

---

### DECISIONES TÉCNICAS YA TOMADAS

Las siguientes decisiones técnicas son **definitivas y no negociables**:

| Decisión | Valor | Notas |
|----------|-------|-------|
| **Backend** | **Go (Golang)** | Monolito modular. Cada módulo debe poder extraerse como microservicio en el futuro. |
| **Frontend** | **Next.js** | Desplegado en Vercel (serverless). |
| **Base de datos** | **PostgreSQL** | Única base de datos relacional. |
| **Multi-tenancy** | **Row-level isolation** | Columna `tenant_id` en **todas** las tablas de negocio. Uso de Row-Level Security (RLS) de PostgreSQL. |
| **Actualizaciones de UI** | **Polling** | No WebSockets en el MVP. El frontend hace polling periódico a endpoints de estado. |
| **Motor DDMRP** | **Event-driven** | Los recálculos de CPD, zonas, flujo neto se disparan por eventos (transacciones). |
| **Deployment backend** | **Serverless** | Plataformas como Vercel/Netlify/Render. Diseñar para cold starts y stateless. |
| **Históricos** | **Persistir históricos de KPIs** | Se deben almacenar snapshots diarios de KPIs y estado de buffers para tendencias y comparativas. |

---

### DOMINIO DDMRP — REGLAS DE NEGOCIO CRÍTICAS

Estas fórmulas y reglas son **la ley del sistema**. No pueden simplificarse ni alterarse:

#### Cálculo de Zonas del Buffer

```
Zona Roja Base    = CPD × LT × Factor_LT
Zona Roja Plus    = CPD × LT × Factor_Variabilidad
Zona Roja Total   = Roja_Base + Roja_Plus

Zona Amarilla     = CPD × LT

Zona Verde        = MAX(CPD × LT × Factor_LT, MOQ, Ciclo_Pedido_Días × CPD)

TOG (Top of Green) = Zona_Roja + Zona_Amarilla + Zona_Verde
```

#### Ecuación de Flujo Neto

```
Flujo Neto = Inventario_Físico + Inventario_En_Tránsito - Demanda_Calificada
```

Donde:
- **Inventario Físico** = Σ ingresos - Σ egresos (todas las transacciones confirmadas)
- **Inventario En Tránsito** = Σ cantidades de OC en estado Pendiente/Parcialmente Recibida
- **Demanda Calificada** = OV en firme vencidas + Picos Calificados dentro del horizonte de picos

#### Picos Calificados

```
Un pedido es Pico Calificado si: cantidad_pedido > Umbral_Pico
Umbral_Pico = Zona_Roja_Total × Porcentaje_Umbral (default: 50%)
```

#### Sugerencias de Reposición

```
Si Flujo_Neto < TOG → Cantidad_Sugerida = TOG - Flujo_Neto
Prioridad = profundidad relativa en zona roja (mayor penetración = mayor urgencia)
```

#### Factores de Ajuste Dinámico (prioridad Should, pero DEBEN estar diseñados)

```
CPD_Ajustado = CPD × FAD                    (Factor de Ajuste de Demanda)
LT_Ajustado  = LT × FALTD                   (Factor de Ajuste de Lead Time)
Zona_X_Ajustada = Zona_X × FAZ              (Factor de Ajuste de Zona)
```

Los factores tienen **vigencia temporal** (fecha inicio → fecha fin) y se aplican multiplicativamente.

#### CPD (Consumo Promedio Diario)

```
CPD = Σ unidades_vendidas_en_ventana / días_en_ventana
Ventana por defecto: 7 días (configurable por tenant)
Productos nuevos sin historial: CPD manual ingresado por usuario
```

---

### MÓDULOS FUNCIONALES DEL SISTEMA

El sistema se compone de los siguientes dominios/módulos. Cada módulo debe diseñarse como un **bounded context** con interfaces claras entre ellos:

| # | Módulo | HUs | RFs | Descripción |
|---|--------|-----|-----|-------------|
| 1 | **Auth & Tenancy** | HU-019 | RF-095 a RF-101 | Autenticación email/password, 3 roles (Admin, Usuario, Solo Lectura), aislamiento por tenant, gestión de usuarios |
| 2 | **Proveedores** | HU-001 | RF-001 a RF-004 | CRUD de proveedores con Lead Time, ubicación, datos de contacto, estado activo/inactivo |
| 3 | **Categorías** | HU-002 | RF-005 a RF-008 | Categorías jerárquicas (árbol), sin límite de niveles |
| 4 | **Productos** | HU-003 | RF-009 a RF-016 | Catálogo con SKU único, múltiples proveedores por producto (cada uno con LT, MOQ, frecuencia), perfil de buffer 1:1 |
| 5 | **Órdenes de Compra** | HU-004 | RF-017 a RF-023 | Ciclo: Pendiente → Parcialmente Recibida → Recibida / Cancelada. Cálculo automático de fecha de llegada. Afecta inventario en tránsito. |
| 6 | **Órdenes de Venta** | HU-005 | RF-024 a RF-027 | Ciclo: En Firme → Efectuada / Cancelada. Solo OV vencidas o que superen umbral de pico afectan demanda calificada. Al efectuar: egreso automático. |
| 7 | **Ajustes Extraordinarios** | HU-006 | RF-028 a RF-030 | Ingresos (devolución, ajuste+, traspaso) y egresos (pérdida, merma, obsolescencia, ajuste-, muestra). Justificación obligatoria. No permite inventario negativo. |
| 8 | **Motor DDMRP** | HU-007 a HU-012 | RF-031 a RF-062 | CPD, perfiles de buffer, cálculo de zonas, flujo neto, sugerencias de reposición, ajustes dinámicos. **Este es el CORE del sistema.** |
| 9 | **Dashboard** | HU-013 | RF-063 a RF-068 | Semáforo general, productos críticos, filtros por categoría/proveedor, tendencia de buffer |
| 10 | **Planificación** | HU-014 | RF-069 a RF-072 | Lista de reposiciones pendientes, agrupación por proveedor, selección múltiple → crear sugerencia de OC |
| 11 | **Alertas** | HU-015 | RF-073 a RF-078 | Producto en zona roja, llegada próxima, llegada vencida, centro de notificaciones. Resolución automática cuando la condición se resuelve. |
| 12 | **KPIs** | HU-016 | RF-079 a RF-085 | 4 indicadores: Costo Total, Días en Inventario, Inmovilizado, Rotación. Comparativa vs periodo anterior. Históricos diarios. |
| 13 | **Entrada Manual** | HU-017 | RF-086 a RF-089 | Formularios con validación en tiempo real, autocompletado fuzzy |
| 14 | **Importación CSV** | HU-018 | RF-090 a RF-094 | Upload, mapeo de columnas, validación preview, importación selectiva, templates descargables, log de importaciones |
| 15 | **Multi-moneda** | HU-020 | RF-102 a RF-105 | USD + Peso local, tipo de cambio manual, consolidación en moneda base para KPIs |

---

### ENTREGABLE REQUERIDO

Genera el documento de diseño dividido en las siguientes **secciones independientes** (cada sección debe poder analizarse de forma aislada pero mantener coherencia con las demás). **Cada sección DEBE incluir los diagramas Mermaid indicados.**

---

#### SECCIÓN 1: Visión General de Arquitectura

**Contenido obligatorio:**
- Descripción del patrón arquitectónico: **Monolito Modular event-driven** con capacidad de extracción a microservicios.
- Justificación de cada decisión técnica (Go, PostgreSQL, Next.js, serverless).
- Principios de diseño que rigen todo el sistema (SOLID, Clean Architecture, DDD tactical patterns).
- Estrategia de comunicación entre módulos internos (event bus in-process para el monolito, preparado para message broker futuro).
- Estrategia de polling del frontend.

**Diagramas Mermaid obligatorios:**
1. **Diagrama C4 - Nivel 1 (Contexto del Sistema):** GIIA, usuarios, sistemas externos.
2. **Diagrama C4 - Nivel 2 (Contenedores):** Frontend Next.js, Backend Go API, PostgreSQL, Job Scheduler (para snapshots de KPIs).
3. **Diagrama C4 - Nivel 3 (Componentes del Backend):** Todos los módulos internos del monolito, sus dependencias, y el event bus interno.
4. **Diagrama de deployment:** Vercel (frontend), servicio backend (Render/similar), PostgreSQL managed, y flujos de red.

---

#### SECCIÓN 2: Modelo de Datos y Entidades

**Contenido obligatorio:**
- Diseño completo del schema PostgreSQL con **TODAS** las tablas necesarias.
- Estrategia de multi-tenancy: Row-Level Security (RLS) con `tenant_id`, políticas de seguridad.
- Estrategia de soft-delete por entidad.
- Estrategia de auditoría (quién, cuándo, qué cambió).
- Índices recomendados para las queries más frecuentes.
- Estrategia para persistencia de transacciones: proponer el enfoque más escalable y mantenible para las 4 entidades transaccionales (históricos de ventas, históricos de compras, desvíos de ingreso, desvíos de egreso). Considerar que estas tablas crecerán más que las demás.
- Tablas de snapshots para KPIs históricos y estado de buffers diario.
- Estrategia de tipos de datos para montos monetarios (evitar floating point).

**Diagramas Mermaid obligatorios:**
1. **Diagrama ER completo** del dominio de datos maestros (Tenant, User, Supplier, Category, Product, ProductSupplier, BufferProfile).
2. **Diagrama ER completo** del dominio transaccional (PurchaseOrder, PurchaseOrderLine, SalesOrder, SalesOrderLine, ExtraordinaryMovement, InventorySnapshot).
3. **Diagrama ER completo** del dominio DDMRP (BufferZoneCalculation, NetFlowCalculation, ReplenishmentSuggestion, DynamicAdjustment, CPDHistory).
4. **Diagrama ER completo** del dominio de soporte (Alert, Notification, KPISnapshot, ImportLog, CurrencyConfig, SystemConfig).
5. **Diagrama ER consolidado** mostrando las relaciones ENTRE dominios (foreign keys cross-domain).

---

#### SECCIÓN 3: Diseño del Motor DDMRP

**Este es el componente más crítico del sistema entero.** Debe diseñarse con máxima precisión.

**Contenido obligatorio:**
- Arquitectura interna del motor DDMRP como módulo Go.
- Pipeline de cálculo: cómo se encadenan CPD → Zonas → Flujo Neto → Sugerencias.
- Diseño del sistema de eventos internos: qué eventos dispara cada transacción y qué recálculos desencadenan.
- Tabla de eventos y sus efectos:

| Evento | Recalcula CPD | Recalcula Zonas | Recalcula Flujo Neto | Genera Sugerencia | Genera Alerta |
|--------|:---:|:---:|:---:|:---:|:---:|
| OV efectuada | ✓ | ✓ | ✓ | ✓ | ✓ |
| OV en firme registrada | | | ✓ | ✓ | ✓ |
| OC creada | | | ✓ | ✓ | |
| OC recibida | | | ✓ | ✓ | ✓ |
| Ajuste extraordinario | ✓ (si egreso) | ✓ | ✓ | ✓ | ✓ |
| Cambio de perfil buffer | | ✓ | ✓ | ✓ | ✓ |
| Cambio de proveedor preferido | | ✓ | ✓ | ✓ | |
| Ajuste dinámico aplicado | | ✓ | ✓ | ✓ | |
| Ventana CPD modificada | ✓ | ✓ | ✓ | ✓ | |

- Estrategia para productos nuevos sin historial (CPD manual → transición a CPD calculado).
- Estrategia de concurrencia: qué pasa si llegan múltiples eventos simultáneos para el mismo producto.
- Estrategia de idempotencia en recálculos.
- Diseño del sistema de snapshots diarios (job que persiste estado de buffers/KPIs cada día).

**Diagramas Mermaid obligatorios:**
1. **Diagrama de flujo del pipeline DDMRP:** Desde una transacción entrante hasta la actualización de sugerencias de reposición.
2. **Diagrama de secuencia: OV efectuada** — Mostrar paso a paso: registro → evento → recálculo CPD → recálculo zonas → recálculo flujo neto → evaluación sugerencia → evaluación alerta.
3. **Diagrama de secuencia: OC recibida (parcial)** — Mostrar: registro → actualización inventario en tránsito → actualización inventario físico → recálculo flujo neto → evaluación de desvío de LT → alerta si aplica.
4. **Diagrama de secuencia: Recálculo completo de un producto** — Desde CPD hasta sugerencia final, mostrando cada paso intermedio y los datos que consume.
5. **Diagrama de estados del Buffer** — Transiciones entre zonas (Verde → Amarillo → Rojo base → Rojo seguridad) y los eventos que las causan.
6. **Diagrama del Event Bus interno** — Publishers, subscribers, tipos de eventos, y flujo de propagación.

---

#### SECCIÓN 4: Diseño de APIs

**Contenido obligatorio:**
- Diseño de la API REST completa del backend Go.
- Agrupación de endpoints por módulo/dominio.
- Convenciones de naming, versionado, paginación, filtrado, ordenamiento.
- Estrategia de autenticación (JWT) y cómo se inyecta el `tenant_id` en cada request.
- Rate limiting por tenant.
- Manejo de errores estandarizado.
- Endpoints de polling para el frontend (dashboard state, alerts count, etc.).
- Lista completa de endpoints con método HTTP, path, descripción, request/response resumido, y permisos por rol.

**Diagramas Mermaid obligatorios:**
1. **Diagrama de secuencia: Flujo de autenticación** — Login → JWT → Request autenticado → Extracción de tenant_id → RLS.
2. **Diagrama de secuencia: Flujo de polling del dashboard** — Frontend solicita estado → Backend consulta datos precalculados → Respuesta con semáforo + KPIs + alertas.
3. **Diagrama de secuencia: Flujo de creación de OC desde sugerencia de reposición** — Desde "seleccionar sugerencias" hasta "OC creada".
4. **Diagrama de secuencia: Flujo de importación CSV** — Upload → Validación → Preview → Confirmación → Procesamiento background → Notificación.

---

#### SECCIÓN 5: Diseño del Frontend

**Contenido obligatorio:**
- Arquitectura de la aplicación Next.js (App Router, estructura de carpetas).
- Estrategia de state management.
- Estrategia de polling (intervalos por tipo de dato, backoff, pausa cuando tab no visible).
- Mapa completo de pantallas/vistas con su ruta, permisos requeridos, y datos que consume de la API.
- Diseño del sistema de semáforo visual (colores, accesibilidad para daltonismo).
- Componentes principales reutilizables.
- Estrategia de formularios (validación client-side, autocompletado fuzzy).
- Estrategia de manejo de errores y feedback visual (< 200ms según RNF-013).

**Diagramas Mermaid obligatorios:**
1. **Mapa de navegación completo** — Todas las rutas/pantallas y cómo se conectan, incluyendo guards de permisos por rol.
2. **Diagrama de componentes del Dashboard** — Composición de widgets: semáforo, lista críticos, KPIs, filtros.
3. **Diagrama de flujo: Creación de producto completo** — Paso a paso desde el formulario hasta la confirmación, incluyendo asignación de proveedores y perfil de buffer automático.

---

#### SECCIÓN 6: Seguridad y Multi-tenancy

**Contenido obligatorio:**
- Diseño detallado de Row-Level Security (RLS) en PostgreSQL: políticas, variables de sesión, performance implications.
- Flujo completo de autenticación y autorización.
- Matriz de permisos completa: qué puede hacer cada rol (Admin, Usuario, Solo Lectura) en cada módulo.
- Protección contra ataques comunes (SQL injection, XSS, CSRF, brute force en login).
- Estrategia de sesiones (JWT con expiración, refresh tokens, expiración por inactividad de 30 min según RNF-007).
- Hashing de contraseñas (bcrypt según RNF-005).
- Auditoría de operaciones de escritura (RNF-009).
- HTTPS/TLS enforcement (RNF-006).

**Diagramas Mermaid obligatorios:**
1. **Diagrama de secuencia: Request completo con RLS** — Desde HTTP request → middleware auth → set session variable → query con RLS → response filtrada por tenant.
2. **Diagrama de la matriz de permisos** — Tabla visual de roles × módulos × acciones (CRUD).

---

#### SECCIÓN 7: Flujos de Negocio End-to-End

**Contenido obligatorio:**
- Documentar los flujos de negocio más críticos del sistema de principio a fin, mostrando la interacción entre todos los módulos involucrados.

**Diagramas Mermaid obligatorios:**
1. **Flujo E2E: Onboarding de un nuevo tenant** — Registro → Configuración inicial (monedas, umbrales LT, ventana CPD) → Importación CSV de productos → Importación de histórico de ventas → Cálculo inicial de CPD → Cálculo de zonas → Dashboard operativo.
2. **Flujo E2E: Día típico de operación** — Usuario abre dashboard → Revisa alertas → Va a planificación → Selecciona sugerencias → Crea OC → Registra ventas del día → Registra recepción de OC anterior → Revisa KPIs al final del día.
3. **Flujo E2E: Ciclo de vida de un producto** — Creación → Asignación de proveedores → Buffer profile automático → Primera venta → CPD calculado → Zonas calculadas → Flujo neto activo → Sugerencia generada → OC creada → OC recibida → Ajuste dinámico por temporada → Desactivación.
4. **Flujo E2E: Ciclo de vida de una Orden de Compra** — Sugerencia → Draft OC → OC Pendiente → Recepción parcial → Recepción completa → Impacto en inventario + flujo neto + KPIs.
5. **Flujo E2E: Detección y resolución de rotura de stock** — Venta grande → Flujo neto cae a zona roja → Alerta generada → Usuario ve dashboard → Va a planificación → Crea OC urgente → OC llega → Stock se recupera → Alerta resuelta automáticamente.

---

#### SECCIÓN 8: Estrategia de Datos y Performance

**Contenido obligatorio:**
- Estimaciones de volumen de datos por tenant (basado en RNF-018: 10,000 productos, 100,000 transacciones).
- Estrategia de índices para las queries más frecuentes (dashboard, planificación, búsquedas).
- Estrategia de datos precalculados/materializados (ej: flujo neto, estado de buffer, KPIs) vs calculados on-the-fly.
- Estrategia de paginación y filtrado eficiente.
- Plan para cumplir RNF-001 (dashboard < 3s), RNF-002 (CRUD < 500ms), RNF-003 (recálculo 1000 productos < 10s).
- Estrategia de procesamiento de CSV en background (hasta 5000 registros < 60s según RNF-004).
- Job scheduler para snapshots diarios de KPIs y estado de buffers.
- Estrategia de cleanup/archivado de datos históricos a largo plazo.

**Diagramas Mermaid obligatorios:**
1. **Diagrama de flujo: Procesamiento de CSV** — Upload → Parsing → Validación → Preview → Confirmación → Bulk insert → Eventos de recálculo → Notificación.
2. **Diagrama de flujo: Job de snapshot diario** — Trigger → Iterar productos por tenant → Calcular KPIs → Persistir snapshots → Log.

---

### REGLAS DE FORMATO

1. **Cada sección debe comenzar con un header H1 con el número y título de la sección.**
2. **Todos los diagramas DEBEN ser Mermaid válido y renderizable.** Verificar sintaxis.
3. **Cada diagrama debe tener un título descriptivo y una leyenda/explicación debajo.**
4. **Usar tablas para información estructurada** (endpoints, permisos, eventos, etc.).
5. **Cada decisión de diseño debe incluir: decisión, alternativas consideradas, justificación.**
6. **Referenciar los IDs de requerimientos (RF-XXX, RNF-XXX, HU-XXX) donde corresponda** para mantener trazabilidad.
7. **Usar admonitions/callouts** para advertencias, decisiones importantes, y trade-offs.
8. **No usar emojis.** Mantener tono técnico profesional.
9. **Los nombres de tablas, campos, endpoints, y variables deben estar en inglés** (convención de código). Las explicaciones en español.

---

### RESTRICCIONES DE CALIDAD

- **Completitud:** TODAS las 20 historias de usuario deben estar cubiertas en el diseño. Verificar al final que no quede ninguna HU sin representación en al menos un diagrama o sección.
- **Consistencia:** Los nombres de entidades, campos, y endpoints deben ser consistentes entre TODAS las secciones.
- **Trazabilidad:** Cada decisión importante debe referenciar al menos un RF, RNF, o HU.
- **Pragmatismo:** Es un MVP. Favorecer simplicidad y velocidad de desarrollo, pero sin comprometer la escalabilidad.
- **Validez Mermaid:** Cada bloque de código Mermaid debe ser sintácticamente correcto. Preferir `graph TD`, `sequenceDiagram`, `erDiagram`, `stateDiagram-v2`, y `flowchart TD`.

---

### INSTRUCCIÓN FINAL

Genera las 8 secciones completas del documento de diseño. Asegúrate de que:

1. Cada sección sea autocontenida pero coherente con las demás.
2. Todos los diagramas Mermaid sean válidos y estén explicados.
3. El modelo de datos cubra TODAS las entidades necesarias para los 105 requerimientos funcionales.
4. Los flujos de secuencia muestren interacción real entre componentes (no sean genéricos).
5. La arquitectura refleje un monolito modular en Go con event bus interno, preparado para extracción a microservicios.
6. La seguridad multi-tenant con RLS esté integrada transversalmente en todo el diseño.
7. El motor DDMRP esté diseñado con la profundidad que merece siendo el core del sistema.
8. Al finalizar, incluye una **Matriz de Trazabilidad** que mapee cada HU a las secciones del diseño donde está cubierta.

**Comienza generando la Sección 1 y continúa secuencialmente hasta la Sección 8 + Matriz de Trazabilidad.**

---

_FIN DEL PROMPT_
