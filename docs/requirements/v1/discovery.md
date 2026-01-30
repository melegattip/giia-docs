# Discovery: GIIA - Gestión de Inventario con Inteligencia Artificial

> **Versión:** 1.0  
> **Fecha:** 2026-01-29  
> **Estado:** Completado - Pendiente de validación

---

## Resumen Ejecutivo

GIIA es un sistema de gestión de inventario basado en la metodología **Demand Driven MRP (DDMRP)**, diseñado específicamente para el sector Retail y Distribución. El producto busca democratizar el acceso a DDMRP para comercios, distribuidores e importadores pymes, resolviendo el problema central de determinar qué, cuánto y cuándo comprar para mantener niveles óptimos de inventario. El MVP se enfocará en una posición estratégica única (el almacén o punto de venta), simplificando la implementación de DDMRP sin perder sus beneficios fundamentales. El sistema operará bajo un modelo SaaS Multi-tenant, proporcionando visualización intuitiva mediante un sistema de semáforo (Rojo/Amarillo/Verde) para facilitar la toma de decisiones de compra.

---

## 1. Contexto de Negocio

### 1.1 Situación Actual

Los comercios minoristas, distribuidores e importadores enfrentan desafíos significativos con los sistemas tradicionales de planificación de inventario (MRP/ERP):

| Desafío | Impacto en el Negocio |
|---------|----------------------|
| **Efecto Látigo (Bullwhip)** | Amplificación de variaciones de demanda desde el proveedor, generando distorsiones en toda la cadena |
| **Dependencia del Pronóstico** | Errores de predicción que generan excesos o faltantes de stock |
| **Lead Times Variables** | Proveedores con tiempos de entrega impredecibles que dificultan la planificación |
| **Inventario Desbalanceado** | Capital inmovilizado en productos de baja rotación y faltantes en los de alta |
| **Decisiones Reactivas** | Compras tardías (rotura de stock) o excesivas (sobrestock) |
| **Falta de Visibilidad** | Incertidumbre sobre qué comprar, cuánto y cuándo |

### 1.2 Motivación

La motivación para desarrollar GIIA surge de múltiples factores:

1. **Oportunidad de Mercado:** Existe una brecha significativa entre las soluciones empresariales complejas de DDMRP y las necesidades de las pymes del sector retail/distribución
2. **Democratización de DDMRP:** Las herramientas actuales de DDMRP están orientadas a grandes empresas manufactureras, dejando desatendido al sector retail/distribución
3. **Problema Recurrente:** Los comerciantes enfrentan diariamente la pregunta: *"¿Cuánto debo comprar hoy de cada producto para no quedarme sin stock, pero sin inmovilizar capital innecesariamente?"*
4. **Adaptación Metodológica:** La oportunidad de adaptar los 4 pilares operativos de DDMRP específicamente para retail, donde la posición estratégica ya está definida (almacén/punto de venta)

---

## 2. Problema de Negocio

### 2.1 Declaración del Problema

Los comercios minoristas, distribuidores e importadores carecen de herramientas accesibles y especializadas que les permitan:

1. **Visualizar proactivamente** el estado de su inventario frente a la demanda real
2. **Determinar automáticamente** qué productos necesitan reposición y en qué cantidad
3. **Anticiparse a roturas de stock** considerando lead times, pedidos en tránsito y demanda comprometida
4. **Optimizar el capital de trabajo** evitando tanto el sobrestock como la escasez

Actualmente, estas decisiones se toman de forma manual, reactiva y sin considerar la ecuación completa de flujo (inventario físico + en tránsito - demanda calificada).

### 2.2 Impacto del Problema

Si este problema no se resuelve:

- **Pérdida de ventas:** Roturas de stock frecuentes resultan en ventas perdidas y clientes insatisfechos
- **Capital inmovilizado:** Sobrestock de productos de baja rotación que no generan retorno
- **Costos ocultos:** Productos obsoletos, caducados o deteriorados por permanencia excesiva
- **Ineficiencia operativa:** Tiempo excesivo dedicado a decisiones manuales de reposición
- **Pérdida de competitividad:** Incapacidad de responder ágilmente a cambios en la demanda
- **Estrés operativo:** Incertidumbre constante sobre el estado real del inventario

---

## 3. Objetivos de Negocio

| ID | Objetivo | Métrica de Éxito | Valor Actual | Valor Objetivo |
|----|----------|------------------|--------------|----------------|
| OBJ-001 | Reducir roturas de stock | % de SKUs en zona roja crítica | N/A (sin medición actual) | < 5% del inventario activo |
| OBJ-002 | Optimizar capital de trabajo | Rotación de inventario mensual | Variable por cliente | Incremento del 20% vs. baseline |
| OBJ-003 | Reducir inventario inmovilizado | % de productos con más de X días en stock | Variable por cliente | Reducción del 30% vs. baseline |
| OBJ-004 | Acelerar toma de decisiones | Tiempo para generar orden de compra | Manual (horas) | < 5 minutos por proveedor |
| OBJ-005 | Mejorar visibilidad de inventario | Usuarios con dashboard actualizado diariamente | 0% | 100% de usuarios activos |
| OBJ-006 | Automatizar cálculos DDMRP | Productos con buffers calculados automáticamente | 0% | 100% del catálogo activo |

