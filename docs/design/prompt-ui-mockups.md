# Prompt: Generacion de Mockups UI - GIIA

> **Proposito:** Este prompt esta optimizado para Claude Opus 4.6 con acceso al MCP Stitch (generacion de HTML/CSS estatico). Debe ejecutarse proporcionando como contexto adjunto el documento de diseno tecnico (`design-document.md`) y la imagen de referencia para el grafico de buffers (`idea_grafico_buffers.png`).
>
> **Instruccion de uso:** Copia el contenido de la seccion "PROMPT" completa y pegala en una conversacion nueva con Opus 4.6, adjuntando los archivos mencionados. Ejecuta las pantallas en el orden indicado en el plan de implementacion (Fase 1 a Fase 7).
>
> **Dependencia MCP:** Stitch (generacion de HTML/CSS estatico hi-fi)

---

## PROMPT

---

### ROL Y OBJETIVO

Eres un disenador UI/UX Senior especializado en aplicaciones SaaS B2B de gestion empresarial, con experiencia profunda en dashboards de datos en tiempo real, sistemas de semaforo visual y aplicaciones de inventario/supply chain. Tu tarea es generar **todas las pantallas del sistema GIIA como mockups HTML/CSS estaticos de alta fidelidad** utilizando la herramienta Stitch.

**Objetivo:** Producir un set completo y coherente de pantallas que comparten el mismo sistema de diseno, listas para servir como referencia visual definitiva para el equipo de desarrollo frontend.

**Tecnologia de referencia:** Next.js + shadcn/ui + Tailwind CSS.
**Nivel de fidelidad:** Alto (colores finales, tipografia, iconografia, datos realistas de ejemplo).

---

### PRINCIPIOS DE DISENO

Cada pantalla debe cumplir estos principios sin excepcion:

| Principio | Aplicacion |
|-----------|-----------|
| **Simplicidad** | Maxima claridad visual. Nada innecesario. Cada elemento tiene un proposito. |
| **Intuitivo** | Un usuario nuevo debe entender la pantalla en <5 segundos. Acciones principales visibles. |
| **Navegacion facil** | Sidebar persistente + breadcrumbs + acciones contextuales. Maximo 2 clicks para cualquier tarea frecuente. |
| **Moderno** | Estilo flat con sombras sutiles, bordes redondeados, espaciado generoso (Tailwind spacing scale). |
| **Visualmente atractivo** | Uso inteligente del color como informacion (semaforo), no como decoracion. Fondos limpios, jerarquia visual clara. |
| **Consistente** | Todos los elementos (tablas, formularios, cards, botones) siguen el mismo patron en todas las pantallas. |
| **Accesible** | Contraste WCAG AA, patrones adicionales al color (iconos, texturas) para el semaforo. |

---

### SISTEMA DE DISENO

#### Paleta de Colores

```
/* Base (shadcn/ui defaults con customizacion) */
--background:       #FFFFFF        /* Fondo principal */
--foreground:       #0F172A        /* Texto principal (slate-900) */
--muted:            #F1F5F9        /* Fondos secundarios (slate-100) */
--muted-foreground: #64748B        /* Texto secundario (slate-500) */
--border:           #E2E8F0        /* Bordes (slate-200) */
--card:             #FFFFFF        /* Fondo de cards */
--card-foreground:  #0F172A        /* Texto de cards */

/* Primario - Azul corporativo */
--primary:          #2563EB        /* blue-600 - Acciones principales */
--primary-hover:    #1D4ED8        /* blue-700 */
--primary-light:    #EFF6FF        /* blue-50 - Fondos de seleccion */

/* Semaforo DDMRP (CRITICO - no cambiar) */
--zone-green:       #16A34A        /* green-600 - Buffer saludable */
--zone-green-bg:    #DCFCE7        /* green-100 - Fondo verde */
--zone-yellow:      #F59E0B        /* amber-500 - Buffer precaucion */
--zone-yellow-bg:   #FEF3C7        /* amber-100 - Fondo amarillo */
--zone-red:         #DC2626        /* red-600 - Buffer critico base */
--zone-red-bg:      #FEE2E2        /* red-100 - Fondo rojo */
--zone-red-safety:  #991B1B        /* red-800 - Buffer critico seguridad */

/* Estados */
--success:          #16A34A        /* green-600 */
--warning:          #F59E0B        /* amber-500 */
--destructive:      #DC2626        /* red-600 */
--info:             #2563EB        /* blue-600 */

/* Sidebar */
--sidebar-bg:       #0F172A        /* slate-900 - Fondo oscuro */
--sidebar-text:     #CBD5E1        /* slate-300 */
--sidebar-active:   #2563EB        /* blue-600 - Item activo */
--sidebar-hover:    #1E293B        /* slate-800 */
```

#### Tipografia

```
Font familia:    Inter (Google Fonts)
H1:              24px / semibold / slate-900   (Titulo de pagina)
H2:              20px / semibold / slate-900   (Subtitulo/seccion)
H3:              16px / semibold / slate-900   (Titulo de card)
Body:            14px / regular / slate-700    (Texto general)
Small:           12px / regular / slate-500    (Labels, metadata)
Caption:         11px / medium / slate-400     (Texto auxiliar)
Monospace:       JetBrains Mono / 13px         (SKUs, codigos, numeros)
```

#### Espaciado y Layout

```
Sidebar:         width 256px (colapsable a 64px con iconos)
Header:          height 64px, sticky top
Content padding: 24px (p-6)
Card padding:    16px (p-4) o 24px (p-6)
Gap entre cards: 16px (gap-4) o 24px (gap-6)
Border radius:   8px (rounded-lg) para cards, 6px (rounded-md) para inputs
Sombra cards:    shadow-sm (0 1px 2px rgba(0,0,0,0.05))
Max content:     1440px centrado en pantallas >1440px
```

#### Iconografia

```
Libreria:        Lucide Icons (integrada con shadcn/ui)
Tamano sidebar:  20px
Tamano acciones: 16px
Tamano inline:   14px
```

#### Componentes Base (shadcn/ui)

Todos los mockups deben usar estos componentes con apariencia consistente:

| Componente | Estilo |
|-----------|--------|
| **Button** | Primary (azul), Secondary (outline), Destructive (rojo), Ghost (sin borde) |
| **Input** | Border slate-200, focus ring blue-600, placeholder slate-400, h-10 |
| **Select** | Igual que Input con chevron-down |
| **Table** | Header bg-slate-50, filas alternadas hover:bg-slate-50, bordes horizontales |
| **Card** | bg-white, border slate-200, shadow-sm, rounded-lg |
| **Badge** | Variantes: default (slate), success (green), warning (amber), destructive (red) |
| **Dialog/Modal** | Overlay oscuro, max-w-lg, centrado vertical, rounded-lg |
| **Tabs** | Underline style (no box), active = blue-600 border-bottom |
| **Toast** | Bottom-right, 4 variantes (success, error, warning, info) |
| **Breadcrumb** | Text-sm, separador /, ultima parte bold |
| **Pagination** | Numeros + Prev/Next, max 7 paginas visibles |
| **Skeleton** | Rectangulos animados slate-200 pulse |

---

### LAYOUT PRINCIPAL (SHELL)

Todas las pantallas dentro de la app (post-login) comparten este layout:

