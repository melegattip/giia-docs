# SRS: GIIA - Gestión de Inventario con Inteligencia Artificial

> **Versión:** 1.0  
> **Fecha:** 2026-01-30  
> **Estado:** Completado - Pendiente de validación

---

## Resumen Ejecutivo

GIIA es un sistema de gestión de inventario basado en la metodología **Demand Driven MRP (DDMRP)**, diseñado específicamente para comercios minoristas, distribuidores e importadores pymes. El producto transforma la compleja tarea de determinar qué, cuánto y cuándo comprar en un proceso visual e intuitivo mediante un sistema de semáforo (Rojo/Amarillo/Verde). Operando bajo un modelo SaaS Multi-tenant, GIIA automatiza los cálculos de buffers, demanda calificada y flujo neto, proporcionando sugerencias de reposición precisas que optimizan el capital de trabajo y reducen las roturas de stock. El MVP se enfoca en una posición estratégica única (almacén o punto de venta), democratizando el acceso a DDMRP para el sector retail/distribución.

---

## 1. Introducción

### 1.1 Propósito

Este documento especifica los requerimientos funcionales y no funcionales del producto GIIA (Gestión de Inventario con Inteligencia Artificial) en su versión MVP. Sirve como referencia principal para el equipo de desarrollo, asegurando una comprensión compartida de lo que se construirá y cómo se validará su correcta implementación.

### 1.2 Alcance del Producto

#### Dentro del Alcance (In-Scope)

**Gestión de Datos Maestros:**
- [x] Gestión de Proveedores (Lead Time, Razón Social, Ubicación)
- [x] Gestión de Categorías (Nombre, Descripción)
- [x] Gestión de Productos (SKU, LT por proveedor, MOQ, Frecuencia de pedido, Costo unitario)
- [x] Soporte para múltiples proveedores por producto

**Transacciones de Inventario:**
- [x] Registro de Órdenes de Compra (OC) vigentes y efectuadas
- [x] Registro de Órdenes de Venta en firme y efectuadas
- [x] Ingresos y Egresos extraordinarios (devoluciones, pérdidas, ajustes)
- [x] Seguimiento de estado de OC (vigente, efectuada)

**Motor DDMRP:**
- [x] Cálculo de CPD (Consumo Promedio Diario) con ventana configurable (7 días por defecto)
- [x] Perfiles de Buffer (configuración de zonas según LT, Variabilidad, MOQ, Frecuencia)
- [x] Cálculo de Zonas del Buffer (Rojo, Amarillo, Verde)
- [x] Ecuación de Flujo Neto (Inventario físico + En tránsito - Demanda calificada)
- [x] Sugerencias de Reposición (qué comprar, cuánto, cuándo)
- [x] Demanda Calificada (OV en firme + picos calificados con umbral configurable)

**Ajustes Dinámicos (Should):**
- [x] Ajustes recalculados automáticos (CPD, LT, cambios de perfil)
- [x] Factores de Ajuste de Demanda (FAD)
- [x] Factores de Ajuste de Zona (FAZ)
- [x] Factores de Ajuste de Lead Time (FALTD)

**Visualización y Ejecución:**
- [x] Dashboard de estado de inventario con semáforo visual
- [x] Pantalla de planificación con priorización por urgencia
- [x] Alertas configurables por desvíos en llegada de mercancía

**KPIs (4 indicadores):**
- [x] Costo Total de Inventario
- [x] Días en Inventario Valorizado
- [x] Inventario Inmovilizado / Obsolescencia
- [x] Rotación de Inventario

**Entrada de Datos:**
- [x] Input manual con interfaz web
- [x] Importación masiva vía CSV

**Autenticación y Autorización:**
- [x] Autenticación Email/Password
- [x] Roles básicos (Admin, Usuario, Solo Lectura)

**Configuración:**
- [x] Soporte multi-moneda (USD y Peso local)
- [x] Modelo SaaS Multi-tenant

#### Fuera del Alcance (Out-of-Scope)

| Elemento Excluido | Justificación |
|-------------------|---------------|
| **Integración API Manager ERP** | Requiere discovery adicional con proveedores de ERP específicos. Se evaluará post-validación del modelo de datos |
| **Integración Odoo** | Complejidad de integración justifica desarrollo post-MVP, una vez validado el producto core |
| **Sistema de Suscripciones y Pagos** | El MVP se enfocará en validar el valor del producto. Monetización se implementará post-MVP |
| **Gamificación** | Feature de engagement que se implementará una vez validada la retención de usuarios |
| **Autenticación 2FA** | Feature de seguridad avanzada. Prioridad "Should" - se evaluará según recursos disponibles |
| **Forecasting Personalizado** | Excluido del MVP por decisión del Product Owner |
| **Múltiples posiciones estratégicas** | El MVP se enfoca en una posición única (almacén/punto de venta) |

---

## 2. Objetivos de Negocio

| ID | Objetivo | Métrica de Éxito | Valor Actual | Valor Objetivo |
|----|----------|------------------|--------------|----------------|
| OBJ-001 | Reducir roturas de stock | % de SKUs en zona roja crítica | N/A (sin medición actual) | < 5% del inventario activo |
| OBJ-002 | Optimizar capital de trabajo | Rotación de inventario mensual | Variable por cliente | Incremento del 20% vs. baseline |
| OBJ-003 | Reducir inventario inmovilizado | % de productos con más de X días en stock | Variable por cliente | Reducción del 30% vs. baseline |
| OBJ-004 | Acelerar toma de decisiones | Tiempo para generar orden de compra | Manual (horas) | < 5 minutos por proveedor |
| OBJ-005 | Mejorar visibilidad de inventario | Usuarios con dashboard actualizado diariamente | 0% | 100% de usuarios activos |
| OBJ-006 | Automatizar cálculos DDMRP | Productos con buffers calculados automáticamente | 0% | 100% del catálogo activo |

