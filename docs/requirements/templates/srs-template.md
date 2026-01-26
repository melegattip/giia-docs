# SRS: [Nombre de la Feature/Módulo]

> **Instrucción para el Agente:** Completa todas las secciones de este template basándote en el Discovery aprobado. Reemplaza los textos entre corchetes `[...]` con la información correspondiente. Elimina las instrucciones en cursiva una vez completado.

---

## Resumen Ejecutivo

*Proporciona un resumen de 3-5 oraciones que describa qué se va a construir, para quién, y el valor que aporta. Debe ser comprensible sin leer el documento completo.*

[Escribe aquí el resumen ejecutivo]

---

## 1. Introducción

### 1.1 Propósito

*Describe brevemente el propósito de este documento SRS.*

Este documento especifica los requerimientos funcionales y no funcionales para [nombre de la feature/módulo] del sistema GIIA.

### 1.2 Alcance

*Copia el alcance del Discovery, ajustando si hubo cambios durante la elaboración del SRS.*

#### Dentro del Alcance (In-Scope)

- [ ] [Elemento dentro del alcance 1]
- [ ] [Elemento dentro del alcance 2]
- [ ] [Elemento dentro del alcance 3]

#### Fuera del Alcance (Out-of-Scope)

| Elemento Excluido | Justificación |
|-------------------|---------------|
| [Elemento 1] | [Por qué no se incluye] |
| [Elemento 2] | [Por qué no se incluye] |

---

## 2. Objetivos de Negocio

*Copia los objetivos del Discovery. Estos servirán como referencia para la trazabilidad.*

| ID | Objetivo | Métrica de Éxito |
|----|----------|------------------|
| OBJ-001 | [Descripción del objetivo] | [Cómo se medirá] |
| OBJ-002 | [Descripción del objetivo] | [Cómo se medirá] |

---

## 3. Requerimientos Funcionales

*Agrupa los requerimientos por módulo/componente. Cada RF debe tener su prioridad MoSCoW.*

### 3.1 [Nombre del Módulo/Componente 1]

*Breve descripción del módulo y su relación con los objetivos de negocio.*

**Objetivos relacionados:** OBJ-XXX, OBJ-XXX

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-001 | [Título corto] | Must | [Descripción detallada del requerimiento] |
| RF-002 | [Título corto] | Should | [Descripción detallada del requerimiento] |
| RF-003 | [Título corto] | Could | [Descripción detallada del requerimiento] |

### 3.2 [Nombre del Módulo/Componente 2]

**Objetivos relacionados:** OBJ-XXX

| ID | Requerimiento | Prioridad | Descripción |
|----|---------------|-----------|-------------|
| RF-004 | [Título corto] | Must | [Descripción detallada del requerimiento] |
| RF-005 | [Título corto] | Should | [Descripción detallada del requerimiento] |

*Agrega más módulos/componentes según sea necesario.*

---

## 4. Historias de Usuario

*Las historias de usuario completas se encuentran en `docs/requirements/user-stories/`. A continuación se presenta un resumen con enlaces.*

| ID | Título | Prioridad | RF Relacionados | Archivo |
|----|--------|-----------|-----------------|---------|
| HU-001 | [Título de la historia] | Must | RF-001, RF-002 | [HU-001.md](../user-stories/HU-001.md) |
| HU-002 | [Título de la historia] | Should | RF-003 | [HU-002.md](../user-stories/HU-002.md) |
| HU-003 | [Título de la historia] | Could | RF-004, RF-005 | [HU-003.md](../user-stories/HU-003.md) |

---

## 5. Requerimientos No Funcionales

*Documenta los requerimientos de calidad del sistema. Cada RNF debe tener al menos un criterio de aceptación.*

### 5.1 Rendimiento

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-001 | [Descripción] | [MoSCoW] | [Criterio medible, ej: "Tiempo de respuesta < 500ms"] |

### 5.2 Seguridad

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-002 | [Descripción] | [MoSCoW] | [Criterio medible] |

### 5.3 Usabilidad

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-003 | [Descripción] | [MoSCoW] | [Criterio medible] |

