# Prompt: Generar Discovery Document

## Tu Rol
Eres un **Analista de Negocio** experto trabajando en el proyecto GIIA (Gestión de Inventario con Inteligencia Artificial). Tu objetivo es generar un documento de Discovery completo.

## Contexto del Proyecto
- **Proyecto:** GIIA - Sistema de gestión de inventario basado en metodología DDMRP
- **Tipo:** MVP (Minimum Viable Product) - Versión 1
- **Arquitectura:** SaaS Multi-tenant
- **Metodología:** SRS-DD (SRS-Driven Development)

## Visión del Producto (del Product Owner)

GIIA es un software innovador enfocado en la gestión de inventario basado en la metodología **Demand Driven Material Requirements Planning (DDMRP)**, adaptada específicamente para el sector **Retail y Distribución**.

### Idea Central
Desarrollar una herramienta que permita a usuarios visualizar y gestionar su inventario de forma proactiva, aplicando los 4 pilares operativos de DDMRP para Retail:
1. **Perfiles de Buffer** - Configurar zonas según Lead Time, Variabilidad, MOQ, Frecuencia
2. **Ajustes Dinámicos** - Adaptar buffers según estacionalidad, promociones, tendencias
3. **Planificación por Demanda** - Ecuación de Flujo Neto (Stock Físico + En Tránsito - Demanda Calificada)
4. **Ejecución Visual** - Sistema de semáforo (Rojo/Amarillo/Verde) para decisiones de compra

### Público Objetivo
| Segmento | Características |
|----------|----------------|
| **Comercios Minoristas** | Tiendas con inventarios de múltiples SKUs que compran a distribuidores |
| **Distribuidores** | Empresas que compran a fabricantes y venden a comercios |
| **Importadores** | Negocios con lead times largos y alta variabilidad |

### Premisas del Diseño
- Diseño Visual e Intuitivo
- De Fácil Comprensión
- Soporte a la Toma de Decisiones (qué y cuánto comprar)
- Retroalimentación en Tiempo Real
- Modelo SaaS Multi-tenant

### Inputs Principales
**A. Transacciones de Ingreso:**
- Órdenes de Compra (OC) vigentes (proveedor, SKUs, cantidades, precios, LT, ETA, estado)
- Compras efectuadas
- Ingresos extraordinarios (devoluciones, compensaciones)

**B. Transacciones de Egreso:**
- Órdenes de venta en firme
- Ventas efectuadas
- Egresos extraordinarios (caducidad, pérdida, robo)

**C. Información de Proveedor/Categoría/Producto:**
- Proveedor: Lead Time, Razón Social, Ubicación
- Categoría: Nombre, Descripción
- Producto: SKU, LT por proveedor, MOQ, Frecuencia de pedido, Costo unitario

**D. Variables de Setup:**
- Ventana de tiempo para CPD
- FO deseado, Categoría LT, %LT, Variabilidad, %Variabilidad
- Umbral de pico, Horizonte de pico

### Outputs Principales
- **Niveles de Buffer DDMRP** (Zonas Rojo/Amarillo/Verde)
- **Ecuación de Flujo Neto** (EFP = Inventario físico + En tránsito - Demanda calificada)
- **Sugerencias de Reposición** (qué comprar, cuánto, cuándo)
- **KPIs**: Costo Total Inventario, Días en Inventario Valorizado, Inventario Inmovilizado, Rotación

### Decisiones de Alcance MVP (confirmadas por PO)

| Feature | Prioridad MVP |
|---------|---------------|
| Input Manual e Interfaz | Must |
| Importación CSV | Must |
| API Manager ERP | Should (pendiente discovery con proveedor) |
| Todos los KPIs | Must |
| Ajustes Dinámicos (FAD, FAZ, FALTD) | Should |
| Roles (Admin, Usuario, Solo Lectura) | Must |
| Auth Email/Password | Must |
| 2FA | Should |
| Suscripciones/Pagos | Post-MVP |
| Gamificación | Post-MVP |
| Integración Odoo | Post-MVP |

## Tu Tarea
Genera el documento `discovery.md` siguiendo el template en `docs/requirements/templates/discovery-template.md`, que incluya:

1. **Resumen Ejecutivo** - Síntesis del problema y solución propuesta
2. **Contexto de Negocio** - Situación actual y motivación
3. **Problema de Negocio** - Declaración clara e impacto
4. **Objetivos de Negocio** - Objetivos medibles (usar prefijo OBJ-XXX)
5. **Alcance del MVP** - In-scope y out-of-scope claramente definidos
6. **Suposiciones** - Supuestos de trabajo pendientes de validar
7. **Restricciones Conocidas** - Limitaciones (usar prefijo RES-XXX)
8. **Preguntas Abiertas** - Dudas que deben resolverse

## Instrucciones Operativas
- Haz preguntas clarificadoras si detectas ambigüedad
- NO escribas requerimientos detallados (RF-XXX, RNF-XXX)
- NO escribas historias de usuario
- Guarda el documento en `docs/requirements/v1/discovery.md`
- Usa español para todo el documento

## Señal de Completación
Al finalizar, indica: **"DISCOVERY COMPLETADO - Listo para validación del Orquestador"**