---

## 3. Requerimientos Funcionales

### 3.1 Gestión de Proveedores

Módulo para administrar la información de proveedores, incluyendo datos de contacto, ubicación y tiempos de entrega.

**Objetivos relacionados:** OBJ-004, OBJ-006

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-001 | Crear proveedor | Must | El sistema debe permitir crear un nuevo proveedor con: Razón Social, Lead Time promedio, Ubicación, Datos de contacto, y Estado (activo/inactivo) |
| RF-002 | Editar proveedor | Must | El sistema debe permitir modificar todos los datos de un proveedor existente |
| RF-003 | Listar proveedores | Must | El sistema debe mostrar una lista paginada de proveedores con búsqueda y filtros por estado |
| RF-004 | Desactivar proveedor | Must | El sistema debe permitir desactivar un proveedor sin eliminarlo, preservando historial de transacciones |

### 3.2 Gestión de Categorías

Módulo para organizar productos en categorías jerárquicas.

**Objetivos relacionados:** OBJ-006

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-005 | Crear categoría | Must | El sistema debe permitir crear una categoría con: Nombre, Descripción, y Categoría padre (opcional) |
| RF-006 | Editar categoría | Must | El sistema debe permitir modificar nombre y descripción de una categoría |
| RF-007 | Listar categorías | Must | El sistema debe mostrar las categorías en formato jerárquico (árbol) |
| RF-008 | Eliminar categoría | Must | El sistema debe permitir eliminar categorías sin productos asociados. Si tiene productos, debe mostrar advertencia |

### 3.3 Gestión de Productos

Módulo central para administrar el catálogo de productos con sus atributos DDMRP.

**Objetivos relacionados:** OBJ-002, OBJ-003, OBJ-004, OBJ-006

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-009 | Crear producto | Must | El sistema debe permitir crear un producto con: SKU (único), Nombre, Descripción, Categoría, Costo unitario, Unidad de medida, y Estado (activo/inactivo) |
| RF-010 | Asignar proveedores a producto | Must | El sistema debe permitir asignar múltiples proveedores a un producto, cada uno con: Lead Time específico, MOQ (Cantidad Mínima de Orden), Frecuencia de pedido, y Proveedor preferido (flag) |
| RF-011 | Configurar perfil de buffer por producto | Must | El sistema debe permitir asignar un perfil de buffer a cada producto para el cálculo de zonas |
| RF-012 | Editar producto | Must | El sistema debe permitir modificar todos los atributos de un producto existente |
| RF-013 | Listar productos | Must | El sistema debe mostrar lista paginada de productos con búsqueda por SKU/nombre, filtros por categoría, proveedor, estado de buffer |
| RF-014 | Ver detalle de producto | Must | El sistema debe mostrar vista detallada de producto incluyendo: datos maestros, proveedores asignados, inventario actual, historial de transacciones, y estado de buffer actual |
| RF-015 | Desactivar producto | Must | El sistema debe permitir desactivar productos sin eliminarlos, preservando historial |
| RF-016 | Comportamiento producto nuevo | Must | Para productos sin histórico de ventas, el sistema debe aplicar comportamiento estable (lineal) hasta disponer de información suficiente |

### 3.4 Transacciones - Órdenes de Compra

Módulo para gestionar las órdenes de compra y su ciclo de vida.

**Objetivos relacionados:** OBJ-001, OBJ-002, OBJ-004

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-017 | Crear orden de compra | Must | El sistema debe permitir crear una OC con: Proveedor, Fecha de emisión, Fecha estimada de llegada (calculada por LT), Lista de productos con cantidad y costo, y Estado inicial "Pendiente" |
| RF-018 | Calcular fecha estimada de llegada | Must | El sistema debe calcular automáticamente la fecha estimada de llegada sumando el Lead Time del proveedor a la fecha de emisión |
| RF-019 | Editar orden de compra pendiente | Must | El sistema debe permitir modificar OC en estado "Pendiente": agregar/quitar productos, modificar cantidades |
| RF-020 | Registrar recepción de OC | Must | El sistema debe permitir registrar la recepción de una OC: fecha real de llegada, cantidades recibidas (puede ser parcial), generar ingreso automático a inventario |
| RF-021 | Cancelar orden de compra | Must | El sistema debe permitir cancelar una OC pendiente, registrando motivo de cancelación |
| RF-022 | Listar órdenes de compra | Must | El sistema debe mostrar lista de OC con filtros por: estado, proveedor, rango de fechas, productos incluidos |
| RF-023 | Ver detalle de OC | Must | El sistema debe mostrar detalle de OC incluyendo: datos generales, líneas de productos, estado, y trazabilidad de cambios |

### 3.5 Transacciones - Órdenes de Venta

Módulo para gestionar las órdenes de venta que afectan la demanda calificada.

**Objetivos relacionados:** OBJ-001, OBJ-004

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-024 | Registrar orden de venta | Must | El sistema debe permitir registrar una OV con: Cliente (opcional), Fecha, Lista de productos con cantidad, y Estado (en firme / efectuada) |
| RF-025 | Marcar OV como efectuada | Must | El sistema debe permitir marcar una OV en firme como efectuada, generando automáticamente el egreso de inventario |
| RF-026 | Cancelar orden de venta | Must | El sistema debe permitir cancelar una OV en firme, liberando la demanda calificada |
| RF-027 | Listar órdenes de venta | Must | El sistema debe mostrar lista de OV con filtros por estado, rango de fechas, productos |

### 3.6 Transacciones - Ingresos y Egresos Extraordinarios

