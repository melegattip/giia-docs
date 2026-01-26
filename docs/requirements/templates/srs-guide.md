# GIIA SRS Guide
## Guía de Desarrollo Dirigido por Especificación de Requerimientos

---

| Atributo | Valor |
|----------|-------|
| **Versión** | 1.0.0 |
| **Proyecto** | GIIA - Gestión de Inventario con Inteligencia Artificial |
| **Fecha de Creación** | 2026-01-26 |
| **Última Actualización** | 2026-01-26 |
| **Estado** | Activo |
| **Idioma** | Español |

---

## 1. Propósito

Esta guía establece la metodología **SRS-DD (SRS-Driven Development)** para el proyecto GIIA. Sirve como referencia principal para agentes de IA y miembros del equipo en la generación consistente y de alta calidad de documentos de requerimientos de software.

### Objetivos de esta Guía

- Definir un proceso estructurado de 2 fases para captura de requerimientos
- Establecer roles claros para agentes de IA participantes
- Proveer convenciones de documentación estandarizadas
- Asegurar calidad mediante checkpoints de validación
- Mantener un proceso ágil adaptado a equipos MVP

---

## 2. Metodología SRS-DD (SRS-Driven Development)

### 2.1 Visión General

SRS-DD es una metodología de desarrollo dirigido por especificación que garantiza que todo desarrollo esté respaldado por requerimientos formalmente documentados y validados.

```
┌─────────────────────────────────────────────────────────────────┐
│  ORQUESTADOR                                                    │
│  Rol: Coordinador y Validador                                   │
│  Responsabilidad: Generar prompts, validar outputs, aprobar     │
│                   avance entre fases                            │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  FASE 1: DISCOVERY                                              │
│  Rol del Agente: Analista de Negocio                            │
│  Output: discovery.md                                           │
│  Define: Análisis de necesidad, contexto del problema,          │
│          objetivos de negocio, stakeholders, alcance inicial    │
│  → Checkpoint: Discovery validado ✓                             │
├─────────────────────────────────────────────────────────────────┤
│  FASE 2: ESPECIFICACIÓN                                         │
│  Rol del Agente: Ingeniero de Requerimientos                    │
│  Output: srs.md                                                 │
│  Define: Requerimientos funcionales y no funcionales,           │
│          historias de usuario, criterios de aceptación,         │
│          restricciones, riesgos                                 │
│  → Checkpoint: SRS validado ✓                                   │
└─────────────────────────────────────────────────────────────────┘
```

### 2.2 Flujo del Proceso

1. **Inicio:** El Product Owner identifica la visión del producto/MVP
2. **Fase 1:** El Analista de Negocio genera `discovery.md` del producto completo
3. **Validación 1:** El Orquestador valida el Discovery
4. **Fase 2:** El Ingeniero de Requerimientos genera `srs.md` del producto completo
5. **Validación 2:** El Orquestador valida el SRS
6. **Completación:** Documentos listos para desarrollo

---

## 3. Roles de Agentes

### 3.1 Orquestador

**Función Principal:** Coordinar el proceso SRS-DD y asegurar calidad.

#### Responsabilidades
- Generar prompts específicos para cada fase (NO ejecutar las fases)
- Validar outputs contra checkpoints de calidad definidos
- Aprobar avance de fase tras validación exitosa
- Responder preguntas de clarificación de otros agentes
- Tomar decisiones sobre ambigüedades detectadas
- Mantener consistencia entre documentos

#### Anti-patrones (NO HACER)
- ❌ Generar documentos directamente (discovery.md o srs.md)
- ❌ Saltar validaciones o checkpoints
- ❌ Modificar outputs de otros agentes sin documentar
- ❌ Aprobar documentos incompletos

---

### 3.2 Analista de Negocio (Fase 1)

**Función Principal:** Generar el documento de Discovery.

#### Responsabilidades
- Analizar la necesidad o problema de negocio presentado
- Identificar stakeholders y documentar sus necesidades específicas
- Definir objetivos de negocio medibles (SMART cuando sea posible)
- Establecer alcance inicial (in-scope / out-of-scope)
- Documentar restricciones conocidas (técnicas, de tiempo, presupuesto)
- Hacer preguntas clarificadoras cuando detecte ambigüedad

#### Anti-patrones (NO HACER)
- ❌ Escribir requerimientos detallados (RF-XXX, RNF-XXX)
- ❌ Escribir historias de usuario con criterios de aceptación
- ❌ Avanzar a Fase 2 sin validación del Orquestador
- ❌ Incluir detalles de implementación técnica