---

## 4. Alcance del MVP

### 4.1 Dentro del Alcance (In-Scope)

**Gestión de Datos Maestros:**
- [x] Gestión de Proveedores (Lead Time, Razón Social, Ubicación)
- [x] Gestión de Categorías (Nombre, Descripción)
- [x] Gestión de Productos (SKU, LT por proveedor, MOQ, Frecuencia de pedido, Costo unitario)

**Transacciones de Inventario:**
- [x] Registro de Órdenes de Compra (OC) vigentes y efectuadas
- [x] Registro de Órdenes de Venta en firme y efectuadas
- [x] Ingresos y Egresos extraordinarios (devoluciones, pérdidas, ajustes)
- [x] Seguimiento de estado de OC (vigente, efectuada)

**Motor DDMRP:**
- [x] Cálculo de CPD (Consumo Promedio Diario)
- [x] Perfiles de Buffer (configuración de zonas según LT, Variabilidad, MOQ, Frecuencia)
- [x] Cálculo de Zonas del Buffer (Rojo, Amarillo, Verde)
- [x] Ecuación de Flujo Neto (Inventario físico + En tránsito - Demanda calificada)
- [x] Sugerencias de Reposición (qué comprar, cuánto, cuándo)

**Ajustes Dinámicos (Should - MVP):**
- [x] Ajustes recalculados automáticos (CPD, LT, cambios de perfil)
- [x] Factores de Ajuste de Demanda (FAD)
- [x] Factores de Ajuste de Zona (FAZ)
- [x] Factores de Ajuste de Lead Time (FALTD)

**Visualización y Ejecución:**
- [x] Dashboard de estado de inventario con semáforo visual
- [x] Pantalla de planificación con priorización por urgencia
- [x] Alertas al usuario por desvíos en llegada de mercancía

**KPIs:**
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

**Arquitectura:**
- [x] Modelo SaaS Multi-tenant

### 4.2 Fuera del Alcance (Out-of-Scope)

| Elemento Excluido | Justificación |
|-------------------|---------------|
| **Integración API Manager ERP** | Requiere discovery adicional con proveedores de ERP específicos. Se evaluará post-validación del modelo de datos |
| **Integración Odoo** | Complejidad de integración justifica desarrollo post-MVP, una vez validado el producto core |
| **Sistema de Suscripciones y Pagos** | El MVP se enfocará en validar el valor del producto. Monetización se implementará post-MVP |
| **Gamificación** | Feature de engagement que se implementará una vez validada la retención de usuarios |
| **Autenticación 2FA** | Feature de seguridad avanzada. Prioridad "Should" - se evaluará según recursos disponibles |
| **Forecasting Personalizado** | Marcado como TBD en requisitos. Requiere definición más detallada |
| **Múltiples posiciones estratégicas** | El MVP se enfoca en una posición única (almacén/punto de venta) |

---

## 5. Suposiciones

| ID | Suposición | Estado |
|----|------------|--------|
| SUP-001 | Los usuarios tienen conocimiento básico de gestión de inventario | 🟢 Confirmada |
| SUP-002 | Los usuarios podrán proporcionar datos históricos de ventas/compras para cálculo inicial de CPD | 🟢 Confirmada |
| SUP-003 | El formato CSV será suficiente para la carga masiva inicial de datos | 🟢 Confirmada |
| SUP-004 | Los usuarios están dispuestos a invertir tiempo en configurar los perfiles de buffer iniciales | 🟢 Confirmada |
| SUP-005 | Una posición estratégica única (almacén/punto de venta) es suficiente para el MVP | 🟢 Confirmada |
| SUP-006 | Los proveedores tienen Lead Times variables: algunos estables/predecibles, otros no. El sistema debe manejar ambos escenarios mediante el factor de variabilidad. | 🟢 Confirmada |
| SUP-007 | Los usuarios actualizarán regularmente el sistema con transacciones (compras/ventas) | 🟢 Confirmada |
| SUP-008 | El modelo SaaS Multi-tenant es aceptable para el mercado objetivo | 🟢 Confirmada |

**Estados posibles:**
- 🟡 Pendiente de validar
- 🟢 Confirmada
- 🔴 Invalidada

---

## 6. Restricciones Conocidas