```
+----------------------------------------------------------+
| SIDEBAR (256px)  |  HEADER (64px, full width)             |
| - Logo GIIA      |  [Breadcrumbs]        [Alertas] [User] |
| - Navegacion     |----------------------------------------|
|   * Dashboard    |                                        |
|   * Proveedores  |  CONTENT AREA                          |
|   * Categorias   |  (scroll vertical independiente)       |
|   * Productos    |                                        |
|   * Ord. Compra  |  Padding: 24px                         |
|   * Ord. Venta   |  Max-width: 1440px (centrado)          |
|   * Ajustes Inv. |                                        |
|   * Planificacion|                                        |
|   * Alertas      |                                        |
|   * KPIs         |                                        |
|   * ------------ |                                        |
|   * Importacion  |                                        |
|   * Aj. Dinamicos|                                        |
|   * ------------ |                                        |
|   * Config       |                                        |
+----------------------------------------------------------+
```

**Sidebar:**
- Fondo oscuro (slate-900), texto claro (slate-300)
- Logo "GIIA" arriba en bold, blanco, con un icono de grafico estilizado
- Navegacion con iconos Lucide + label
- Item activo: fondo blue-600/10, borde izquierdo blue-600 3px, texto blanco
- Separadores visuales entre grupos de secciones (Datos maestros, Transacciones, DDMRP, Admin)
- Boton de colapsar sidebar en la parte inferior
- Badge numerico en "Alertas" mostrando conteo de no leidas

**Header:**
- Fondo blanco, border-bottom slate-200
- Izquierda: Breadcrumbs (ej: "Dashboard > Productos > PRD-001")
- Derecha: Icono campana con badge rojo de alertas no leidas + Avatar circular con menu dropdown (nombre, email, rol, cerrar sesion)

---

### REFERENCIA VISUAL: GRAFICO DE BUFFERS

Se adjunta la imagen `idea_grafico_buffers.png` como referencia para el estilo del grafico de estado de buffers. Los elementos clave a replicar son:

1. **Area chart apilado** con tres zonas: Verde (arriba), Amarilla (medio), Roja (abajo)
2. **Linea negra gruesa** representando la "Posicion de Flujo Neto" (Net Flow Position)
3. **Linea punteada blanca** representando "Target On-Hand" (Inventario objetivo)
4. **Linea con marcadores** representando "On-Hand" (Inventario real)
5. **Eje X** con fechas (timeline)
6. **Eje Y** con valores numericos (unidades)
7. **Leyenda** clara arriba del grafico
8. **Filtros laterales** con checkboxes para Site Id y Material

**Adaptaciones para GIIA:**
- Reemplazar "Site Id" por filtro de Categoria
- Reemplazar "Material" por filtro de Producto
- Mantener la misma idea visual de zonas coloreadas como area stacked chart
- La linea de flujo neto debe ser prominente (negra, 2px)
- Anadir linea horizontal punteada para TOG (Top of Green)
- El grafico debe mostrarse en: Dashboard (tendencia al clickear producto), Detalle de Producto (tab Buffer), y pantalla de KPIs

---

### PLAN DE IMPLEMENTACION POR FASES

Genera las pantallas en este orden. Cada fase debe completarse antes de pasar a la siguiente. Todas las pantallas deben compartir el mismo shell (sidebar + header).

---

## FASE 1: Fundacion (Shell + Auth)

### Pantalla 1.1: Login

```
Ruta: /login
Layout: Centrado, sin sidebar (auth layout)
```

**Estructura:**
- Fondo: gradiente sutil de slate-50 a blue-50
- Card centrada (max-w-md), sombra media
- Logo GIIA grande arriba (icon + texto)
- Subtitulo: "Gestion de Inventario Inteligente"
- Campos:
  - Email (input con icono Mail)
  - Contrasena (input con icono Lock + toggle ojo para mostrar/ocultar)
- Boton "Iniciar Sesion" (primary, full width)
- Link "Olvide mi contrasena" debajo (text-sm, blue-600)
- Footer: "GIIA v1.0 - (c) 2026"

**Datos de ejemplo:**
- Email placeholder: "usuario@empresa.com"

---

### Pantalla 1.2: Recuperar Contrasena

```
Ruta: /forgot-password
Layout: Centrado, sin sidebar (auth layout)
```

**Estructura:**
- Misma card centrada que Login
- Icono grande de sobre (Mail) arriba
- Titulo: "Recuperar Contrasena"
- Texto: "Ingresa tu email y te enviaremos instrucciones para restablecer tu contrasena."
- Campo: Email
- Boton "Enviar Instrucciones" (primary, full width)
- Link "Volver al login" (text-sm)

---

### Pantalla 1.3: Restablecer Contrasena

```
Ruta: /reset-password
Layout: Centrado, sin sidebar (auth layout)
```

**Estructura:**
- Card centrada
- Icono de llave (KeyRound)
- Titulo: "Nueva Contrasena"
- Campos:
  - Nueva contrasena (con indicador de fortaleza: barra roja/amarilla/verde)
  - Confirmar contrasena
- Boton "Restablecer Contrasena" (primary)
- Link "Volver al login"

---

### Pantalla 1.4: Shell / Layout Principal

```
Layout: Sidebar + Header + Content vacio
```

Genera el shell principal con:
- Sidebar completo con todos los items de navegacion y sus iconos
- Header con breadcrumbs, campana de notificacion (badge "3"), avatar de usuario
- Area de contenido vacia con texto "Selecciona una seccion"
- Mostrar el sidebar tanto expandido (256px) como colapsado (64px, solo iconos)

**Items de navegacion del sidebar con iconos Lucide:**

| Grupo | Item | Icono Lucide |
|-------|------|-------------|
| Principal | Dashboard | LayoutDashboard |
| Datos Maestros | Proveedores | Truck |
| Datos Maestros | Categorias | FolderTree |
| Datos Maestros | Productos | Package |
| Transacciones | Ordenes de Compra | ShoppingCart |
| Transacciones | Ordenes de Venta | Receipt |
| Transacciones | Ajustes Inventario | ArrowLeftRight |
| DDMRP | Planificacion | CalendarClock |
| DDMRP | Ajustes Dinamicos | SlidersHorizontal |
| Monitoreo | Alertas | Bell (con badge) |
| Monitoreo | KPIs | BarChart3 |
| Admin | Importacion CSV | Upload |
| Admin | Configuracion | Settings |

---

## FASE 2: Dashboard y Visualizacion Core

### Pantalla 2.1: Dashboard Principal

```
Ruta: /dashboard
Permisos: Todos los roles
Polling: 30s (estado), 60s (alertas), 5min (KPIs)
```

Esta es la pantalla MAS IMPORTANTE del sistema. El usuario la ve al iniciar sesion. Debe comunicar el estado del inventario de un vistazo.

**Layout (de arriba a abajo):**

**Fila 1 - Encabezado:**
- H1: "Dashboard" + Badge con hora de ultima actualizacion ("Actualizado hace 15s")
- Filtros inline a la derecha: Categoria (TreeSelect), Proveedor (Select), Estado Buffer (MultiSelect: Verde/Amarillo/Rojo)

**Fila 2 - KPIs + Semaforo (grid 5 columnas):**

| Col 1: Semaforo | Col 2: KPI | Col 3: KPI | Col 4: KPI | Col 5: KPI |
|----------------|-----------|-----------|-----------|-----------|