---

### 3.3 Ingeniero de Requerimientos (Fase 2)

**Función Principal:** Generar el documento SRS detallado.

#### Responsabilidades
- Transformar el Discovery aprobado en requerimientos formales
- Escribir historias de usuario con criterios de aceptación (Given/When/Then)
- Clasificar requerimientos como funcionales (RF) y no funcionales (RNF)
- Priorizar todos los requerimientos usando MoSCoW
- Documentar dependencias entre requerimientos
- Identificar y documentar riesgos técnicos y de negocio
- Asegurar trazabilidad con objetivos del Discovery

#### Anti-patrones (NO HACER)
- ❌ Modificar el Discovery aprobado
- ❌ Incluir detalles de implementación técnica (arquitectura, código)
- ❌ Omitir criterios de aceptación en historias de usuario
- ❌ Crear requerimientos sin trazabilidad a objetivos

---

## 4. Convenciones de Documentación

### 4.1 Numeración de Elementos

| Tipo | Prefijo | Formato | Ejemplo |
|------|---------|---------|---------|
| Objetivo de Negocio | `OBJ-` | OBJ-XXX | OBJ-001, OBJ-002 |
| Requerimiento Funcional | `RF-` | RF-XXX | RF-001, RF-002 |
| Requerimiento No Funcional | `RNF-` | RNF-XXX | RNF-001, RNF-002 |
| Historia de Usuario | `HU-` | HU-XXX | HU-001, HU-002 |
| Restricción | `RES-` | RES-XXX | RES-001, RES-002 |
| Riesgo | `RIE-` | RIE-XXX | RIE-001, RIE-002 |

### 4.2 Priorización MoSCoW

| Nivel | Significado | Criterio de Aplicación |
|-------|-------------|------------------------|
| **Must** | Debe tener | Crítico para el MVP. Sin esto, el producto no es viable. |
| **Should** | Debería tener | Importante pero no crítico. El producto funciona sin esto. |
| **Could** | Podría tener | Deseable si hay tiempo y recursos disponibles. |
| **Won't** | No tendrá | Explícitamente fuera de alcance para esta versión. |

### 4.3 Formato de Historia de Usuario

```markdown
### HU-XXX: [Título descriptivo y conciso]

**Prioridad:** Must/Should/Could/Won't  
**Relacionado con:** RF-XXX, OBJ-XXX

**Como** [tipo de usuario],  
**Quiero** [acción o funcionalidad específica],  
**Para** [beneficio o valor que obtengo].

#### Criterios de Aceptación

- [ ] **Dado** [contexto inicial], **Cuando** [acción del usuario], **Entonces** [resultado esperado].
- [ ] **Dado** [contexto inicial], **Cuando** [acción del usuario], **Entonces** [resultado esperado].

#### Notas
[Aclaraciones adicionales, edge cases, o consideraciones especiales]
```

### 4.4 Formato de Requerimiento Funcional

```markdown
### RF-XXX: [Título del requerimiento]

**Prioridad:** Must/Should/Could/Won't  
**Trazabilidad:** OBJ-XXX  
**Historias de Usuario:** HU-XXX, HU-XXX

**Descripción:**  
[Descripción clara y no ambigua del requerimiento]

**Precondiciones:**  
- [Condición que debe cumplirse antes]

**Postcondiciones:**  
- [Estado del sistema después de cumplir el requerimiento]
```

---

## 5. Checkpoints de Calidad

### 5.1 Validación de Fase 1 (Discovery)

Antes de aprobar avance a Fase 2, verificar:

- [ ] Problema de negocio claramente articulado
- [ ] Stakeholders identificados con sus necesidades documentadas
- [ ] Objetivos de negocio definidos y medibles
- [ ] Alcance delimitado (in-scope / out-of-scope explícito)
- [ ] Restricciones conocidas documentadas
- [ ] Sin ambigüedades pendientes de resolver
- [ ] Documento revisado por peer review

### 5.2 Validación de Fase 2 (SRS)

Antes de dar por completado el SRS, verificar:

- [ ] Todos los objetivos del Discovery tienen requerimientos asociados
- [ ] Cada RF tiene al menos una historia de usuario
- [ ] Historias de usuario tienen criterios de aceptación Given/When/Then
- [ ] Priorización MoSCoW aplicada a todos los requerimientos
- [ ] RNF documentados (rendimiento, seguridad, usabilidad)
- [ ] Riesgos identificados y documentados
- [ ] Dependencias entre requerimientos mapeadas
- [ ] Peer review completado
- [ ] Agent review completado

