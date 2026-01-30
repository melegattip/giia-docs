# Prompt: Generar SRS Document

## Tu Rol
Eres un **Ingeniero de Requerimientos** experto trabajando en el proyecto GIIA. Tu objetivo es transformar el Discovery aprobado en un SRS (Software Requirements Specification) completo.

## Contexto del Proyecto
- **Proyecto:** GIIA - Sistema de gestión de inventario basado en metodología DDMRP
- **Tipo:** MVP (Minimum Viable Product) - Versión 1
- **Arquitectura:** SaaS Multi-tenant
- **Metodología:** SRS-DD (SRS-Driven Development)

## Discovery Aprobado

El Discovery completo se encuentra en: `docs/requirements/v1/discovery.md`

### Resumen del Discovery

GIIA es un sistema de gestión de inventario basado en DDMRP para retail/distribución. El MVP incluye:

**Objetivos de Negocio:**
| ID | Objetivo |
|----|----------|
| OBJ-001 | Reducir roturas de stock (< 5% SKUs en zona roja crítica) |
| OBJ-002 | Optimizar capital de trabajo (incremento 20% rotación) |
| OBJ-003 | Reducir inventario inmovilizado (reducción 30%) |
| OBJ-004 | Acelerar toma de decisiones (< 5 min por orden) |
| OBJ-005 | Mejorar visibilidad de inventario (100% usuarios con dashboard) |
| OBJ-006 | Automatizar cálculos DDMRP (100% catálogo con buffers calculados) |

**Decisiones de Alcance (actualizado con respuestas del PO):**

| Feature | Prioridad |
|---------|-----------|
| Input Manual e Interfaz | Must |
| Importación CSV | Must |
| API Manager ERP | Should |
| KPIs (4 indicadores) | Must |
| Ajustes Dinámicos (FAD, FAZ, FALTD) | Should |
| Roles (Admin, Usuario, Solo Lectura) | Must |
| Auth Email/Password | Must |
| 2FA | Should |
| Soporte múltiples monedas (USD + Peso local) | Must |
| Múltiples proveedores por producto | Must |

**Respuestas Clave del PO (para especificar requerimientos):**

1. **CPD:** Ventana de 7 días por defecto, editable por usuario
2. **Demanda Calificada:** Órdenes de venta en firme + picos calificados (pedidos > umbral de pico). Umbral = 50% zona roja por defecto, editable
3. **Categorías Lead Time:** Seteable por usuario en setup. Futuro: sugerencias basadas en datos empíricos
4. **Variabilidad:** Desviación estándar por producto relativo al CPD
5. **Monedas:** USD y Peso local
6. **Inventario Inmovilizado:** Número editable, calculado por sistema. Productos que ingresaron antes de las últimas 2 OC
7. **Múltiples proveedores por producto:** Sí
8. **Productos nuevos sin histórico:** Comportamiento estable (lineal) hasta tener información suficiente
9. **Forecasting custom:** Excluido del MVP
10. **Alertas de desvío de llegada:** Usuario define % o días previos para avisar. Día de llegada: alertar. Si no entra: alerta con prioridad

**Restricciones (RES-001 a RES-006):**
- SaaS Multi-tenant con aislamiento de datos
- Interfaz visual e intuitiva para usuarios no técnicos
- Retroalimentación en tiempo real
- Cálculos DDMRP según metodología estándar
- Cálculo de ETA basado en Lead Times
- Recursos de desarrollo limitados

## Tu Tarea

Genera el documento `srs.md` siguiendo el template en `docs/requirements/templates/srs-template.md`, que incluya:

1. **Resumen Ejecutivo** - Síntesis de qué se construirá
2. **Introducción** - Propósito y alcance
3. **Objetivos de Negocio** - Copiar del Discovery
4. **Requerimientos Funcionales** (RF-XXX) organizados por módulos:
   - Gestión de Datos Maestros (Proveedores, Categorías, Productos)
   - Transacciones de Inventario (OC, OV, Ingresos/Egresos)
   - Motor DDMRP (CPD, Perfiles Buffer, Zonas, Flujo Neto)
   - Ajustes Dinámicos (Should)
   - Visualización y Ejecución (Dashboard, Planificación, Alertas)
   - KPIs
   - Entrada de Datos (Manual, CSV)
   - Autenticación y Autorización
   - Configuración Multi-moneda
   
5. **Historias de Usuario** (HU-XXX) - Resumen con referencia a archivos individuales
6. **Requerimientos No Funcionales** (RNF-XXX):
   - Rendimiento
   - Seguridad
   - Usabilidad
   - Disponibilidad
   - Escalabilidad
   - Compatibilidad
   
7. **Restricciones** - Copiar del Discovery
8. **Dependencias** - Entre requerimientos
9. **Riesgos** (RIE-XXX) - Con mitigación propuesta
10. **Glosario** - Términos DDMRP

## Convenciones de Documentación

- **Numeración:** OBJ-XXX, RF-XXX, RNF-XXX, HU-XXX, RES-XXX, RIE-XXX, DEP-XXX
- **Priorización MoSCoW:** Must, Should, Could, Won't
- **Criterios de Aceptación:** Formato Given/When/Then
- **Trazabilidad:** Cada RF debe vincular a OBJ-XXX

## Instrucciones para Historias de Usuario

Genera archivos individuales en `docs/requirements/user-stories/HU-XXX.md` siguiendo el template `docs/requirements/templates/user-story-template.md`.

Cada historia debe incluir:
- Prioridad MoSCoW
- RF relacionados
- Formato "Como [rol], Quiero [acción], Para [beneficio]"
- Criterios de aceptación en formato Given/When/Then

## Instrucciones Operativas

- NO modifiques el Discovery aprobado
- Asegura trazabilidad completa: OBJ → RF → HU
- Prioriza TODOS los elementos con MoSCoW
- NO incluyas detalles de implementación técnica (arquitectura, código, frameworks)
- Usa español para todo el documento
- Guarda el SRS en `docs/requirements/v1/srs.md`
- Guarda las historias de usuario en `docs/requirements/user-stories/`

## Señal de Completación

Al finalizar, indica: **"SRS COMPLETADO - Listo para validación del Orquestador"**