Módulo para registrar movimientos de inventario que no corresponden a compras/ventas normales.

**Objetivos relacionados:** OBJ-003, OBJ-005

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-028 | Registrar ingreso extraordinario | Must | El sistema debe permitir registrar ingresos por: devoluciones de clientes, ajustes de inventario positivos, traspasos. Debe incluir tipo, productos, cantidades, y justificación |
| RF-029 | Registrar egreso extraordinario | Must | El sistema debe permitir registrar egresos por: pérdidas, mermas, obsolescencia, ajustes negativos, muestras. Debe incluir tipo, productos, cantidades, y justificación |
| RF-030 | Listar movimientos extraordinarios | Must | El sistema debe mostrar historial de movimientos extraordinarios con filtros por tipo, fecha, producto |

### 3.7 Motor DDMRP - Consumo Promedio Diario (CPD)

Cálculos del consumo promedio diario, base de la metodología DDMRP.

**Objetivos relacionados:** OBJ-001, OBJ-006

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-031 | Calcular CPD por producto | Must | El sistema debe calcular el CPD automáticamente basándose en el consumo histórico dentro de la ventana de tiempo configurada |
| RF-032 | Configurar ventana de CPD | Must | El sistema debe permitir configurar la ventana de tiempo para el cálculo del CPD. Valor por defecto: 7 días. Editable por usuario |
| RF-033 | Mostrar CPD en vista de producto | Must | El sistema debe mostrar el CPD calculado en la vista de detalle de cada producto |
| RF-034 | Recalcular CPD automáticamente | Must | El sistema debe recalcular el CPD automáticamente cuando se registren nuevas transacciones de venta/egreso |

### 3.8 Motor DDMRP - Perfiles de Buffer

Configuración de perfiles de buffer para determinar el comportamiento de las zonas.

**Objetivos relacionados:** OBJ-006

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-035 | Crear perfil de buffer | Must | El sistema debe permitir crear perfiles de buffer con: Nombre, Categoría de Lead Time (Corto/Medio/Largo - umbrales configurables), Factor de Lead Time, Factor de Variabilidad |
| RF-036 | Definir umbrales de Lead Time | Must | El sistema debe permitir configurar los umbrales que definen las categorías de Lead Time (Corto/Medio/Largo) durante el setup. Valores configurados por usuario |
| RF-037 | Editar perfil de buffer | Must | El sistema debe permitir modificar los parámetros de un perfil de buffer existente |
| RF-038 | Listar perfiles de buffer | Must | El sistema debe mostrar lista de perfiles de buffer con sus parámetros |
| RF-039 | Eliminar perfil de buffer | Must | El sistema debe permitir eliminar perfiles de buffer que no estén asignados a productos |

### 3.9 Motor DDMRP - Cálculo de Zonas

Cálculo automático de las zonas del buffer (Rojo, Amarillo, Verde).

**Objetivos relacionados:** OBJ-001, OBJ-005, OBJ-006

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-040 | Calcular zona roja | Must | El sistema debe calcular la zona roja siguiendo la fórmula DDMRP: Base Roja = CPD × LT × Factor LT. Plus Roja = CPD × LT × Factor Variabilidad. Zona Roja = Base + Plus |
| RF-041 | Calcular zona amarilla | Must | El sistema debe calcular la zona amarilla: Zona Amarilla = CPD × LT |
| RF-042 | Calcular zona verde | Must | El sistema debe calcular la zona verde considerando: el mayor entre (CPD × LT × Factor LT), MOQ, o Ciclo de pedido en días |
| RF-043 | Calcular TOG | Must | El sistema debe calcular el Top of Green (TOG) = Zona Roja + Zona Amarilla + Zona Verde |
| RF-044 | Visualizar buffer | Must | El sistema debe mostrar visualmente el buffer de cada producto como una barra dividida en zonas Roja/Amarilla/Verde con el nivel de inventario actual superpuesto |
| RF-045 | Recalcular zonas automáticamente | Must | El sistema debe recalcular las zonas cuando cambien: CPD, Lead Time, Perfil de buffer, MOQ, o Frecuencia de pedido |

### 3.10 Motor DDMRP - Ecuación de Flujo Neto

Cálculo del flujo neto para determinar la posición real de inventario.

**Objetivos relacionados:** OBJ-001, OBJ-002, OBJ-004

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-046 | Calcular inventario físico | Must | El sistema debe mantener actualizado el inventario físico de cada producto basándose en todas las transacciones registradas |
| RF-047 | Calcular inventario en tránsito | Must | El sistema debe calcular el inventario en tránsito sumando las cantidades de OC pendientes de recepción |
| RF-048 | Calcular demanda calificada | Must | El sistema debe calcular la demanda calificada sumando: OV en firme (no efectuadas) + Picos calificados (pedidos que superan el umbral de pico). Umbral por defecto: 50% de zona roja, editable |
| RF-049 | Configurar umbral de pico | Must | El sistema debe permitir configurar el umbral de pico para identificar demanda extraordinaria. Valor por defecto: 50% de la zona roja |
| RF-050 | Calcular flujo neto | Must | El sistema debe calcular: Flujo Neto = Inventario Físico + En Tránsito - Demanda Calificada |
| RF-051 | Determinar estado de buffer | Must | El sistema debe determinar el estado del buffer comparando el flujo neto con las zonas: Rojo (crítico/base), Amarillo, Verde |
| RF-052 | Actualizar flujo neto en tiempo real | Must | El sistema debe actualizar el flujo neto automáticamente cuando cambie cualquier componente de la ecuación |

### 3.11 Motor DDMRP - Sugerencias de Reposición

Generación de sugerencias de compra basadas en el estado de los buffers.