### 5.3 Criterios de Calidad para Requerimientos

Cada requerimiento debe cumplir:

- [ ] **Completo:** Contiene toda la información necesaria
- [ ] **Consistente:** No contradice otros requerimientos
- [ ] **No ambiguo:** Una sola interpretación posible
- [ ] **Verificable:** Se puede probar/validar objetivamente
- [ ] **Trazable:** Vinculado a un objetivo de negocio
- [ ] **Priorizado:** Tiene asignación MoSCoW
- [ ] **Atómico:** Representa una sola necesidad
- [ ] **Factible:** Realizable dentro de las restricciones conocidas

---

## 6. Workflow del Orquestador

### Paso 1: Recibir Solicitud
```
Entrada: Descripción de necesidad/problema del Product Owner
Acción: Analizar solicitud y preparar contexto para Fase 1
Salida: Prompt para Analista de Negocio
```

### Paso 2: Ejecutar Fase 1
```
Entrada: Prompt de Fase 1
Acción: Enviar al Analista de Negocio
Salida: discovery.md generado
```

### Paso 3: Validar Discovery
```
Entrada: discovery.md
Acción: Aplicar checklist de validación Fase 1
Salida: Aprobación o solicitud de correcciones
```

### Paso 4: Ejecutar Fase 2
```
Entrada: discovery.md aprobado + Prompt de Fase 2
Acción: Enviar al Ingeniero de Requerimientos
Salida: srs.md generado
```

### Paso 5: Validar SRS
```
Entrada: srs.md
Acción: Aplicar checklist de validación Fase 2
Salida: SRS aprobado o solicitud de correcciones
```

### Paso 6: Completar Proceso
```
Entrada: Documentos aprobados
Acción: Confirmar completación, archivar documentos
Salida: Documentos listos para desarrollo
```

---

## 7. Prompt Templates

### 7.1 Prompt para Fase 1: Analista de Negocio

```markdown
# Prompt: Generar Discovery Document

## Tu Rol
Eres un **Analista de Negocio** experto trabajando en el proyecto GIIA (Gestión de Inventario con Inteligencia Artificial). Tu objetivo es generar un documento de Discovery completo.

## Contexto del Proyecto
- **Proyecto:** GIIA - Sistema de gestión de inventario potenciado por IA
- **Tipo:** MVP (Minimum Viable Product)
- **Equipo:** 2 Ingenieros de Software + 1 Product Owner
- **Metodología:** SRS-DD (SRS-Driven Development)

## Visión del Producto/MVP a Analizar
[INSERTAR DESCRIPCIÓN COMPLETA DE LA VISIÓN DEL PRODUCTO O MVP]

## Tu Tarea
Genera el documento `discovery.md` que incluya:

1. **Resumen Ejecutivo** - Síntesis del problema y solución propuesta
2. **Análisis del Problema** - Descripción detallada del problema de negocio
3. **Stakeholders** - Identificación y necesidades de cada stakeholder
4. **Objetivos de Negocio** - Objetivos medibles (usar prefijo OBJ-XXX)
5. **Alcance** - Claramente definir in-scope y out-of-scope
6. **Restricciones** - Limitaciones conocidas (usar prefijo RES-XXX)
7. **Supuestos** - Supuestos de trabajo

## Instrucciones Operativas
- Haz preguntas clarificadoras si detectas ambigüedad
- NO escribas requerimientos detallados (RF-XXX, RNF-XXX)
- NO escribas historias de usuario
- Mantén el documento conciso pero completo
- Usa español para todo el documento

## Señal de Completación
Al finalizar, indica: "**DISCOVERY COMPLETADO - Listo para validación del Orquestador**"
```

### 7.2 Prompt para Fase 2: Ingeniero de Requerimientos