- **Semaforo (col 1, card mas alta):** Donut chart con 3 segmentos (verde/amarillo/rojo). Centro muestra total de productos. Debajo del donut: 3 lineas con icono + numero:
  - Circulo verde: "142 Saludables (68%)"
  - Circulo amarillo: "47 Precaucion (22%)"
  - Triangulo rojo: "21 Criticos (10%)"

- **KPI Cards (cols 2-5, cada una es un card):**
  Cada card muestra:
  - Icono pequeno + label en slate-500 (ej: "Costo Total Inventario")
  - Valor grande en slate-900 (ej: "$1,245,680.00")
  - Cambio vs periodo anterior: flecha verde arriba "+3.2% vs mes anterior" o flecha roja abajo "-1.5%"

  KPIs:
  1. **Costo Total Inventario** - icono DollarSign - "$1,245,680.00" - +3.2%
  2. **Dias de Inventario Promedio** - icono Calendar - "18.5 dias" - -2.1 dias
  3. **Capital Inmovilizado** - icono Lock - "$89,420.00" - -12.5%
  4. **Rotacion de Inventario** - icono RefreshCw - "4.2x" - +0.3x

**Fila 3 - Productos Criticos (tabla principal):**

Card con titulo "Productos Criticos" + badge rojo "21 productos" + boton "Ver Planificacion" a la derecha.

Tabla con columnas:
| Prioridad | SKU | Producto | Categoria | Proveedor | Buffer | Flujo Neto | Sugerencia | Accion |
|-----------|-----|----------|-----------|-----------|--------|-----------|------------|--------|

- **Prioridad:** Numero + badge de severidad (1, 2, 3...)
- **Buffer:** Barra horizontal inline (mini) mostrando zonas R/A/V con linea de posicion actual. Ancho ~120px.
  - La barra tiene 3 segmentos coloreados (rojo, amarillo, verde) proporcionales a las zonas calculadas
  - Una linea vertical negra indica la posicion del flujo neto
  - Si esta en rojo, la barra pulsa sutilmente
- **Flujo Neto:** Numero + porcentaje de penetracion en zona roja (ej: "45 uds (85% penetracion)")
- **Sugerencia:** "Reponer 110 uds" en texto blue-600
- **Accion:** Boton ghost "Ver detalle" con icono ExternalLink

**Datos de ejemplo para la tabla (10 filas):**

| # | SKU | Producto | Categoria | Proveedor | Flujo Neto | Penetracion | Sugerencia |
|---|-----|----------|-----------|-----------|-----------|-------------|------------|
| 1 | PRD-0421 | Pintura Latex Blanca 20L | Pinturas > Latex | ProveedorA S.A. | 12 uds | 92% | 198 uds |
| 2 | PRD-0087 | Tubo PVC 4" x 6m | Plomeria > Tubos | Plasticos XYZ | 28 uds | 78% | 142 uds |
| 3 | PRD-0193 | Cable THW 12 AWG Rojo 100m | Electricidad > Cables | Conductores MX | 35 uds | 71% | 115 uds |
| 4 | PRD-0512 | Cemento Portland 50kg | Construccion > Cementos | Cementera Nacional | 45 uds | 65% | 205 uds |
| 5 | PRD-0034 | Lamina Galvanizada C26 | Aceros > Laminas | Aceros del Norte | 18 uds | 88% | 82 uds |
| 6 | PRD-0298 | Impermeabilizante 19L Rojo | Impermeabilizantes | QuimicaPlus | 52 uds | 58% | 98 uds |
| 7 | PRD-0156 | Varilla Corrugada 3/8" | Aceros > Varillas | Aceros del Norte | 65 uds | 52% | 135 uds |
| 8 | PRD-0743 | Pegamento PVC 500ml | Plomeria > Adhesivos | Plasticos XYZ | 8 uds | 95% | 42 uds |
| 9 | PRD-0601 | Foco LED 10W Luz Fria | Electricidad > Focos | Iluminacion Global | 40 uds | 62% | 60 uds |
| 10 | PRD-0089 | Llave de Paso 1/2" | Plomeria > Valvulas | Válvulas Corp | 22 uds | 75% | 38 uds |

**Fila 4 - Tendencia de Buffer (aparece al clickear un producto):**

Card expandible que aparece debajo de la tabla al seleccionar un producto. Muestra:
- Titulo: "Tendencia: Pintura Latex Blanca 20L (PRD-0421) - Ultimos 30 dias"
- **Grafico tipo area stacked** (REFERENCIA: idea_grafico_buffers.png):
  - Eje X: fechas (ultimos 30 dias)
  - Eje Y: unidades
  - Areas apiladas: Zona Roja (abajo), Zona Amarilla (medio), Zona Verde (arriba)
  - Linea negra gruesa: Flujo Neto
  - Linea punteada gris: TOG (Top of Green)
  - Marcadores circulares en la linea de flujo neto donde cambio de zona
- Leyenda encima del grafico: [Verde] Zona Verde  [Amarillo] Zona Amarilla  [Rojo] Zona Roja  [Linea negra] Flujo Neto  [Punteada] TOG

---

### Pantalla 2.2: Detalle de Producto (vista completa con buffer)

```
Ruta: /products/[id]
Permisos: Todos (RO: solo lectura)
```

**Layout:**

**Header de pagina:**
- Breadcrumb: "Productos > PRD-0421"
- H1: "Pintura Latex Blanca 20L" + Badge de estado buffer (rojo: "Critico")
- Subtitulo: "SKU: PRD-0421 | Categoria: Pinturas > Latex | Unidad: Litros"
- Acciones: Boton "Editar" (secondary) + Boton "Desactivar" (destructive ghost)

**Tabs:**
[ Informacion General | Proveedores | Buffer & Zonas | Transacciones | Historial CPD ]

**Tab "Informacion General":**
- Grid 2 columnas con cards:
  - **Card Datos Basicos:** Nombre, SKU, Descripcion, Categoria, Unidad de Medida, Costo unitario ($125.00 USD / $2,500.00 MXN), Estado (Activo)
  - **Card Resumen Rapido:** Inventario Fisico (12 uds), En Transito (50 uds), Flujo Neto (45 uds), CPD (8.5 uds/dia), Estado Buffer (badge rojo "Critico - 92% penetracion")

**Tab "Proveedores":**
- Tabla de proveedores asignados:
  | Proveedor | Lead Time | MOQ | Frecuencia | Preferido | Acciones |
  - El proveedor preferido tiene una estrella amarilla
  - Botones: Editar, Quitar
  - Boton "Asignar Proveedor" arriba de la tabla