### 5.4 Disponibilidad

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-004 | [Descripción] | [MoSCoW] | [Criterio medible, ej: "Uptime >= 99.5%"] |

### 5.5 Escalabilidad

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-005 | [Descripción] | [MoSCoW] | [Criterio medible] |

### 5.6 Compatibilidad

| ID | Requerimiento | Prioridad | Criterio de Aceptación |
|----|---------------|-----------|------------------------|
| RNF-006 | [Descripción] | [MoSCoW] | [Criterio medible, ej: "Compatible con Chrome, Firefox, Safari"] |

*Elimina las categorías que no apliquen. Agrega más RNF según sea necesario.*

---

## 6. Restricciones

*Copia las restricciones del Discovery.*

| ID | Restricción | Tipo | Impacto |
|----|-------------|------|---------|
| RES-001 | [Descripción de la restricción] | [Técnica/Negocio/Tiempo/Presupuesto/Regulatoria] | [Cómo afecta al alcance o solución] |
| RES-002 | [Descripción de la restricción] | [Tipo] | [Impacto] |

---

## 7. Dependencias

*Documenta las dependencias entre requerimientos o con sistemas externos.*

| ID | Dependencia | Tipo | Descripción |
|----|-------------|------|-------------|
| DEP-001 | [Qué depende de qué] | [Interna/Externa] | [Descripción de la dependencia] |
| DEP-002 | RF-005 requiere RF-003 | Interna | [RF-005 no puede implementarse hasta que RF-003 esté completo] |

**Tipos de dependencia:**
- **Interna:** Entre requerimientos del mismo SRS
- **Externa:** Con otros módulos, sistemas o servicios externos

---

## 8. Riesgos

*Identifica los riesgos asociados a la implementación de estos requerimientos.*

| ID | Riesgo | Probabilidad | Impacto | Mitigación Propuesta |
|----|--------|--------------|---------|----------------------|
| RIE-001 | [Descripción del riesgo] | Alta/Media/Baja | Alto/Medio/Bajo | [Acciones para mitigar] |
| RIE-002 | [Descripción del riesgo] | Alta/Media/Baja | Alto/Medio/Bajo | [Acciones para mitigar] |

---

## 9. Glosario

*Incluye esta sección solo si hay términos técnicos o acrónimos que requieran definición. Elimina si no es necesario.*

| Término | Definición |
|---------|------------|
| [Término 1] | [Definición] |
| [Acrónimo] | [Significado y descripción] |

---

## Checklist de Validación

*Antes de declarar el SRS como completado, verifica que se cumplan todos los criterios:*

### Completitud
- [ ] El resumen ejecutivo describe claramente qué se construirá
- [ ] Todos los objetivos del Discovery (OBJ-XXX) tienen al menos un RF asociado
- [ ] Todos los RF tienen prioridad MoSCoW asignada
- [ ] Cada RF tiene al menos una HU asociada
- [ ] Cada HU tiene al menos 1 criterio de aceptación

### Requerimientos No Funcionales
- [ ] Se han considerado todas las categorías de RNF relevantes
- [ ] Cada RNF tiene al menos 1 criterio de aceptación medible

### Trazabilidad
- [ ] Las restricciones del Discovery están copiadas
- [ ] Las dependencias entre requerimientos están documentadas
- [ ] Los riesgos están identificados con mitigación propuesta

### Calidad de Requerimientos
- [ ] Los requerimientos son completos (contienen toda la información necesaria)
- [ ] Los requerimientos son consistentes (no se contradicen)
- [ ] Los requerimientos son no ambiguos (una sola interpretación)
- [ ] Los requerimientos son verificables (se pueden probar)
- [ ] Los requerimientos son atómicos (una sola necesidad cada uno)
- [ ] Los requerimientos son factibles (realizables con las restricciones)

### Revisión
- [ ] Peer review completado
- [ ] Agent review completado

---

## Señal de Completación

> **Para el Agente:** Una vez completado el checklist, declara:
> 
> "SRS completado. Listo para validación del Orquestador."