**Objetivos relacionados:** OBJ-001, OBJ-002, OBJ-004

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-053 | Generar sugerencia de reposición | Must | El sistema debe generar sugerencias de reposición cuando el flujo neto esté en zona Roja o Amarilla. La cantidad sugerida = TOG - Flujo Neto |
| RF-054 | Priorizar sugerencias | Must | El sistema debe priorizar las sugerencias por profundidad en zona roja: mayor penetración = mayor prioridad |
| RF-055 | Agrupar sugerencias por proveedor | Must | El sistema debe permitir agrupar las sugerencias de reposición por proveedor para facilitar la generación de OC |
| RF-056 | Convertir sugerencia en OC | Must | El sistema debe permitir convertir una o más sugerencias de reposición en una orden de compra con un clic |
| RF-057 | Ajustar cantidad sugerida | Must | El sistema debe permitir ajustar la cantidad sugerida antes de generar la OC, respetando MOQ mínimo |

### 3.12 Ajustes Dinámicos de Buffer

Ajustes manuales y automáticos para adaptar los buffers a situaciones especiales.

**Objetivos relacionados:** OBJ-001, OBJ-002

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-058 | Aplicar Factor de Ajuste de Demanda (FAD) | Should | El sistema debe permitir aplicar un FAD temporal a productos específicos para anticipar cambios de demanda (ej: temporadas, promociones) |
| RF-059 | Aplicar Factor de Ajuste de Zona (FAZ) | Should | El sistema debe permitir ajustar temporalmente el tamaño de las zonas del buffer para productos específicos |
| RF-060 | Aplicar Factor de Ajuste de Lead Time (FALTD) | Should | El sistema debe permitir ajustar temporalmente el Lead Time considerado para el cálculo de zonas |
| RF-061 | Programar vigencia de ajustes | Should | El sistema debe permitir programar fecha de inicio y fin para los factores de ajuste, revertiendo automáticamente al vencer |
| RF-062 | Recalcular ajustes automáticos | Should | El sistema debe recalcular automáticamente los buffers cuando cambien datos base (CPD, LT, perfil) |

### 3.13 Visualización - Dashboard de Inventario

Panel principal de visualización del estado del inventario.

**Objetivos relacionados:** OBJ-001, OBJ-005

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-063 | Mostrar semáforo general | Must | El sistema debe mostrar un resumen visual del estado del inventario: % de SKUs en cada zona (Rojo/Amarillo/Verde) |
| RF-064 | Mostrar lista de productos en zona roja | Must | El sistema debe mostrar prominentemente los productos en zona roja, ordenados por criticidad (profundidad en zona) |
| RF-065 | Filtrar dashboard por categoría | Must | El sistema debe permitir filtrar la vista del dashboard por categoría de productos |
| RF-066 | Filtrar dashboard por proveedor | Must | El sistema debe permitir filtrar la vista del dashboard por proveedor |
| RF-067 | Ver tendencia de buffer | Should | El sistema debe mostrar la tendencia del flujo neto de un producto en los últimos N días |
| RF-068 | Actualización del dashboard | Must | El sistema debe actualizar el dashboard automáticamente cuando cambien datos relevantes |

### 3.14 Visualización - Pantalla de Planificación

Vista de trabajo para planificar y ejecutar reposiciones.

**Objetivos relacionados:** OBJ-004

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-069 | Lista de reposiciones pendientes | Must | El sistema debe mostrar lista de productos que requieren reposición, ordenados por prioridad |
| RF-070 | Agrupar por proveedor | Must | El sistema debe permitir agrupar la lista de reposiciones por proveedor |
| RF-071 | Selección múltiple para OC | Must | El sistema debe permitir seleccionar múltiples productos para generar una OC consolidada |
| RF-072 | Vista de calendario de llegadas | Should | El sistema debe mostrar calendario con fechas estimadas de llegada de OC pendientes |

### 3.15 Alertas

Sistema de notificaciones para mantener informado al usuario.

**Objetivos relacionados:** OBJ-001, OBJ-005

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-073 | Alerta de producto en zona roja | Must | El sistema debe alertar cuando un producto entre en zona roja |
| RF-074 | Alerta de llegada próxima | Must | El sistema debe alertar cuando una OC esté próxima a su fecha de llegada estimada. Días de anticipación configurables por usuario |
| RF-075 | Alerta de llegada vencida | Must | El sistema debe alertar con prioridad alta cuando una OC supere su fecha de llegada estimada sin haberse recibido |
| RF-076 | Configurar alertas de desvío | Must | El sistema debe permitir configurar % o cantidad de días previos para alertas de llegada |
| RF-077 | Centro de notificaciones | Must | El sistema debe proveer un centro de notificaciones donde el usuario pueda ver todas las alertas activas |
| RF-078 | Marcar alerta como leída | Must | El sistema debe permitir marcar alertas como leídas/atendidas |

### 3.16 KPIs - Indicadores de Gestión

Métricas clave para evaluar el desempeño del inventario.

**Objetivos relacionados:** OBJ-002, OBJ-003, OBJ-005

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-079 | KPI Costo Total de Inventario | Must | El sistema debe calcular y mostrar el costo total del inventario actual (suma de inventario físico × costo unitario) |
| RF-080 | KPI Días en Inventario Valorizado | Must | El sistema debe calcular y mostrar los días promedio que el inventario permanece en stock |
| RF-081 | KPI Inventario Inmovilizado | Must | El sistema debe calcular y mostrar el valor del inventario inmovilizado. Criterio: productos que ingresaron antes de las últimas 2 OC |
| RF-082 | Configurar criterio de inmovilizado | Must | El sistema debe permitir editar el criterio para considerar inventario como inmovilizado |
| RF-083 | KPI Rotación de Inventario | Must | El sistema debe calcular y mostrar la rotación de inventario (ventas / inventario promedio) |
| RF-084 | Visualizar KPIs en dashboard | Must | El sistema debe mostrar los 4 KPIs en el dashboard principal con comparativa vs. período anterior |
| RF-085 | Exportar KPIs | Should | El sistema debe permitir exportar los KPIs a formato CSV o PDF |

