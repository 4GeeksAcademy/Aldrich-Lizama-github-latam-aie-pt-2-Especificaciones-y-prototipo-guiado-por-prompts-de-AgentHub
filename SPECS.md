# Especificacion funcional y tecnica del panel de administracion AgentHub

## 1. Objetivo
Construir un prototipo frontend en HTML, CSS y JavaScript del panel de administracion interno de AgentHub, con datos hardcodeados y sin integracion de backend.

## 2. Alcance
Incluye un layout principal con:
- Navegacion lateral persistente.
- Barra superior con toggle de modo claro/oscuro.
- Seis secciones funcionales: Dashboard, Gestion de usuarios, Gestion de agentes, Skills, Contrataciones de agentes, Log de errores.
- Interacciones UI completas: dropdowns, modales, expandibles y badges de severidad.

No incluye:
- Consumo de API.
- Autenticacion.
- Persistencia de datos.

## 3. Tecnologias y convenciones
- HTML5 semantico (`section`, `table`, `nav`, `header`, `main`, `footer`, `aside`).
- Tailwind CSS via CDN (v3.4+), usando variantes `dark:` para tema oscuro y clases utilitarias exclusivamente.
- Sin estilos en linea ni archivos CSS externos.
- JavaScript vanilla en bloque `<script>` al final del HTML para datos, renderizado e interacciones.
- Todos los datos se cargan desde estructuras JavaScript locales (objeto `DATA`).

## 4. Arquitectura de interfaz
### 4.1 Layout general
- Sidebar fija (`fixed`) en escritorio (`lg:translate-x-0`) y desplegable con overlay en movil (`-translate-x-full`).
- Header superior sticky con:
  - Titulo contextual de seccion activa.
  - Boton de menu movil (solo visible en `<lg`).
  - Toggle de tema claro/oscuro.
- Area de contenido con una sola seccion visible a la vez (clases `hidden`/`block`).
- Footer con estado del sistema y enlaces.

### 4.2 Navegacion
- La sidebar contiene 6 items con iconos Material Symbols, uno por seccion.
- Al hacer clic en un item:
  - Se marca como activo (fondo `bg-surface-container`, texto bold).
  - Se actualiza el titulo del header mobile.
  - Se muestra la seccion correspondiente via `navigateTo()`.
  - Se ocultan las otras secciones.
  - Se cierran dropdowns abiertos y el sidebar mobile.

## 5. Requisitos por seccion
### 5.1 Dashboard
Debe mostrar:
- Tarjeta 1 (payments): ingresos totales del mes con indicador de tendencia.
- Tarjeta 2 (price_check): perdida total por descuentos y cupones.
- Tarjeta 3 (smart_toy): cantidad de agentes activos.
- Tarjeta 4 (dangerous): cantidad de agentes fallando.
- Area placeholder para grafico de actividad semanal (barras por dia con dos tonalidades).
- Panel lateral "Estado Reciente" con 4 agentes de ejemplo y sus estados.
- Botones de filtro "7 Dias" / "30 Dias" en el area de grafico.

### 5.2 Gestion de usuarios
- Tabla con columnas: Nombre (con avatar de iniciales), Email, Plan, Estado, Acciones.
- 5 usuarios hardcodeados con nombres, empresas reales y planes variados (Enterprise, Pro, Free).
- Cada fila incluye dropdown (`more_vert`) con opciones:
  - "Ver detalle" → abre modal overlay con informacion completa del usuario.
  - "Eliminar" (placeholder visual).
- Modal de detalle muestra: empresa, email, plan, estado, fecha de alta, ultimo acceso y grafico de actividad mensual placeholder.

### 5.3 Gestion de agentes
- Tabla con columnas: Nombre del agente (con avatar + ID), Propietario, Estado, Skills, Acciones.
- 8 agentes hardcodeados con estados variados: activo, inactivo, fallando.
- Skills colapsadas por defecto: solo se muestran las primeras 2, el resto ocultas con transicion `max-h-0`/`max-h-52`.
- Control expandible con texto "+N mas" que al hacer clic revela las restantes con transicion suave de altura y opacidad.
- Dropdown por fila con:
  - "Configurar" → abre modal con textarea del system prompt, slider de temperatura y selector de modelo LLM.
  - "Eliminar" (placeholder visual).