| ID | Restricción | Tipo | Impacto |
|----|-------------|------|---------|
| RES-001 | El MVP debe operar bajo modelo SaaS Multi-tenant | Arquitectura | Requiere diseño de aislamiento de datos por tenant desde el inicio |
| RES-002 | La interfaz debe ser visual e intuitiva, accesible para usuarios no técnicos | UX/UI | Limita complejidad de configuraciones avanzadas en pantallas principales |
| RES-003 | El sistema debe soportar retroalimentación en tiempo real | Técnica | Requiere arquitectura que soporte actualizaciones frecuentes de datos |
| RES-004 | Los cálculos DDMRP deben seguir la metodología estándar | Negocio | Las fórmulas de zonas y flujo neto están definidas y no son negociables |
| RES-005 | El sistema debe calcular fechas de llegada estimadas basándose en Lead Times | Técnica | Requiere precisión en datos de proveedor y fecha de OC |
| RES-006 | Recursos de desarrollo limitados para el MVP | Recursos | Priorización estricta de features "Must" sobre "Should" |

---

## 7. Preguntas Abiertas

| ID | Pregunta | Responsable | Estado | Respuesta |
|----|----------|-------------|--------|-----------|
| PA-001 | ¿Cuál es la ventana de tiempo por defecto para el cálculo del CPD? | Product Owner | 🟢 Resuelta | 7 días por defecto. Debe ser editable por el usuario. |
| PA-002 | ¿Cómo se define la "Demanda Calificada"? ¿Incluye solo pedidos en firme o también pronósticos? | Product Owner | 🟢 Resuelta | Órdenes de venta en firme + picos calificados (pedidos que superan el umbral de pico). El umbral es por defecto el 50% de la zona roja y debe ser editable. |
| PA-003 | ¿Cuáles son los umbrales específicos para las categorías de Lead Time (Corto/Medio/Largo)? | Product Owner | 🟢 Resuelta | Seteable por el usuario en la etapa de setup. En futuro, el sistema sugerirá valores recomendados basados en datos empíricos. |
| PA-004 | ¿Cómo se calculará la variabilidad de proveedor automáticamente? | Product Owner | 🟢 Resuelta | Desviación estándar por producto relativo al CPD. |
| PA-005 | ¿El sistema debe soportar múltiples monedas o solo moneda local? | Product Owner | 🟢 Resuelta | Sí, al menos 2 monedas: USD y Peso local. |
| PA-006 | ¿Cuál es el umbral de tiempo para considerar un producto como "inmovilizado"? | Product Owner | 🟢 Resuelta | Número editable. Luego debe ser calculado por el sistema. Son aquellos productos que ingresaron a stock antes de las últimas 2 órdenes de compra. |
| PA-007 | ¿Se requiere soporte para múltiples proveedores por producto desde el MVP? | Product Owner | 🟢 Resuelta | Sí. |
| PA-008 | ¿Cuál es el comportamiento esperado para productos nuevos sin histórico de ventas? | Product Owner | 🟢 Resuelta | Comportamiento definido inicialmente de forma estable (lineal) hasta que haya información suficiente. |
| PA-009 | ¿El "Forecasting custom" marcado como TBD se incluirá en el MVP? | Product Owner | 🟢 Resuelta | No, excluido del MVP. |
| PA-010 | ¿Cuáles son los requisitos específicos de la alerta por desvío en llegada de mercancía? | Product Owner | 🟢 Resuelta | El usuario define el % o cantidad de días previos a la llegada para comenzar a avisar. El día de llegada debe alertar, y si el pedido no entra, la alerta debe permanecer con prioridad. |

**Estados posibles:**
- 🟡 Abierta
- 🟢 Resuelta
- 🔴 Bloqueante

---

## 8. Referencias

| Tipo | Descripción | Ubicación/Enlace |
|------|-------------|------------------|
| Documento | Análisis del Product Owner - GIIA | `docs/requirements/product_owner_request.md` |
| Metodología | DDMRP - Demand Driven MRP | Referencia externa: Demand Driven Institute |
| Template | Template de Discovery | `docs/requirements/templates/discovery-template.md` |

---

## Checklist de Validación

*Antes de declarar el Discovery como completado, verifica que se cumplan todos los criterios:*

### Completitud
- [x] El resumen ejecutivo captura la esencia del discovery
- [x] El contexto de negocio explica claramente el "por qué ahora"
- [x] El problema de negocio está claramente articulado
- [x] Todos los objetivos tienen métricas de éxito definidas

### Claridad
- [x] El alcance está claramente delimitado (in-scope y out-of-scope)
- [x] Las justificaciones de exclusión son claras y razonables
- [x] No hay ambigüedades en las descripciones

### Trazabilidad
- [x] Las suposiciones están identificadas y pendientes de validación
- [x] Las restricciones conocidas están documentadas
- [x] Las preguntas abiertas tienen responsable asignado
- [x] Las referencias relevantes están listadas

### Listo para Fase 2
- [x] No hay preguntas bloqueantes sin resolver
- [x] El documento ha sido revisado por pares (Peer Review)
- [x] El documento ha sido revisado por agente (Agent Review)

---

## Señal de Completación

**DISCOVERY COMPLETADO Y APROBADO POR EL ORQUESTADOR**

---

> **Validación del Orquestador (2026-01-30):** Todas las preguntas abiertas han sido resueltas por el Product Owner. El Discovery cumple con todos los criterios de calidad y está aprobado para avanzar a Fase 2 (SRS).

