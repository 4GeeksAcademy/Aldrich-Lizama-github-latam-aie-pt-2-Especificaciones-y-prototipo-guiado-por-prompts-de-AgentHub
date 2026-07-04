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
- HTML5 semantico.
- Tailwind CSS via CDN, usando variantes dark: para tema oscuro.
- CSS adicional en styles.css para identidad visual y detalles no triviales.
- JavaScript vanilla en app.js para render e interacciones.
- Todos los datos se cargan desde estructuras JavaScript locales.

## 4. Arquitectura de interfaz
### 4.1 Layout general
- Sidebar fija en escritorio y desplegable en movil.
- Header superior con:
  - Titulo contextual de seccion activa.
  - Boton de menu movil.
  - Toggle de tema claro/oscuro.
- Area de contenido con una sola seccion visible a la vez.

### 4.2 Navegacion
- La sidebar contiene 6 items, uno por seccion.
- Al hacer clic en un item:
  - Se marca como activo.
  - Se actualiza el titulo del header.
  - Se muestra la seccion correspondiente.
  - Se ocultan las otras secciones.

## 5. Requisitos por seccion
### 5.1 Dashboard
Debe mostrar:
- Tarjeta 1: ingresos totales del mes.
- Tarjeta 2: perdida total por descuentos y cupones.
- Tarjeta 3: cantidad de agentes activos.
- Tarjeta 4: cantidad de agentes fallando.
- Debajo, un area placeholder para grafico de actividad semanal.

### 5.2 Gestion de usuarios
- Tabla con columnas: Nombre, Email, Plan, Estado, Acciones.
- Cada fila incluye dropdown con opciones:
  - Ver detalle.
  - Eliminar.
- Ver detalle abre modal overlay con informacion completa del usuario.
- El modal cierra con boton y clic en backdrop.

### 5.3 Gestion de agentes
- Lista o tabla con: Nombre del agente, Propietario, Estado, Skills, Acciones.
- Estado soportado: activo, inactivo, fallando.
- Skills colapsadas por defecto.
- Control expandible para revelar skills con transicion suave.
- Dropdown por fila con:
  - Configurar (abre modal con prompt de sistema).
  - Eliminar.

### 5.4 Skills
- Catalogo de skills con:
  - Nombre.
  - Descripcion breve.
  - Cantidad de agentes que la usan.
- Incluir texto explicativo sobre que es una skill en AgentHub.
- Dropdown por skill con:
  - Ver detalle.
  - Eliminar.

### 5.5 Contrataciones de agentes
- Tabla con:
  - Cliente.
  - Agente alquilado.
  - Skills contratadas.
  - Fechas del contrato.
  - Importe total pagado.
  - Acciones.
- Dropdown por fila con Ver detalle.
- Ver detalle abre modal con desglose completo:
  - Datos generales del contrato.
  - Lista de skills y precio individual.

### 5.6 Log de errores
- Tabla o lista con:
  - Timestamp.
  - Nombre del agente.
  - Tipo de error.
  - Descripcion breve.
  - Acciones.
- Badges de color por severidad o tipo.
- Dropdown por entrada con:
  - Ver detalle (modal con traza completa).
  - Marcar como resuelto.

## 6. Reglas de comportamiento
### 6.1 Toggle de tema
- El boton de tema alterna clase dark en el elemento raiz.
- Todo el estilo oscuro debe depender de clases dark: de Tailwind.
- El estado del tema se persiste en localStorage.

### 6.2 Dropdowns
- Solo un dropdown abierto simultaneamente.
- Cierre automatico al hacer clic fuera o seleccionar accion.

### 6.3 Modales
- Modal reutilizable para usuarios, agentes, contratos y errores.
- Debe abrir centrado con backdrop.
- Cierra con:
  - Boton de cierre.
  - Clic sobre backdrop.
  - Tecla Escape.

### 6.4 Expandibles de skills
- Inician colapsados.
- Se expanden con animacion de altura y opacidad.

### 6.5 Responsive
- Mobile first.
- Sidebar colapsable bajo 1024px.
- Tablas deben ser navegables horizontalmente en pantallas pequenas.

## 7. Modelo de datos hardcodeado
### 7.1 Usuarios
Campos: id, nombre, email, plan, estado, empresa, fechaAlta, ultimoAcceso.

### 7.2 Agentes
Campos: id, nombre, propietario, estado, skills[], promptSistema.

### 7.3 Skills
Campos: id, nombre, descripcion, agentesActivos, detalle.

### 7.4 Contratos
Campos: id, cliente, agente, skillsContratadas[{nombre, precio}], fechaInicio, fechaFin, total.

### 7.5 Errores
Campos: id, timestamp, agente, severidad, tipo, descripcion, traza.

### 7.6 Dashboard
Campos agregados: ingresosMes, perdidasDescuentos, agentesActivos, agentesFallando, actividadSemanal[] (placeholder visual).

## 8. Criterios de aceptacion
- Existen las 6 secciones accesibles desde sidebar.
- Toggle claro/oscuro funcional usando clases dark:.
- Dashboard con 4 metricas y placeholder de grafico semanal.
- Usuarios, agentes, skills, contratos y errores con dropdown de acciones.
- Modales funcionales para detalle solicitado en cada modulo.
- Skills de agentes expandibles con transicion.
- Datos totalmente hardcodeados.
- Prototipo usable y visualmente profesional en desktop y mobile.

## 9. Entregables
- SPECIFICACION.md con este documento.
- index.html con estructura del panel.
- styles.css con identidad visual y refinamientos.
- app.js con datos hardcodeados e interacciones.