- Modal de configuracion incluye boton "Guardar cambios" visible que actualiza el prompt en memoria.

### 5.4 Skills
- Catalogo en grid responsive (1 col movil, 2 tablet, 3 desktop) con tarjetas.
- Cada tarjeta contiene: icono, nombre, descripcion (max 2 lineas), contador de agentes activos y estado.
- Incluir texto explicativo sobre que es una skill en AgentHub.
- Dropdown por tarjeta con:
  - "Ver detalle" → modal con descripcion completa, agentes activos y detalle tecnico.
  - "Eliminar" (placeholder visual).
- 8 skills hardcodeadas con descripciones y detalles tecnicos realistas.

### 5.5 Contrataciones de agentes
- Tabla con columnas: Cliente (con iniciales), Agente, Skills contratadas, Fechas, Importe, Acciones.
- 5 contratos hardcodeados con clientes empresariales realistas.
- Skills contratadas muestran max 2 + badge "+N" si hay mas.
- Dropdown por fila con "Ver detalle".
- Modal de detalle con desglose completo:
  - Datos generales del contrato (cliente, periodo).
  - Lista de skills con precio individual y check icon.
  - Importe total destacado con fondo de color primario.

### 5.6 Log de errores
- Tabla con columnas: Timestamp (con ID de error), Agente/Entidad (con tipo), Severidad, Descripcion, Acciones.
- 6 errores hardcodeados con severidades: Critico, Advertencia e Inactivo.
- Badges de color por severidad con indicador de punto de color (rojo para critico, amarillo para advertencia, gris para inactivo).
- Dropdown por fila con:
  - "Ver detalle" → modal con timestamp, severidad, agente, tipo, descripcion y traza completa en bloque de codigo.
  - "Marcar como resuelto" (placeholder visual).

## 6. Reglas de comportamiento
### 6.1 Toggle de tema
- El boton de tema alterna clase `dark` en el elemento raiz `<html>`.
- Todo el estilo oscuro debe depender exclusivamente de clases `dark:` de Tailwind.
- El estado del tema se persiste en `localStorage` bajo la clave `theme`.
- Al cargar la pagina, se restaura el tema guardado.

### 6.2 Dropdowns
- Solo un dropdown abierto simultaneamente (se cierran al abrir otro).
- Cierre automatico al hacer clic fuera del dropdown y del boton que lo activa.
- Cierre al seleccionar una accion del dropdown.
- Implementados con clase `hidden` toggle y selectores `[id^="udrop-"], [id^="adrop-"], [id^="sdrop-"], [id^="cdrop-"], [id^="edrop-"]`.

### 6.3 Modales
- Modal reutilizable unico para usuarios, agentes, skills, contratos y errores.
- Debe abrir centrado con backdrop semitransparente y efecto blur.
- Cierra con 3 mecanismos:
  - Boton de cierre (X).
  - Clic sobre backdrop.
  - Tecla Escape.
- Animacion de entrada: scale de 0.95→1.0 y opacidad 0→1 con duracion 300ms.
- Animacion de salida: inversa antes de ocultar.

### 6.4 Expandibles de skills
- Inician colapsados con `max-h-0` y `opacity-0`.
- Al hacer clic en "+N mas", se expanden con `max-h-52`, `opacity-100` y margen.
- Transicion CSS: `transition-all duration-300 ease-out`.
- Al colapsar, vuelven suavemente a `max-h-0 opacity-0`.

### 6.5 Responsive
- Mobile first con punto de quiebre en `lg` (1024px).
- Sidebar colapsable bajo 1024px con overlay oscuro.
- Tablas con `overflow-x-auto` para scroll horizontal en pantallas pequenas.
- Grids adaptativos con `grid-cols-1 sm:grid-cols-2 lg:grid-cols-4`.

## 7. Inventario de componentes