**Tab "Buffer & Zonas" (la mas importante visualmente):**
- **Card superior:** Visualizacion de buffer actual
  - Barra horizontal grande (altura ~48px, ancho completo):
    - Zona Roja Safety (rojo oscuro #991B1B) | Zona Roja Base (rojo #DC2626) | Zona Amarilla (#F59E0B) | Zona Verde (#16A34A)
    - Marcador triangular debajo indicando posicion del Flujo Neto
    - Labels encima de cada zona con el valor (ej: "35.7" | "24.3" | "85.0" | "65.0")
    - Label del TOG al final: "TOG: 210.0"
    - Label del Flujo Neto: "Flujo Neto: 45.0 uds"

- **Card Parametros del Perfil de Buffer:**
  Grid 3 columnas:
  | Parametro | Valor |
  | Categoria Lead Time | Medio |
  | Factor Lead Time | 0.35 |
  | Factor Variabilidad | 0.25 |
  | Ventana CPD | 7 dias |
  | LT Proveedor Preferido | 10 dias |
  | MOQ | 50 uds |
  | Frecuencia Pedido | 15 dias |
  Boton "Editar Perfil" (secondary)

- **Card Grafico de Tendencia:**
  - Grafico area stacked (30 dias) - USAR REFERENCIA VISUAL
  - Mismo estilo que en Dashboard pero mas grande (height ~350px)

**Tab "Transacciones":**
- Tabla con historial:
  | Fecha | Tipo | Referencia | Cantidad | Saldo | Notas |
  - Tipos con badges coloreados: Ingreso (verde), Egreso (rojo), Ajuste+ (blue), Ajuste- (amber)
  - Filtros: Tipo, Rango de fechas
  - Paginacion

**Tab "Historial CPD":**
- Grafico de linea mostrando evolucion del CPD en los ultimos 90 dias
- Tabla debajo con valores diarios
- Indicador de origen: "Manual" (badge azul) vs "Calculado" (badge verde)

---

## FASE 3: Datos Maestros (CRUD)

Todas las pantallas CRUD siguen el mismo patron de diseno para consistencia.

### Patron de Pantalla de Lista (aplicar a todas las listas)

```
+--------------------------------------------------+
| H1: [Nombre Seccion]   [Boton "+ Crear Nuevo"]   |
|--------------------------------------------------|
| Barra de filtros: [Busqueda] [Filtro1] [Filtro2] |
|--------------------------------------------------|
| Tabla con:                                        |
|  - Header fijo (sticky)                           |
|  - Filas con hover highlight                      |
|  - Acciones por fila (Ver, Editar, Desactivar)    |
|  - Checkbox para seleccion multiple               |
|  - Paginacion abajo                               |
|  - Texto "Mostrando 1-20 de 156 resultados"      |
+--------------------------------------------------+
```

### Patron de Pantalla de Formulario (aplicar a todos los formularios)

```
+--------------------------------------------------+
| Breadcrumb: Seccion > Crear Nuevo                |
| H1: "Crear [Entidad]" o "Editar [Entidad]"      |
|--------------------------------------------------|
| Card con formulario:                              |
|  - Campos organizados en grid 2 columnas          |
|  - Labels arriba de cada campo                    |
|  - Asterisco rojo en campos obligatorios          |
|  - Mensajes de error debajo del campo (rojo)      |
|  - Mensajes de exito debajo del campo (verde)     |
|--------------------------------------------------|
| Footer sticky:                                    |
|  [Cancelar] (secondary)    [Guardar] (primary)   |
+--------------------------------------------------+
```

---

### Pantalla 3.1: Lista de Proveedores

```
Ruta: /suppliers
Permisos: Todos (RO: no ve boton crear)
```

**Filtros:** Busqueda (nombre/contacto), Estado (Activo/Inactivo)

**Columnas de tabla:**
| Nombre | Contacto | Telefono | Email | Ciudad, Pais | Productos | Estado | Acciones |
- **Productos:** Numero de productos asociados (link clickeable)
- **Estado:** Badge verde "Activo" o badge gris "Inactivo"
- **Acciones:** Iconos Ver (Eye), Editar (Pencil), Desactivar (UserMinus)

**Datos de ejemplo (8 filas):**
| ProveedorA S.A. | Juan Perez | +52 55 1234 5678 | juan@proveedora.com | CDMX, Mexico | 45 | Activo |
| Plasticos XYZ | Maria Lopez | +52 33 8765 4321 | maria@plastxyz.com | Guadalajara, Mexico | 32 | Activo |
| Conductores MX | Pedro Gomez | +52 81 2345 6789 | pedro@condmx.com | Monterrey, Mexico | 18 | Activo |
| Cementera Nacional | Ana Torres | +52 55 3456 7890 | ana@cemnac.com | CDMX, Mexico | 12 | Activo |
| Aceros del Norte | Roberto Diaz | +52 81 4567 8901 | rdiaz@acenor.com | Monterrey, Mexico | 28 | Activo |
| QuimicaPlus | Laura Sanchez | +52 33 5678 9012 | laura@quimplus.com | Guadalajara, Mexico | 15 | Activo |
| Iluminacion Global | Carlos Ruiz | +52 55 6789 0123 | cruiz@ilumglob.com | CDMX, Mexico | 22 | Inactivo |
| Valvulas Corp | Diana Martinez | +52 81 7890 1234 | diana@valvcorp.com | Monterrey, Mexico | 8 | Activo |

---

### Pantalla 3.2: Formulario Proveedor (Crear/Editar)

```
Ruta: /suppliers/new o /suppliers/[id] (modo edicion)
Permisos: Admin, User
```

**Campos (grid 2 columnas):**
- Nombre de empresa* (text)
- Nombre de contacto* (text)
- Email* (email, validacion formato)
- Telefono (tel, formato libre)
- Direccion (text)
- Ciudad* (text)
- Pais* (select con paises)
- Notas (textarea, 3 lineas)

---

### Pantalla 3.3: Detalle de Proveedor

```
Ruta: /suppliers/[id]
Permisos: Todos
```

**Layout:**
- Card con datos del proveedor (grid 2 columnas, read-only con labels)
- Tabla de productos asociados abajo:
  | SKU | Producto | Lead Time | MOQ | Frecuencia | Preferido |
- Boton "Editar" en header

---

### Pantalla 3.4: Categorias (Vista de Arbol)

```
Ruta: /categories
Permisos: Todos (RO: no edita)
```

**Layout:**
- H1: "Categorias" + Boton "+ Nueva Categoria"
- Vista de arbol expandible/contraible (TreeView):
  - Cada nodo muestra: Icono carpeta + Nombre + Badge con # productos
  - Flecha para expandir/contraer
  - On hover: iconos de Editar (Pencil) y Eliminar (Trash2)
  - Drag-and-drop visual para reorganizar (opcional, indicar con cursor grab)

**Datos de ejemplo:**
```
v Construccion (45)
    v Cementos (12)
    v Varillas (8)
    > Blocks (6)
    > Herramientas (19)
v Plomeria (28)
    v Tubos (10)
    v Valvulas (8)
    > Adhesivos (5)
    > Conexiones (5)
v Electricidad (35)
    v Cables (15)
    v Focos (10)
    > Apagadores (5)
    > Contactos (5)
v Pinturas (22)
    v Latex (12)
    > Esmalte (6)
    > Impermeabilizantes (4)
v Aceros (18)
    v Laminas (8)
    > Perfiles (6)
    > Varillas (4)
```

- Modal para crear/editar categoria: Nombre, Categoria padre (TreeSelect), Descripcion

---

### Pantalla 3.5: Lista de Productos

```
Ruta: /products
Permisos: Todos (RO: no ve crear)
```

**Filtros:** Busqueda (SKU/nombre, autocompletado fuzzy), Categoria (TreeSelect), Proveedor (Select), Estado Buffer (MultiSelect: Verde/Amarillo/Rojo), Estado (Activo/Inactivo)

**Columnas:**
| SKU | Producto | Categoria | Costo | CPD | Buffer | Flujo Neto | Estado | Acciones |
- **SKU:** Font monospace
- **Buffer:** Mini barra horizontal con zonas (mismo componente que Dashboard)
- **Flujo Neto:** Numero + badge de color segun zona
- **Estado:** Badge Activo/Inactivo

**Datos de ejemplo:** Reutilizar los 10 productos de la tabla del Dashboard + agregar 5 mas en zona amarilla y 5 en zona verde para balance.

---

### Pantalla 3.6: Crear Producto (Wizard Multi-paso)

```
Ruta: /products/new
Permisos: Admin, User
```

**Wizard de 4 pasos (stepper horizontal arriba):**

```
[1. Datos Basicos] ---- [2. Proveedores] ---- [3. Perfil Buffer] ---- [4. Revision]
     (activo)             (pendiente)           (pendiente)            (pendiente)
```

**Paso 1 - Datos Basicos:**
- SKU* (input monospace, con validacion async "SKU disponible" checkmark verde o "SKU ya existe" X roja)
- Nombre* (text)
- Descripcion (textarea)
- Categoria* (TreeSelect)
- Unidad de Medida* (select: Unidades, Litros, Kilogramos, Metros, Piezas)
- Costo Unitario* (CurrencyInput con selector USD/MXN)
- Botones: [Cancelar] [Siguiente ->]

**Paso 2 - Proveedores:**
- Tabla editable inline:
  | Proveedor (FuzzyAutocomplete) | Lead Time (dias) | MOQ | Frecuencia (dias) | Preferido (radio) |
  - Boton "+ Agregar Proveedor"
  - Al menos 1 proveedor requerido, al menos 1 marcado como preferido
- Botones: [<- Anterior] [Siguiente ->]

**Paso 3 - Perfil de Buffer:**
- Titulo: "Configuracion de Buffer DDMRP"
- Texto explicativo: "El sistema calculara automaticamente las zonas del buffer basandose en estos parametros."
- Campos:
  - Categoria Lead Time* (select: Corto <5d, Medio 5-15d, Largo >15d) - auto-sugerido segun LT del proveedor preferido
  - Factor Lead Time* (number, 0.1-1.0, default 0.35, step 0.05)
  - Factor Variabilidad* (number, 0.1-1.0, default 0.25, step 0.05)
- Card informativa: "El CPD (Consumo Promedio Diario) sera calculado automaticamente a partir del historial de ventas."
- Checkbox: "Este producto no tiene historial de ventas"
  - Si checked: campo "CPD Estimado" (number, placeholder "Unidades por dia")
- Preview de zonas calculadas (read-only, actualizado en tiempo real):
  - Barra horizontal grande mostrando las zonas calculadas con valores
- Botones: [<- Anterior] [Siguiente ->]

**Paso 4 - Revision:**
- Resumen de todos los datos en formato read-only
- Seccion "Datos Basicos" (card)
- Seccion "Proveedores" (mini tabla)
- Seccion "Perfil de Buffer" (card con parametros + preview de zonas)
- Botones: [<- Anterior] [Confirmar y Crear] (primary, icono Check)

---

## FASE 4: Transacciones

### Pantalla 4.1: Lista de Ordenes de Compra

```
Ruta: /purchase-orders
Permisos: Todos
```

**Filtros:** Busqueda (# OC), Proveedor (Select), Estado (MultiSelect: Pendiente/Parcial/Recibida/Cancelada), Rango de fechas

**Columnas:**
| # OC | Proveedor | Fecha Emision | Fecha Est. Llegada | Productos | Total | Estado | Acciones |
- **# OC:** "OC-2026-0045" (monospace)
- **Estado:** Badges coloreados: Pendiente (amber), Parcialmente Recibida (blue), Recibida (green), Cancelada (slate)
- **Total:** Monto en moneda base

**Datos de ejemplo (8 filas):**
| OC-2026-0045 | ProveedorA S.A. | 05/02/2026 | 15/02/2026 | 3 | $15,750.00 | Pendiente |
| OC-2026-0044 | Plasticos XYZ | 03/02/2026 | 18/02/2026 | 2 | $8,320.00 | Pendiente |
| OC-2026-0043 | Aceros del Norte | 01/02/2026 | 12/02/2026 | 4 | $42,100.00 | Parcialmente Recibida |
| OC-2026-0042 | Cementera Nacional | 28/01/2026 | 08/02/2026 | 1 | $25,000.00 | Recibida |
| OC-2026-0041 | Conductores MX | 25/01/2026 | 05/02/2026 | 3 | $12,450.00 | Recibida |
| OC-2026-0040 | QuimicaPlus | 20/01/2026 | 02/02/2026 | 2 | $18,900.00 | Recibida |
| OC-2026-0039 | ProveedorA S.A. | 15/01/2026 | 25/01/2026 | 5 | $31,200.00 | Recibida |
| OC-2026-0038 | Plasticos XYZ | 10/01/2026 | 20/01/2026 | 1 | $4,500.00 | Cancelada |

---

### Pantalla 4.2: Crear Orden de Compra

```
Ruta: /purchase-orders/new
Permisos: Admin, User
```

**Layout:**
- Proveedor* (FuzzyAutocomplete)
- Fecha de emision* (DatePicker, default hoy)
- Notas (textarea)
- **Tabla de lineas (editable):**
  | Producto (FuzzyAutocomplete, filtrado por proveedor) | Cantidad* | Precio Unitario* | Subtotal (auto) |
  - Boton "+ Agregar Linea"
  - Cada linea muestra info del buffer del producto: mini barra + flujo neto actual
  - Total general calculado al fondo
- Card informativa derecha:
  - "Fecha estimada de llegada: 15/02/2026" (calculada: fecha emision + LT del proveedor)
  - "LT del proveedor: 10 dias"

---

### Pantalla 4.3: Detalle de Orden de Compra

```
Ruta: /purchase-orders/[id]
Permisos: Todos
```

**Header:**
- H1: "OC-2026-0045" + Badge estado (Pendiente, amber)
- Acciones: [Editar] [Registrar Recepcion] (primary) [Cancelar OC] (destructive ghost)

**Layout:**
- **Card datos generales:**
  - Proveedor, Fecha emision, Fecha est. llegada, Total, Notas
- **Tabla de lineas:**
  | Producto | Cantidad Pedida | Cantidad Recibida | Pendiente | Precio Unit. | Subtotal |
- **Seccion "Registrar Recepcion" (expandible/modal):**
  - Tabla editable: por cada linea, input de "Cantidad Recibida"
  - Si es parcial, el estado cambia a "Parcialmente Recibida"
  - Nota de recepcion (textarea)
  - Boton "Confirmar Recepcion" (primary)
- **Card "Desvio de Lead Time" (si ya fue recibida):**
  - LT Estimado: 10 dias
  - LT Real: 12 dias
  - Desvio: +2 dias (+20%) - badge warning

---

### Pantalla 4.4: Lista de Ordenes de Venta

```
Ruta: /sales-orders
Permisos: Todos
```

**Filtros:** Busqueda (# OV), Estado (En Firme/Efectuada/Cancelada), Rango de fechas

**Columnas:**
| # OV | Cliente (ref) | Fecha | Productos | Total | Estado | Acciones |
- **Estado:** En Firme (blue), Efectuada (green), Cancelada (slate)

---

### Pantalla 4.5: Crear Orden de Venta

```
Ruta: /sales-orders/new
Permisos: Admin, User
```

**Layout:**
- Referencia del cliente (text, opcional)
- Fecha* (DatePicker, default hoy)
- Notas (textarea)
- **Tabla de lineas:**
  | Producto (FuzzyAutocomplete) | Cantidad* | Precio Unitario* | Subtotal |
  - Warning inline si cantidad > stock disponible: "Stock actual: 12 uds. La cantidad solicitada excede el stock."
  - Boton "+ Agregar Linea"
- Boton de guardado: "Registrar OV en Firme" (primary)

---

### Pantalla 4.6: Detalle de Orden de Venta

```
Ruta: /sales-orders/[id]
Permisos: Todos
```

- Header: "OV-2026-0123" + Badge estado
- Acciones: [Efectuar OV] (primary, solo si "En Firme") [Cancelar OV] (destructive)
- Al efectuar: Dialog de confirmacion "Al efectuar esta OV se descontara el inventario de los productos incluidos. Esta accion no se puede deshacer."
- Datos + tabla de lineas (read-only)

---

### Pantalla 4.7: Lista de Ajustes Extraordinarios

```
Ruta: /adjustments
Permisos: Todos
```

**Filtros:** Direccion (Ingreso/Egreso), Tipo, Producto, Rango de fechas

**Columnas:**
| Fecha | Producto | Direccion | Tipo | Cantidad | Justificacion | Registrado por |
- **Direccion:** Badge verde "Ingreso" o badge rojo "Egreso"
- **Tipo:** Devolucion, Ajuste+, Traspaso, Perdida, Merma, Obsolescencia, Ajuste-, Muestra

---

### Pantalla 4.8: Registrar Ajuste Extraordinario

```
Ruta: /adjustments/new
Permisos: Admin, User
```

**Campos:**
- Producto* (FuzzyAutocomplete)
  - Al seleccionar: Card info "Stock actual: 180 uds"
- Direccion* (Radio: Ingreso / Egreso)
- Tipo* (Select, opciones dependen de Direccion):
  - Ingreso: Devolucion, Ajuste positivo, Traspaso entrada
  - Egreso: Perdida, Merma, Obsolescencia, Ajuste negativo, Muestra
- Cantidad* (number)
  - Si egreso y cantidad > stock: warning "La cantidad excede el stock disponible (180 uds)"
- Justificacion* (textarea, minimo 10 caracteres)
- Fecha* (DatePicker, default hoy)

---

## FASE 5: DDMRP y Planificacion

### Pantalla 5.1: Planificacion de Reposiciones

```
Ruta: /planning
Permisos: Todos (RO: no crea OC)
```

**Esta pantalla es CRITICA para el flujo de trabajo diario.**

**Layout:**

**Fila 1 - Stats cards:**
| Total sugerencias: 21 | Proveedores involucrados: 5 | Valor total sugerido: $125,400.00 |

**Fila 2 - Filtros + Agrupacion:**
- Filtros: Proveedor (Select), Categoria (TreeSelect), Prioridad minima (slider)
- Toggle: "Agrupar por proveedor" (switch)

**Fila 3 - Tabla de sugerencias (con seleccion multiple):**

Modo SIN agrupar:
| Checkbox | Prioridad | SKU | Producto | Proveedor | Flujo Neto | TOG | Sugerencia | Costo Est. | Buffer |
- Checkbox para seleccionar filas
- **Prioridad:** Numero + icono (1-5: triangulo rojo, 6-10: circulo amarillo, 11+: circulo verde)
- **Buffer:** Mini barra horizontal inline

Modo AGRUPADO POR PROVEEDOR:
- Secciones colapsables por proveedor:
  ```
  v ProveedorA S.A. (8 sugerencias | $45,200.00 total)
    | [] | 1 | PRD-0421 | Pintura Latex... | 12 | 210 | 198 | $24,750.00 | [barra] |
    | [] | 4 | PRD-0512 | Cemento Portland... | 45 | 250 | 205 | $10,250.00 | [barra] |
    ...
  v Plasticos XYZ (3 sugerencias | $18,600.00 total)
    ...
  ```

**Fila 4 - Barra de acciones (sticky bottom, aparece al seleccionar productos):**
```
[3 productos seleccionados de ProveedorA S.A. | Total: $35,000.00]  [Ajustar Cantidades] [Crear Orden de Compra ->]
```

- Al clickear "Ajustar Cantidades": Modal donde se pueden editar las cantidades sugeridas antes de crear la OC
- Al clickear "Crear Orden de Compra": Pre-fill formulario de OC con los productos seleccionados

---

### Pantalla 5.2: Calendario de Llegadas (Should)

```
Ruta: /planning/calendar
Permisos: Todos
```

**Layout:**
- Vista mensual tipo calendario
- Cada dia muestra OC que llegan ese dia:
  - Mini card: "OC-045 | ProveedorA | 3 productos"
  - Color de borde segun si es fecha estimada (blue) o ya vencida (red)
- Hoy marcado con borde azul
- OC vencidas (pasaron la fecha y no se recibieron) en rojo

---

### Pantalla 5.3: Ajustes Dinamicos

```
Ruta: /dynamic-adjustments
Permisos: Todos (RO: no crea)
```

**Layout:**
- H1: "Ajustes Dinamicos" + Boton "+ Nuevo Ajuste"
- Card informativa: "Los ajustes dinamicos modifican temporalmente los calculos del buffer para periodos especificos (temporadas, promociones, eventos)."

**Tabla:**
| Nombre | Producto | Tipo | Factor | Fecha Inicio | Fecha Fin | Estado | Acciones |
- **Tipo:** Badge: FAD (blue), FAZ (green), FALTD (amber)
- **Factor:** Numero (ej: 1.5x, 0.8x)
- **Estado:** Activo (verde, si fecha actual esta en rango), Programado (blue, si fecha inicio es futura), Expirado (gris)

**Datos de ejemplo:**
| Temporada Navidad | PRD-0421 | FAD | 1.5x | 01/12/2025 | 31/12/2025 | Expirado |
| Promo San Valentin | PRD-0193 | FAD | 1.3x | 01/02/2026 | 14/02/2026 | Activo |
| Retraso Proveedor | PRD-0087 | FALTD | 1.4x | 10/02/2026 | 28/02/2026 | Activo |
| Temporada Lluvias | PRD-0298 | FAZ | 1.25x | 01/06/2026 | 30/09/2026 | Programado |

**Formulario (modal o pagina):**
- Producto* (FuzzyAutocomplete)
- Tipo de ajuste* (Select: FAD, FAZ, FALTD)
  - FAD: Modifica el CPD (demanda)
  - FAZ: Modifica directamente las zonas
  - FALTD: Modifica el Lead Time
- Nombre descriptivo* (text)
- Factor* (number, min 0.1, max 5.0, step 0.05)
- Fecha inicio* (DatePicker)
- Fecha fin* (DatePicker, debe ser > fecha inicio)
- Justificacion (textarea)
- Preview: "Con este ajuste, el CPD pasara de 8.5 a 12.75 uds/dia"

---

## FASE 6: Monitoreo

### Pantalla 6.1: Centro de Alertas

```
Ruta: /alerts
Permisos: Todos
```

**Layout:**

**Header:**
- H1: "Centro de Alertas" + Badge "12 sin leer"
- Boton "Marcar todas como leidas" (ghost)

**Filtros:**
- Tipo (MultiSelect: Buffer en rojo, Llegada proxima, Llegada vencida, Sobrestock)
- Severidad (Select: Critica, Alta, Media)
- Estado (Toggle: Todas / Solo no leidas)
- Rango de fechas

**Lista de alertas (cards, no tabla):**
Cada alerta es un card horizontal:
```
+----------+------------------------------------------------------+-----------+
| [icono]  | Titulo                                               | Hace 2h   |
| [color]  | Descripcion detallada                                | [Marcar   |
|          | Producto: PRD-0421 - Pintura Latex Blanca 20L        |  leida]   |
+----------+------------------------------------------------------+-----------+
```

- Iconos por tipo:
  - Buffer rojo: AlertTriangle (rojo)
  - Llegada proxima: Truck (blue)
  - Llegada vencida: Clock (amber)
  - Sobrestock: TrendingUp (green)
- Background sutil para no leidas (blue-50)
- Cards leidas: fondo blanco, opacidad reducida

**Datos de ejemplo (8 alertas):**
1. [CRITICA] "Producto en zona roja critica" - PRD-0421 Pintura Latex - Penetracion 92% - Hace 15 min - No leida
2. [CRITICA] "Producto en zona roja" - PRD-0743 Pegamento PVC - Penetracion 95% - Hace 30 min - No leida
3. [ALTA] "OC vencida - No recibida" - OC-2026-0043 Aceros del Norte - 2 dias de retraso - Hace 2h - No leida
4. [MEDIA] "Llegada proxima" - OC-2026-0045 ProveedorA S.A. - Llega en 3 dias - Hace 4h - Leida
5. [ALTA] "Producto en zona roja" - PRD-0034 Lamina Galvanizada - Penetracion 88% - Hace 6h - Leida
6. [MEDIA] "Desvio de Lead Time" - OC-2026-0042 Cementera Nacional - LT real 12d vs estimado 10d - Hace 1 dia - Leida
7. [ALTA] "Producto en zona roja" - PRD-0601 Foco LED - Penetracion 62% - Hace 1 dia - Leida
8. [MEDIA] "Llegada proxima" - OC-2026-0044 Plasticos XYZ - Llega en 5 dias - Hace 2 dias - Leida

**Al clickear una alerta:** Expandir detalles + link "Ver producto" o "Ver OC" segun corresponda

---

### Pantalla 6.2: Dashboard de KPIs

```
Ruta: /kpis
Permisos: Todos
```

**Layout:**

**Fila 1 - Selector de periodo:**
- Rango de fechas (DateRangePicker)
- Presets rapidos: "Ultima semana", "Ultimo mes", "Ultimo trimestre", "Ultimo ano"
- Boton "Exportar" (secondary, icono Download) con opciones CSV/PDF

**Fila 2 - 4 KPI Cards grandes (grid 4 columnas):**
Cada card mas detallada que en el Dashboard:
- Label
- Valor actual (grande)
- Valor periodo anterior (small, gris)
- Cambio % (con flecha y color)
- Mini sparkline chart (linea de tendencia 30 dias, height ~40px)

**Fila 3 - Grafico principal:**
Card con:
- Tabs: [Costo Total | Dias Inventario | Inmovilizado | Rotacion]
- Grafico de linea grande (height ~300px):
  - Eje X: fechas del rango seleccionado
  - Eje Y: valor del KPI
  - Linea principal azul con area fill sutil
  - Linea punteada gris: periodo anterior para comparacion
  - Tooltip al hover con valor exacto y fecha

**Fila 4 - Tabla de detalle:**
- "Detalle por Categoria"
- Tabla:
  | Categoria | Costo Inventario | Dias Inventario | Capital Inmovilizado | Rotacion | Tendencia |
  - **Tendencia:** Mini sparkline por categoria

---

## FASE 7: Administracion

### Pantalla 7.1: Importacion CSV

```
Ruta: /imports
Permisos: Admin
```

**Layout multi-paso (progress bar):**

```
[1. Subir Archivo] ---- [2. Mapear Columnas] ---- [3. Validar] ---- [4. Resultado]
```

**Paso 1 - Subir Archivo:**
- Zona de drag-and-drop grande (border dashed, icono Upload grande, texto "Arrastra tu archivo CSV aqui o haz click para seleccionar")
- Debajo: Select "Tipo de datos" (Productos, Proveedores, Categorias, Historial de Ventas)
- Tabla de templates disponibles:
  | Tipo | Descripcion | Descargar |
  | Productos | SKU, nombre, categoria, costo... | [Descargar Template] |
  | Proveedores | Nombre, contacto, email, telefono... | [Descargar Template] |
  | Historial de Ventas | Fecha, SKU, cantidad, precio... | [Descargar Template] |
- Historial de importaciones previas (mini tabla):
  | Fecha | Tipo | Archivo | Registros | Resultado | Estado |

**Paso 2 - Mapear Columnas:**
- Tabla de mapeo:
  | Columna del CSV | -> | Campo del Sistema | Preview (3 valores) |
  | "cod_producto" | -> | SKU (select) | ABC-001, ABC-002, ABC-003 |
  | "nombre_prod" | -> | Nombre (select) | Pintura, Cemento, Cable |
  | "cat" | -> | Categoria (select) | Pinturas, Construccion, Elec |
  | "precio" | -> | Costo Unitario (select) | 125.00, 450.00, 89.50 |
  - Columnas no mapeadas en gris con option "Ignorar"
  - Auto-deteccion de columnas (match por nombre similar)

**Paso 3 - Validar (Preview):**
- Resumen: "2,340 registros validos | 15 con errores | 5 advertencias"
- Tabs: [Validos (2,340)] [Errores (15)] [Advertencias (5)]
- Tab Errores muestra tabla:
  | Fila | Campo | Valor | Error |
  | 45 | SKU | "" | Campo obligatorio |
  | 123 | Costo | "abc" | Formato numerico invalido |
  | 456 | SKU | "PRD-0421" | SKU ya existe en el sistema |
- Boton: "Importar 2,340 registros validos" (primary) + texto "(los registros con error seran omitidos)"

**Paso 4 - Resultado (con polling cada 3s):**
- Progress bar animada durante procesamiento
- Al completar:
  - Icono Check grande verde
  - "Importacion completada exitosamente"
  - "2,340 registros importados | 15 omitidos por errores"
  - "Tiempo de procesamiento: 18 segundos"
  - "Los buffers de los productos importados se estan recalculando..."
  - Botones: [Ver Productos Importados] [Nueva Importacion]

---

### Pantalla 7.2: Configuracion General

```
Ruta: /settings
Permisos: Admin
```

**Layout con tabs verticales (sidebar interno) o secciones:**

**Seccion "Organizacion":**
- Nombre de la organizacion (text)
- Moneda base (select: USD, MXN, EUR, etc.) - read-only post-setup
- Moneda local (select)
- Tipo de cambio (number, ej: 20.15 MXN/USD) - con fecha de actualizacion

**Seccion "Parametros DDMRP":**
- Ventana CPD (number, dias, default 7)
- Umbral de pico (number, %, default 50)
- Umbrales de Lead Time:
  | Categoria | Rango (dias) |
  | Corto | < 5 |
  | Medio | 5 - 15 |
  | Largo | > 15 |
  (editable)
- Dias de anticipacion para alertas de llegada (number, default 3)

**Seccion "Notificaciones":**
- Switches: Alertas de buffer rojo (on/off), Alertas de llegada proxima (on/off), Alertas de OC vencida (on/off)

---

### Pantalla 7.3: Gestion de Usuarios

```
Ruta: /settings/users
Permisos: Admin
```

**Lista:**
| Nombre | Email | Rol | Ultimo Acceso | Estado | Acciones |
- **Rol:** Badge: Admin (purple), Usuario (blue), Solo Lectura (slate)
- **Acciones:** Editar, Resetear contrasena, Desactivar

**Datos:**
| Juan Perez | juan@empresa.com | Admin | Hace 5 min | Activo |
| Maria Lopez | maria@empresa.com | Usuario | Hace 2h | Activo |
| Pedro Gomez | pedro@empresa.com | Usuario | Hace 1 dia | Activo |
| Ana Torres | ana@empresa.com | Solo Lectura | Hace 3 dias | Activo |
| Roberto Diaz | roberto@empresa.com | Usuario | Hace 30 dias | Inactivo |

**Formulario (modal):**
- Nombre* (text)
- Email* (email)
- Rol* (select: Admin, Usuario, Solo Lectura)
- Card explicativa del rol seleccionado:
  - Admin: "Acceso completo. Puede gestionar usuarios, importar datos, y configurar el sistema."
  - Usuario: "Puede crear y editar datos maestros, transacciones y ajustes. No puede administrar usuarios ni importar."
  - Solo Lectura: "Solo puede consultar dashboards, reportes y datos. No puede crear ni editar nada."

---

### Pantalla 7.4: Configuracion Multi-moneda

```
Ruta: /settings/currencies
Permisos: Admin
```

**Layout:**
- Card "Moneda Base": USD (read-only, configurada en setup)
- Card "Moneda Local": MXN
  - Tipo de cambio: Input numerico "20.15"
  - Fecha de actualizacion: "Actualizado: 14/02/2026"
  - Boton "Actualizar Tipo de Cambio" (primary)
- Card informativa: "Los KPIs y costos de inventario se consolidan en la moneda base (USD). Los montos se pueden visualizar en ambas monedas."

---

### REGLAS DE GENERACION PARA STITCH

1. **Cada pantalla es un archivo HTML/CSS independiente** que se puede abrir en navegador.
2. **Usar Tailwind CSS via CDN** (para que funcionen las clases sin build step).
3. **Usar Inter font de Google Fonts.**
4. **Los datos son estaticos** (hardcoded en el HTML). No se necesita JavaScript funcional, solo para interacciones visuales basicas si Stitch lo soporta (hover states, collapses).
5. **Los graficos pueden ser SVG inline o placeholder con dimensiones y descripcion** de lo que deberian mostrar. Si Stitch puede generar charts simples, usar SVG para las barras de buffer y los donut charts. Para graficos complejos (area charts de tendencia), usar un placeholder con border dashed y texto descriptivo del grafico esperado, o una aproximacion en SVG.
6. **Cada pantalla debe incluir el shell completo** (sidebar + header), no solo el contenido.
7. **Responsive SI es prioridad** - optimizar para 1440px de ancho.
8. **Los colores del semaforo son EXACTOS** - usar los codigos hex definidos en el sistema de diseno.
9. **Mantener consistencia absoluta** entre pantallas: mismo sidebar, mismo header, mismos estilos de tablas, mismos botones.
10. **Nombrar los archivos** segun la ruta: `login.html`, `dashboard.html`, `products-list.html`, `products-new.html`, `products-detail.html`, etc.

---

### ORDEN DE EJECUCION

Genera las pantallas en este orden, verificando consistencia visual con cada pantalla previa:

| # | Archivo | Pantalla | Fase |
|---|---------|----------|------|
| 1 | `login.html` | Login | 1 |
| 2 | `forgot-password.html` | Recuperar Contrasena | 1 |
| 3 | `reset-password.html` | Restablecer Contrasena | 1 |
| 4 | `shell.html` | Layout Shell (sidebar + header) | 1 |
| 5 | `dashboard.html` | Dashboard Principal (con semaforo, KPIs, tabla criticos, tendencia) | 2 |
| 6 | `products-detail.html` | Detalle de Producto (con todas las tabs, especialmente Buffer & Zonas) | 2 |
| 7 | `suppliers-list.html` | Lista de Proveedores | 3 |
| 8 | `suppliers-form.html` | Formulario Proveedor | 3 |
| 9 | `suppliers-detail.html` | Detalle de Proveedor | 3 |
| 10 | `categories.html` | Arbol de Categorias | 3 |
| 11 | `products-list.html` | Lista de Productos | 3 |
| 12 | `products-new.html` | Crear Producto (Wizard 4 pasos - mostrar Paso 1 activo) | 3 |
| 13 | `products-new-step2.html` | Crear Producto - Paso 2 Proveedores | 3 |
| 14 | `products-new-step3.html` | Crear Producto - Paso 3 Buffer | 3 |
| 15 | `products-new-step4.html` | Crear Producto - Paso 4 Revision | 3 |
| 16 | `purchase-orders-list.html` | Lista de OC | 4 |
| 17 | `purchase-orders-new.html` | Crear OC | 4 |
| 18 | `purchase-orders-detail.html` | Detalle OC (con recepcion) | 4 |
| 19 | `sales-orders-list.html` | Lista de OV | 4 |
| 20 | `sales-orders-new.html` | Crear OV | 4 |
| 21 | `sales-orders-detail.html` | Detalle OV | 4 |
| 22 | `adjustments-list.html` | Lista de Ajustes | 4 |
| 23 | `adjustments-new.html` | Registrar Ajuste | 4 |
| 24 | `planning.html` | Planificacion de Reposiciones | 5 |
| 25 | `planning-calendar.html` | Calendario de Llegadas | 5 |
| 26 | `dynamic-adjustments.html` | Ajustes Dinamicos | 5 |
| 27 | `alerts.html` | Centro de Alertas | 6 |
| 28 | `kpis.html` | Dashboard de KPIs | 6 |
| 29 | `imports.html` | Importacion CSV (Paso 1) | 7 |
| 30 | `imports-mapping.html` | Importacion CSV (Paso 2 - Mapeo) | 7 |
| 31 | `imports-preview.html` | Importacion CSV (Paso 3 - Validacion) | 7 |
| 32 | `imports-result.html` | Importacion CSV (Paso 4 - Resultado) | 7 |
| 33 | `settings.html` | Configuracion General | 7 |
| 34 | `settings-users.html` | Gestion de Usuarios | 7 |
| 35 | `settings-currencies.html` | Configuracion Multi-moneda | 7 |

**Total: 35 pantallas.**

---

### INSTRUCCION FINAL

Genera cada pantalla usando Stitch, asegurandote de que:

1. **Todas compartan el mismo shell** (sidebar + header) con el item de navegacion activo correspondiente.
2. **Los datos de ejemplo sean realistas** y consistentes entre pantallas (el mismo producto PRD-0421 aparece en Dashboard, Detalle, Planificacion, Alertas).
3. **Las barras de buffer sean visualmente identicas** en todas las pantallas donde aparecen (Dashboard, Products List, Product Detail, Planning).
4. **El donut chart del semaforo** muestre proporciones reales (68% verde, 22% amarillo, 10% rojo).
5. **Los colores del semaforo** sigan la paleta definida exacta (no valores aproximados).
6. **La tipografia** use Inter en todos los tamanos especificados.
7. **Los estados vacios** (empty states) esten contemplados donde aplique (ej: tabla sin resultados muestra icono + "No se encontraron resultados" + CTA).

Comienza con la Fase 1 (Login + Shell) y avanza secuencialmente. Al completar cada fase, verifica que las nuevas pantallas sean visualmente coherentes con las anteriores y espera aprobacion mia antes de continuar, asi puedo validar lo creado.

---

_FIN DEL PROMPT_