### 3.17 Entrada de Datos - Input Manual

Formularios para ingreso manual de datos.

**Objetivos relacionados:** OBJ-004, OBJ-005

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-086 | Formularios de datos maestros | Must | El sistema debe proveer formularios intuitivos para crear/editar proveedores, categorías, productos |
| RF-087 | Formularios de transacciones | Must | El sistema debe proveer formularios para registrar OC, OV, ingresos y egresos extraordinarios |
| RF-088 | Validación en tiempo real | Must | El sistema debe validar los datos ingresados en tiempo real, mostrando errores antes de guardar |
| RF-089 | Autocompletado | Must | El sistema debe proveer autocompletado para campos de referencia (proveedor, producto, categoría) |

### 3.18 Entrada de Datos - Importación CSV

Carga masiva de datos mediante archivos CSV.

**Objetivos relacionados:** OBJ-004, OBJ-006

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-090 | Importar productos por CSV | Must | El sistema debe permitir importar productos masivamente desde archivo CSV con mapeo de columnas |
| RF-091 | Importar transacciones por CSV | Must | El sistema debe permitir importar histórico de transacciones (ventas, compras) desde CSV |
| RF-092 | Validar CSV antes de importar | Must | El sistema debe validar el archivo CSV y mostrar preview con errores detectados antes de confirmar importación |
| RF-093 | Descargar templates CSV | Must | El sistema debe proveer templates CSV descargables para cada tipo de importación |
| RF-094 | Log de importación | Must | El sistema debe mantener log de importaciones realizadas con resultado (éxito/errores) |

### 3.19 Autenticación y Autorización

Control de acceso y seguridad.

**Objetivos relacionados:** OBJ-005

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-095 | Login Email/Password | Must | El sistema debe permitir autenticación mediante email y contraseña |
| RF-096 | Recuperar contraseña | Must | El sistema debe proveer flujo de recuperación de contraseña vía email |
| RF-097 | Rol Admin | Must | El sistema debe soportar rol Admin con acceso completo: configuración, usuarios, datos maestros, transacciones |
| RF-098 | Rol Usuario | Must | El sistema debe soportar rol Usuario con acceso a: datos maestros, transacciones, dashboard, planificación |
| RF-099 | Rol Solo Lectura | Must | El sistema debe soportar rol Solo Lectura con acceso únicamente a visualización: dashboard, reportes, consultas |
| RF-100 | Gestionar usuarios | Must | El sistema debe permitir a Admin crear, editar, desactivar usuarios y asignar roles |
| RF-101 | Aislamiento de datos por tenant | Must | El sistema debe garantizar que cada organización (tenant) solo pueda ver y modificar sus propios datos |

### 3.20 Configuración Multi-moneda

Soporte para operaciones en múltiples monedas.

**Objetivos relacionados:** OBJ-002, OBJ-003

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-102 | Configurar monedas | Must | El sistema debe permitir configurar las monedas disponibles: USD y Peso local |
| RF-103 | Asignar moneda a transacciones | Must | El sistema debe permitir registrar transacciones en cualquiera de las monedas configuradas |
| RF-104 | Tipo de cambio | Must | El sistema debe permitir configurar tipo de cambio entre monedas para valorización |
| RF-105 | Consolidar en moneda base | Must | El sistema debe consolidar valores de KPIs y reportes en la moneda base configurada |

---

## 4. Historias de Usuario

Las historias de usuario completas se encuentran en `docs/requirements/user-stories/`. A continuación se presenta un resumen con enlaces.

### 4.1 Gestión de Datos Maestros

| ID | Título | Prioridad | RF Relacionados | Archivo |
|----|--------|-----------|-----------------|---------|
| HU-001 | Gestionar proveedores | Must | RF-001, RF-002, RF-003, RF-004 | [HU-001.md](../user-stories/HU-001.md) |
| HU-002 | Organizar productos en categorías | Must | RF-005, RF-006, RF-007, RF-008 | [HU-002.md](../user-stories/HU-002.md) |
| HU-003 | Gestionar catálogo de productos | Must | RF-009, RF-010, RF-011, RF-012, RF-013, RF-014, RF-015, RF-016 | [HU-003.md](../user-stories/HU-003.md) |

### 4.2 Transacciones

| ID | Título | Prioridad | RF Relacionados | Archivo |
|----|--------|-----------|-----------------|---------|
| HU-004 | Gestionar órdenes de compra | Must | RF-017, RF-018, RF-019, RF-020, RF-021, RF-022, RF-023 | [HU-004.md](../user-stories/HU-004.md) |
| HU-005 | Registrar ventas y demanda | Must | RF-024, RF-025, RF-026, RF-027 | [HU-005.md](../user-stories/HU-005.md) |
| HU-006 | Registrar ajustes de inventario | Must | RF-028, RF-029, RF-030 | [HU-006.md](../user-stories/HU-006.md) |

### 4.3 Motor DDMRP