### 7.1 Componentes de layout
| Componente | Tag/elemento | Descripcion |
|---|---|---|
| Sidebar | `<aside>` | Navegacion fija a la izquierda, 6 links de seccion |
| Header | `<header>` | Barra superior sticky con titulo, busqueda, notificaciones y toggle |
| Main content | `<main>` | Area de contenido principal con max-width |
| Footer | `<footer>` | Barra inferior con copyright y estado del sistema |
| Sidebar overlay | `<div>` | Overlay semitransparente para sidebar en movil |

### 7.2 Componentes de UI
| Componente | Descripcion |
|---|---|
| Metric card | Tarjeta con icono, etiqueta, valor numerico y badge de tendencia |
| Bar chart | Grafico de barras semanal con dos colores por barra |
| Status badge | Badge con punto de color segun estado (activo/fallando/inactivo/pendiente) |
| Plan badge | Badge con color segun plan (Enterprise/Pro/Free) |
| Dropdown menu | Menu contextual con opciones de accion, toggle con `more_vert` |
| Modal | Overlay centrado con backdrop, header, body scrolleable y footer |
| Skills expandable | Contenedor colapsable con transicion de altura y opacidad |
| Theme toggle | Boton que alterna entre iconos `dark_mode`/`light_mode` |
| Search input | Input con icono de busqueda en header (solo visual) |
| Notification bell | Icono de notificaciones con punto rojo de alerta |

### 7.3 Componentes de datos
| Seccion | Tipo de listado | Filas/items |
|---|---|---|
| Dashboard | 4 metric cards + chart + status panel | N/A |
| Usuarios | Tabla (`<table>`) | 5 filas |
| Agentes | Tabla (`<table>`) | 8 filas |
| Skills | Grid de tarjetas | 8 tarjetas |
| Contrataciones | Tabla (`<table>`) | 5 filas |
| Errores | Tabla (`<table>`) | 6 filas |

## 8. Modelo de datos hardcodeado
### 8.1 Usuarios
Campos: id, nombre, email, plan, estado, empresa, fechaAlta, ultimoAcceso.

### 8.2 Agentes
Campos: id, nombre, propietario, estado, skills[], promptSistema.

### 8.3 Skills
Campos: id, nombre, descripcion, agentesActivos, detalle.

### 8.4 Contratos
Campos: id, cliente, agente, skillsContratadas[{nombre, precio}], fechaInicio, fechaFin, total.

### 8.5 Errores
Campos: id, timestamp, agente, severidad, tipo, descripcion, traza.

### 8.6 Dashboard
Campos agregados: ingresosMes, perdidasDescuentos, agentesActivos, agentesFallando, actividadSemanal[] (placeholder visual).

## 9. Criterios de aceptacion
1. SPECS.md fue commiteado antes de cualquier archivo HTML (verificado mediante el historial de Git).
2. La spec contiene al menos 3 especificaciones concretas por seccion, un inventario de componentes y una lista numerada de criterios de aceptacion.
3. Las seis secciones del panel (Dashboard, Gestion de usuarios, Gestion de agentes, Skills, Contrataciones, Log de errores) estan presentes y son accesibles desde la barra lateral.
4. Las clases utilitarias de Tailwind se usan de forma consistente en todo el proyecto – sin estilos en linea, sin archivos CSS externos.
5. Todas las filas de los listados tienen un dropdown ⁝ funcional que se abre, se cierra al hacer clic y se cierra al hacer clic fuera.
6. "Ver detalle" abre un modal en al menos cuatro secciones diferentes (usuarios, agentes, skills, contratos, errores).
7. Los modales se cierran con el boton de cierre y con clic en el backdrop.
8. Las listas de skills de los agentes estan colapsadas por defecto y se expanden/colapsan al hacer clic con una transicion visible.
9. El toggle de modo oscuro/claro cambia todo el panel y el estado se mantiene entre secciones.
10. Los datos hardcodeados son consistentes entre secciones (el mismo nombre de agente aparece en Gestion de agentes, Contrataciones y Log de errores).
11. El HTML usa etiquetas semanticas correctamente – `section`, `table`, `nav`, `header`, `main` y similares.
12. El layout es usable en viewports de escritorio y tablet.

## 10. Entregables
- SPECS.md con este documento.
- index.html con estructura del panel, datos hardcodeados e interacciones en un unico archivo.