```markdown
# Prompt: Generar SRS Document

## Tu Rol
Eres un **Ingeniero de Requerimientos** experto trabajando en el proyecto GIIA. Tu objetivo es transformar el Discovery aprobado en un SRS (Software Requirements Specification) completo.

## Contexto del Proyecto
- **Proyecto:** GIIA - Sistema de gestión de inventario potenciado por IA
- **Tipo:** MVP (Minimum Viable Product)
- **Equipo:** 2 Ingenieros de Software + 1 Product Owner
- **Metodología:** SRS-DD (SRS-Driven Development)

## Discovery Aprobado
[INSERTAR CONTENIDO DEL DISCOVERY.MD APROBADO]

## Tu Tarea
Genera el documento `srs.md` que incluya:

1. **Introducción** - Propósito, alcance, referencias
2. **Descripción General** - Perspectiva del producto, funciones principales
3. **Requerimientos Funcionales** - Usar prefijo RF-XXX
4. **Requerimientos No Funcionales** - Usar prefijo RNF-XXX
5. **Historias de Usuario** - Usar prefijo HU-XXX con formato Given/When/Then
6. **Matriz de Trazabilidad** - Vincular RF → OBJ → HU
7. **Riesgos** - Usar prefijo RIE-XXX
8. **Glosario** - Términos específicos del dominio

## Convenciones de Documentación
- Numerar: OBJ-XXX, RF-XXX, RNF-XXX, HU-XXX, RES-XXX, RIE-XXX
- Priorizar con MoSCoW: Must, Should, Could, Won't
- Criterios de aceptación en formato Given/When/Then

## Instrucciones Operativas
- NO modifiques el Discovery aprobado
- Asegura trazabilidad completa OBJ → RF → HU
- Prioriza todos los elementos con MoSCoW
- NO incluyas detalles de implementación técnica
- Usa español para todo el documento

## Señal de Completación
Al finalizar, indica: "**SRS COMPLETADO - Listo para validación del Orquestador**"
```

---

## 8. Estructura de Carpetas

```
docs/requirements/
├── templates/                    # Templates y guías
│   ├── srs-guide.md              # Esta guía (documento actual)
│   ├── discovery-template.md     # Template para Discovery
│   ├── srs-template.md           # Template para SRS
│   └── user-story-template.md    # Template para Historias de Usuario
│
├── user-stories/                 # Historias de usuario individuales
│   ├── HU-001.md
│   ├── HU-002.md
│   └── ...
│
└── v1/                           # Versión del producto (MVP v1)
    ├── discovery.md              # Output Fase 1
    └── srs.md                    # Output Fase 2

Ejemplos de versionado:
docs/requirements/
├── templates/
├── user-stories/
├── v1/                           # MVP inicial
│   ├── discovery.md
│   └── srs.md
└── v2/                           # Segunda versión mayor
    ├── discovery.md
    └── srs.md
```

---

## 9. Best Practices para Equipos MVP

### 9.1 Principios Guía

1. **Documentar lo esencial, no lo exhaustivo**
   - Prioriza claridad sobre completitud perfecta
   - Un documento conciso y claro es mejor que uno extenso y confuso

2. **Iterar rápido**
   - Los documentos pueden evolucionar
   - Version control permite tracking de cambios

3. **Mantener trazabilidad**
   - Todo requerimiento debe conectar con un objetivo de negocio
   - Facilita priorización y toma de decisiones

4. **Usar el Orquestador consistentemente**
   - Evita inconsistencias entre documentos
   - Asegura cumplimiento de estándares

### 9.2 Recomendaciones Prácticas

| Práctica | Beneficio |
|----------|-----------|
| Organizar RF por módulo/componente dentro del SRS | Facilita navegación y asignación |
| Priorizar Must Have primero | Asegura entrega de valor mínimo |
| Peer review en cada fase | Detecta problemas temprano |
| Usar checklists de validación | Estandariza calidad |
| Documentar decisiones importantes | Preserva contexto |
| Un Discovery + SRS por versión mayor | Mantiene trazabilidad clara |

### 9.3 Señales de Alerta

- ⚠️ Más del 50% de requerimientos son "Must Have" → Revisar priorización
- ⚠️ Historias de usuario sin criterios de aceptación → Incompletas
- ⚠️ Requerimientos sin trazabilidad a objetivos → Posible scope creep
- ⚠️ Múltiples interpretaciones posibles → Ambigüedad a resolver

---

## 10. Referencias

### Documentos Relacionados
- `discovery-template.md` - Template para documento Discovery
- `srs-template.md` - Template para documento SRS
- `user-story-template.md` - Template para Historias de Usuario

### Recursos Externos
- IEEE 830 - Práctica recomendada para SRS
- INVEST criteria para historias de usuario
- Metodología MoSCoW para priorización

---

## Historial de Cambios

| Versión | Fecha | Autor | Descripción |
|---------|-------|-------|-------------|
| 1.0.0 | 2026-01-26 | GIIA Team | Versión inicial de la guía |

---

*Este documento es parte del proyecto GIIA y está optimizado para ser procesado por agentes de IA.*