| ID | Título | Prioridad | RF Relacionados | Archivo |
|----|--------|-----------|-----------------|---------|
| HU-007 | Configurar cálculo de CPD | Must | RF-031, RF-032, RF-033, RF-034 | [HU-007.md](../user-stories/HU-007.md) |
| HU-008 | Configurar perfiles de buffer | Must | RF-035, RF-036, RF-037, RF-038, RF-039 | [HU-008.md](../user-stories/HU-008.md) |
| HU-009 | Visualizar zonas de buffer | Must | RF-040, RF-041, RF-042, RF-043, RF-044, RF-045 | [HU-009.md](../user-stories/HU-009.md) |
| HU-010 | Calcular flujo neto | Must | RF-046, RF-047, RF-048, RF-049, RF-050, RF-051, RF-052 | [HU-010.md](../user-stories/HU-010.md) |
| HU-011 | Obtener sugerencias de reposición | Must | RF-053, RF-054, RF-055, RF-056, RF-057 | [HU-011.md](../user-stories/HU-011.md) |
| HU-012 | Aplicar ajustes dinámicos | Should | RF-058, RF-059, RF-060, RF-061, RF-062 | [HU-012.md](../user-stories/HU-012.md) |

### 4.4 Visualización y Ejecución

| ID | Título | Prioridad | RF Relacionados | Archivo |
|----|--------|-----------|-----------------|---------|
| HU-013 | Visualizar estado de inventario | Must | RF-063, RF-064, RF-065, RF-066, RF-067, RF-068 | [HU-013.md](../user-stories/HU-013.md) |
| HU-014 | Planificar reposiciones | Must | RF-069, RF-070, RF-071, RF-072 | [HU-014.md](../user-stories/HU-014.md) |
| HU-015 | Recibir alertas de inventario | Must | RF-073, RF-074, RF-075, RF-076, RF-077, RF-078 | [HU-015.md](../user-stories/HU-015.md) |
| HU-016 | Monitorear KPIs de inventario | Must | RF-079, RF-080, RF-081, RF-082, RF-083, RF-084, RF-085 | [HU-016.md](../user-stories/HU-016.md) |

### 4.5 Entrada de Datos

| ID | Título | Prioridad | RF Relacionados | Archivo |
|----|--------|-----------|-----------------|---------|
| HU-017 | Ingresar datos manualmente | Must | RF-086, RF-087, RF-088, RF-089 | [HU-017.md](../user-stories/HU-017.md) |
| HU-018 | Importar datos masivamente | Must | RF-090, RF-091, RF-092, RF-093, RF-094 | [HU-018.md](../user-stories/HU-018.md) |

### 4.6 Configuración y Seguridad

| ID | Título | Prioridad | RF Relacionados | Archivo |
|----|--------|-----------|-----------------|---------|
| HU-019 | Acceder al sistema de forma segura | Must | RF-095, RF-096, RF-097, RF-098, RF-099, RF-100, RF-101 | [HU-019.md](../user-stories/HU-019.md) |
| HU-020 | Configurar múltiples monedas | Must | RF-102, RF-103, RF-104, RF-105 | [HU-020.md](../user-stories/HU-020.md) |

---

## 5. Requerimientos No Funcionales

### 5.1 Rendimiento

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-001 | Tiempo de carga del dashboard | Must | El dashboard principal debe cargar completamente en menos de 3 segundos con hasta 1000 productos |
| RNF-002 | Tiempo de respuesta de operaciones | Must | Las operaciones CRUD deben completarse en menos de 500ms |
| RNF-003 | Tiempo de recálculo de buffers | Must | El recálculo de buffers para 1000 productos debe completarse en menos de 10 segundos |
| RNF-004 | Procesamiento de importación CSV | Must | La importación de archivos CSV de hasta 5000 registros debe completarse en menos de 60 segundos |

### 5.2 Seguridad

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-005 | Encriptación de datos sensibles | Must | Las contraseñas deben almacenarse hasheadas con algoritmo bcrypt o equivalente |
| RNF-006 | Comunicaciones seguras | Must | Todas las comunicaciones cliente-servidor deben usar HTTPS/TLS 1.2 o superior |
| RNF-007 | Sesiones seguras | Must | Las sesiones deben expirar después de 30 minutos de inactividad |
| RNF-008 | Aislamiento de datos | Must | Ningún usuario puede acceder a datos de otro tenant bajo ninguna circunstancia |
| RNF-009 | Registro de auditoría | Should | El sistema debe registrar todas las operaciones de escritura con usuario, fecha/hora, y cambios realizados |

### 5.3 Usabilidad

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-010 | Interfaz intuitiva | Must | Un usuario nuevo debe poder completar las tareas básicas (ver dashboard, crear OC) sin capacitación, siguiendo únicamente la interfaz |
| RNF-011 | Sistema de colores semáforo | Must | El sistema de colores Rojo/Amarillo/Verde debe ser consistente en toda la aplicación |
| RNF-012 | Mensajes de error claros | Must | Todos los mensajes de error deben incluir: qué falló, por qué, y cómo corregirlo |
| RNF-013 | Feedback visual | Must | Todas las acciones del usuario deben tener retroalimentación visual en menos de 200ms |
| RNF-014 | Diseño responsive | Must | La interfaz debe ser funcional en resoluciones desde 1280x720 hasta 4K |

### 5.4 Disponibilidad

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-015 | Uptime del sistema | Must | El sistema debe tener disponibilidad >= 99.5% medido mensualmente (excluyendo ventanas de mantenimiento programado) |
| RNF-016 | Mantenimientos programados | Must | Las ventanas de mantenimiento deben notificarse con al menos 24 horas de anticipación |
| RNF-017 | Recuperación de fallos | Should | El sistema debe poder recuperarse de fallos en menos de 15 minutos |

### 5.5 Escalabilidad

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-018 | Capacidad por tenant | Must | El sistema debe soportar hasta 10,000 productos y 100,000 transacciones por tenant |
| RNF-019 | Usuarios concurrentes | Must | El sistema debe soportar al menos 50 usuarios concurrentes por tenant sin degradación de rendimiento |
| RNF-020 | Escalabilidad horizontal | Should | La arquitectura debe permitir escalar horizontalmente agregando instancias |

### 5.6 Compatibilidad

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-021 | Navegadores soportados | Must | Compatible con las últimas 2 versiones de Chrome, Firefox, Safari, y Edge |
| RNF-022 | Formato de exportación | Must | Los datos exportados deben estar en formatos estándar: CSV (UTF-8), PDF |
| RNF-023 | Importación de datos | Must | El sistema debe aceptar archivos CSV con encoding UTF-8 y delimitadores estándar (coma, punto y coma) |

---

## 6. Restricciones

| ID | Restricción | Tipo | Impacto |
|----|-------------|------|---------|
| RES-001 | El MVP debe operar bajo modelo SaaS Multi-tenant | Arquitectura | Requiere diseño de aislamiento de datos por tenant desde el inicio |
| RES-002 | La interfaz debe ser visual e intuitiva, accesible para usuarios no técnicos | UX/UI | Limita complejidad de configuraciones avanzadas en pantallas principales |
| RES-003 | El sistema debe soportar retroalimentación en tiempo real | Técnica | Requiere arquitectura que soporte actualizaciones frecuentes de datos |
| RES-004 | Los cálculos DDMRP deben seguir la metodología estándar | Negocio | Las fórmulas de zonas y flujo neto están definidas y no son negociables |
| RES-005 | El sistema debe calcular fechas de llegada estimadas basándose en Lead Times | Técnica | Requiere precisión en datos de proveedor y fecha de OC |
| RES-006 | Recursos de desarrollo limitados para el MVP | Recursos | Priorización estricta de features "Must" sobre "Should" |

---

## 7. Dependencias

| ID | Dependencia | Tipo | Descripción |
|----|-------------|------|-------------|
| DEP-001 | Perfiles de Buffer requieren Categorías de Lead Time | Interna | RF-035 (Crear perfil de buffer) requiere RF-036 (Definir umbrales de Lead Time) |
| DEP-002 | Productos requieren Proveedores | Interna | RF-010 (Asignar proveedores a producto) requiere RF-001 (Crear proveedor) |
| DEP-003 | Productos requieren Categorías | Interna | RF-009 (Crear producto) requiere RF-005 (Crear categoría) |
| DEP-004 | Zonas de buffer requieren Perfil de Buffer | Interna | RF-040-043 (Cálculo de zonas) requieren RF-011 (Configurar perfil de buffer por producto) |
| DEP-005 | Flujo Neto requiere Zonas calculadas | Interna | RF-050 (Calcular flujo neto) requiere RF-040-045 (Cálculo de zonas) |
| DEP-006 | Sugerencias requieren Flujo Neto | Interna | RF-053 (Generar sugerencia de reposición) requiere RF-050 (Calcular flujo neto) |
| DEP-007 | KPI Inmovilizado requiere Historial de OC | Interna | RF-081 (KPI Inventario Inmovilizado) requiere RF-020 (Registrar recepción de OC) con histórico |
| DEP-008 | Dashboard requiere cálculos DDMRP | Interna | RF-063-068 (Dashboard) requieren RF-040-052 (Motor DDMRP) implementados |
| DEP-009 | Alertas requieren Dashboard | Interna | RF-073-078 (Alertas) requieren RF-063 (Dashboard) y RF-051 (Estado de buffer) |
| DEP-010 | Importación CSV requiere Datos Maestros | Interna | RF-090-091 (Importación CSV) requieren RF-001-016 (Datos Maestros) para validar referencias |

---

## 8. Riesgos

| ID | Riesgo | Probabilidad | Impacto | Mitigación Propuesta |
|----|--------|--------------|---------|----------------------|
| RIE-001 | Usuarios sin datos históricos no pueden usar CPD | Media | Alto | Implementar RF-016 (comportamiento lineal para productos nuevos) y permitir ingreso manual de CPD estimado |
| RIE-002 | Complejidad de fórmulas DDMRP causa errores de cálculo | Media | Alto | Implementar suite completa de pruebas unitarias para motor DDMRP con casos de prueba del Demand Driven Institute |
| RIE-003 | Importación CSV con datos inconsistentes | Alta | Medio | Validación exhaustiva pre-importación (RF-092) con preview de errores y rechazo de registros inválidos |
| RIE-004 | Usuarios no entienden metodología DDMRP | Media | Alto | Incluir tooltips educativos, guía de inicio, y documentación contextual en la interfaz |
| RIE-005 | Lead Times de proveedores muy variables | Alta | Medio | Implementar factores de variabilidad (RF-035) y ajustes dinámicos (RF-058-062) para compensar |
| RIE-006 | Sobrecarga de alertas causa ignorarlas | Media | Medio | Implementar priorización de alertas y configuración de umbrales por usuario (RF-076) |
| RIE-007 | Rendimiento degradado con muchos productos | Media | Alto | Optimizar consultas, implementar paginación, y establecer índices apropiados en base de datos |
| RIE-008 | Pérdida de datos por falla del sistema | Baja | Alto | Implementar backups automáticos diarios y procedimiento de recuperación probado |
| RIE-009 | Tipo de cambio desactualizado afecta KPIs | Media | Bajo | Permitir actualización manual de tipo de cambio y considerar integración con fuente externa post-MVP |
| RIE-010 | Resistencia al cambio de usuarios | Media | Medio | Diseñar interfaz intuitiva (RNF-010), proveer capacitación inicial, y soporte durante adopción |

---

## 9. Glosario

| Término | Definición |
|---------|------------|
| **DDMRP** | Demand Driven Material Requirements Planning. Metodología de planificación de materiales basada en demanda real en lugar de pronósticos. |
| **Buffer** | Nivel de inventario estratégico diseñado para absorber variabilidad en demanda y suministro. Dividido en zonas Roja, Amarilla y Verde. |
| **CPD** | Consumo Promedio Diario. Promedio de unidades consumidas por día en una ventana de tiempo definida. Base del cálculo DDMRP. |
| **Flujo Neto** | Ecuación: Inventario Físico + En Tránsito - Demanda Calificada. Indica la posición real del inventario considerando compromisos. |
| **Zona Roja** | Nivel de seguridad del buffer. Cuando el flujo neto está en esta zona, se requiere acción urgente de reposición. |
| **Zona Amarilla** | Zona de consumo del buffer. Representa el inventario de trabajo normal. |
| **Zona Verde** | Zona de reposición. Define cuánto pedir para volver al nivel óptimo (TOG). |
| **TOG** | Top of Green. Nivel máximo del buffer = Zona Roja + Zona Amarilla + Zona Verde. Objetivo de reposición. |
| **Lead Time (LT)** | Tiempo de entrega. Días desde que se emite una OC hasta que se recibe la mercancía. |
| **MOQ** | Minimum Order Quantity. Cantidad mínima de pedido aceptada por el proveedor. |
| **Demanda Calificada** | Demanda confirmada que debe restarse del inventario disponible: OV en firme más picos calificados. |
| **Pico Calificado** | Orden de venta que supera el umbral de pico (por defecto 50% de zona roja) y representa demanda extraordinaria. |
| **FAD** | Factor de Ajuste de Demanda. Multiplicador temporal para anticipar cambios de demanda (temporadas, promociones). |
| **FAZ** | Factor de Ajuste de Zona. Multiplicador temporal para modificar el tamaño de las zonas del buffer. |
| **FALTD** | Factor de Ajuste de Lead Time a Demanda. Multiplicador para ajustar el LT considerado en cálculos. |
| **OC** | Orden de Compra. Documento que formaliza el pedido a un proveedor. |
| **OV** | Orden de Venta. Documento que registra un pedido de cliente. |
| **Tenant** | Inquilino/organización en modelo SaaS. Cada tenant tiene sus datos aislados de otros. |
| **SKU** | Stock Keeping Unit. Código único que identifica un producto. |
| **Variabilidad** | Medida de dispersión de la demanda o lead time. En GIIA, calculada como desviación estándar relativa al CPD. |
| **Inventario Inmovilizado** | Inventario que no ha rotado en un período definido. En GIIA: productos que ingresaron antes de las últimas 2 OC. |

---

## Checklist de Validación

*Antes de declarar el SRS como completado, verifica que se cumplan todos los criterios:*

### Completitud
- [x] El resumen ejecutivo describe claramente qué se construirá
- [x] Todos los objetivos del Discovery (OBJ-001 a OBJ-006) tienen al menos un RF asociado
- [x] Todos los RF tienen prioridad MoSCoW asignada
- [x] Cada RF tiene al menos una HU asociada
- [x] Cada HU tiene al menos 1 criterio de aceptación

### Requerimientos No Funcionales
- [x] Se han considerado todas las categorías de RNF relevantes (Rendimiento, Seguridad, Usabilidad, Disponibilidad, Escalabilidad, Compatibilidad)
- [x] Cada RNF tiene al menos 1 criterio de aceptación medible

### Trazabilidad
- [x] Las restricciones del Discovery están copiadas (RES-001 a RES-006)
- [x] Las dependencias entre requerimientos están documentadas (DEP-001 a DEP-010)
- [x] Los riesgos están identificados con mitigación propuesta (RIE-001 a RIE-010)

### Calidad de Requerimientos
- [x] Los requerimientos son completos (contienen toda la información necesaria)
- [x] Los requerimientos son consistentes (no se contradicen)
- [x] Los requerimientos son no ambiguos (una sola interpretación)
- [x] Los requerimientos son verificables (se pueden probar)
- [x] Los requerimientos son atómicos (una sola necesidad cada uno)
- [x] Los requerimientos son factibles (realizables con las restricciones)

### Revisión
- [ ] Peer review completado
- [ ] Agent review completado

---

## Señal de Completación

**SRS COMPLETADO - Listo para validación del Orquestador**

---

### Resumen de Entregables

| Documento | Ubicación | Estado |
|-----------|-----------|--------|
| SRS Principal | `docs/requirements/v1/srs.md` | ✅ Completado |
| Historias de Usuario (20) | `docs/requirements/user-stories/HU-001.md` a `HU-020.md` | ✅ Completadas |

### Estadísticas del SRS

| Elemento | Cantidad |
|----------|----------|
| Objetivos de Negocio | 6 (OBJ-001 a OBJ-006) |
| Requerimientos Funcionales | 105 (RF-001 a RF-105) |
| Requerimientos No Funcionales | 23 (RNF-001 a RNF-023) |
| Historias de Usuario | 20 (HU-001 a HU-020) |
| Restricciones | 6 (RES-001 a RES-006) |
| Dependencias | 10 (DEP-001 a DEP-010) |
| Riesgos | 10 (RIE-001 a RIE-010) |
| Módulos/Componentes | 20 |

### Trazabilidad Verificada

- ✅ Todos los OBJ tienen al menos un RF asociado
- ✅ Todos los RF están asignados a un módulo
- ✅ Todos los RF tienen prioridad MoSCoW
- ✅ Cada RF está cubierto por al menos una HU
- ✅ Cada HU tiene criterios de aceptación en formato Given/When/Then
- ✅ Las restricciones del Discovery están preservadas
- ✅ Las dependencias críticas están documentadas
- ✅ Los riesgos tienen mitigación propuesta

---

> **Fecha de Completación:** 2026-01-30  
> **Generado por:** Ingeniero de Requerimientos (Fase 2 SRS-DD)

