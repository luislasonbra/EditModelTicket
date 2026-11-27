# Editor de Ticket

Aplicación web de un solo archivo (`editor-ticket.html`) en HTML, CSS y JavaScript vanilla, sin dependencias ni compilación. Genera un ticket de lotería en SVG con tres secciones editables: **Fecha**, **Sorteos** y **Apuestas**, y permite imprimirlo varias veces por hoja.

## Qué puede hacer

### Perfiles
- Al abrir la página hay que **cargar un perfil o crear uno nuevo** (el último usado aparece primero con ★).
- Al crear un perfil (**Nuevo** o desde la pantalla de inicio) eliges **Empezar desde**:
  - **Plantilla** (por defecto): copia la plantilla actual, que incluye grupos, sorteos, diseño y ajustes de impresión.
  - **Cero**: perfil vacío, sin grupos ni sorteos; solo quedan los valores base de diseño e impresión.
- **★ Plantilla:** usa el perfil activo como plantilla para los perfiles nuevos, o restablece la plantilla predeterminada (la que genera el código).
- Los cambios se guardan automáticamente en el perfil activo (`localStorage` del navegador), incluidos los ajustes de impresión. También existe el botón **Guardar**.
- Desde el panel **Perfil**: cambiar de perfil, **Nuevo**, **Duplicar**, **Renombrar**, **Eliminar**, **JSON** y **★ Plantilla**.
- **JSON** exporta el perfil como texto (para respaldo; incluye la configuración de impresión) e importa un JSON pegado como perfil nuevo.

### Perfil predeterminado "prueba"
- La primera vez que se abre la página en un navegador se crea automáticamente el perfil **prueba**, con un solo grupo (**New York**) y un solo sorteo (**NYS**), marcado.
- Se crea **una sola vez**: si lo eliminas, no vuelve a aparecer. Si ya tenías un perfil con ese nombre, no se toca.
- Para usarlo como base de los perfiles nuevos, cárgalo y pulsa **★ Plantilla → Usar como plantilla**.
- Se define en el código, en la constante `DEFAULT_PROFILES` (junto a la carga de perfiles); ahí puedes agregar más perfiles predeterminados con `{name:'...',groups:[{name:'...',items:['...']}]}`.

### Posición y tamaño del panel
- El botón de la esquina superior del panel (**Derecha ➡ / ⬅ Izquierda**) mueve el panel de herramientas a un lado u otro.
- Arrastra la barra divisoria entre el panel y el ticket para **cambiar el ancho del panel** (doble clic: vuelve a 380 px; también funciona con las flechas ← → al enfocar la barra). Mínimo 260 px.
- La posición, el ancho y la pestaña activa se guardan en el navegador (clave `ticketUI`) y valen para todos los perfiles. En pantallas angostas (móvil) el panel va arriba y estos controles se ocultan.

### Secciones
- Arrastra **Fecha / Sorteos / Apuestas** (asa ⠿) para cambiar el orden.
- El ojo 👁️ muestra u oculta cada sección.
- Cada rectángulo se ajusta solo a su contenido y el alto total del ticket se recalcula.

### Sorteos y grupos
- Los sorteos se organizan en **grupos** (New York, New Jersey, etc.).
- Dentro de un grupo **no se permiten sorteos repetidos** (sin distinguir mayúsculas).
- Cada sorteo tiene una casilla: solo los **marcados** salen en el ticket.
- Cada grupo tiene:
  - 👁️ / 🚫 para mostrar u ocultar el grupo completo en el ticket (conserva sus marcas).
  - ☑ / ☐ para marcar o desmarcar todos sus sorteos.
- Los **grupos predeterminados** no se pueden **renombrar** (nombre bloqueado); sí puedes mostrarlos, ocultarlos, eliminarlos y marcar o agregar sus sorteos.
- **Grupos:** se pueden eliminar con ✕ (pide confirmación), también los predeterminados. **↺ Restaurar grupos predeterminados** vuelve a agregar los que falten (ocultos). Ver las variables `ALLOW_DELETE_GROUPS` y `ALLOW_DELETE_ITEMS` más abajo.
- **Sorteos dentro de un grupo:** se pueden eliminar **uno por uno** con el ✕ de cada sorteo (variable `ALLOW_DELETE_ITEMS`).
- Cada sorteo lleva su casilla en la misma línea. Se reparten en columnas según **Filas por columna**; con 16 sorteos y 8 filas salen 2 columnas.
- Se puede reordenar arrastrando ⠿ (grupos y sorteos dentro de su grupo).

### Apuestas, Fecha y Papel
- **Apuestas:** títulos, número de líneas, alto de línea y de encabezado, posición del divisor y tamaño de letra. En la plantilla predeterminada las dos columnas se llaman **APUESTA** y el divisor está al **50 %** (cada columna ocupa la mitad). Esto solo afecta a perfiles nuevos creados desde la plantilla predeterminada; los perfiles existentes y las plantillas personalizadas (★) conservan sus valores.
- **Fecha:** etiqueta, formato, alto y tamaño de letra.
- **Papel:** ancho, margen, espacio entre secciones, radio de esquinas y grosor de borde.

### Imprimir (pestaña independiente)
Pestaña **🖨️ Imprimir** del panel: acomoda varias copias del ticket en una hoja y las manda a la impresora. Muestra una vista previa de las hojas (hasta las 3 primeras) y se actualiza sola al cambiar el diseño del ticket.

- **Tamaño de hoja:** Carta / Letter, Legal, Oficio México, Media carta / Statement, A4, A5, A3, Tabloide / Ledger o **Personalizado** (ancho × alto en mm).
- **Orientación:** Vertical u Horizontal.
- **Márgenes independientes:** *izquierdo/derecho* y *arriba/abajo*, medidos sobre la hoja ya orientada: en horizontal, "arriba/abajo" son los lados cortos, así que cambiarlos nunca altera el otro eje.
- **Espacio entre columnas** y **entre filas** por separado, y **posición en la hoja** (arriba o centrada).
- **Modo Automático:** indicas el **ancho del ticket en mm** (el alto sale de su proporción) y se calcula cuántas columnas y filas caben. Si el ticket no cabe a lo ancho o a lo alto, se reduce y el panel lo avisa.
- **Modo Manual:** indicas **columnas × filas** y el ticket se escala para encajar en cada celda sin deformarse.
- **Cantidad:** `0` llena una hoja completa; un número mayor imprime esa cantidad y agrega las hojas necesarias.
- **Líneas de corte:** guías punteadas entre tickets (opcional).
- Botón **🖨️ Imprimir**: abre el diálogo de impresión del navegador solo con las hojas. En el diálogo desactiva *encabezados y pies de página* y usa escala **100 %**.
- Los ajustes de impresión **se guardan dentro de cada perfil** (también en el JSON exportado y en la plantilla). Los perfiles anteriores migran sus ajustes la primera vez que se cargan.

### Exportar el ticket
- **Descargar SVG / PNG** y **Ver código SVG** (para copiar el SVG; vuelve a pulsar el botón para ocultarlo).
- En una página publicada las descargas pueden estar bloqueadas; usa **Ver código SVG**. Abierto como archivo local, las descargas funcionan normalmente.

## Permitir o bloquear la eliminación de grupos y sorteos

Justo debajo de `DEFAULT_GROUPS` hay dos variables:

```js
const ALLOW_DELETE_GROUPS=true;   // true: se pueden eliminar grupos
const ALLOW_DELETE_ITEMS=true;    // true: se pueden eliminar sorteos uno por uno
```

- `ALLOW_DELETE_GROUPS=true` (por defecto): aparece ✕ en cada grupo. Con `false` se muestra 🔒 en su lugar.
- `ALLOW_DELETE_ITEMS=true` (por defecto): aparece un ✕ en cada sorteo para quitarlo individualmente. Con `false` se oculta y los sorteos no se pueden eliminar.
- Un grupo predeterminado eliminado **no reaparece** al recargar; se recupera con **↺ Restaurar grupos predeterminados**.
- Se puede seguir agregando, renombrando, marcando, ocultando y reordenando. Esto no afecta la eliminación de **perfiles**.

## Cómo agregar grupos de sorteos predeterminados

1. Abre `editor-ticket.html` en tu editor de código.
2. Busca el bloque marcado con el comentario `GRUPOS PREDETERMINADOS` al inicio del `<script>`:

```js
const DEFAULT_GROUPS=[
  {name:'New York',items:['MID - DAY','NY','NYS','BK MID DAY','BK-NIGHT']},
  {name:'New Jersey',items:['NJ DAY','NJ-NIGHT']},
  // ...
];
```

3. Agrega un objeto por cada grupo nuevo, con `name` (nombre del grupo) e `items` (lista de sorteos sin repetir):

```js
  {name:'Texas',items:['TX MORNING','TX DAY','TX EVENING','TX NIGHT']},
```

4. Guarda y recarga la página.

### Cómo se aplican a los perfiles
- **Perfiles nuevos desde plantilla:** reciben los grupos de la plantilla (por defecto, todos los predeterminados visibles y con todos sus sorteos marcados). **Desde cero:** no reciben ninguno.
- **Perfiles ya existentes:** los grupos predeterminados que falten (y que no hayas eliminado) se añaden **ocultos** (🚫), para no alterar tickets ya diseñados. Actívalos con 👁️.
- Los sorteos de un grupo predeterminado que ya existe en un perfil **no se vuelven a sincronizar**: si agregas sorteos nuevos a un grupo en `DEFAULT_GROUPS`, agrégalos también a mano en ese grupo dentro de los perfiles existentes, o crea un perfil nuevo.
- Para que un grupo se considere predeterminado, el nombre en `DEFAULT_GROUPS` debe coincidir con el del grupo (sin importar mayúsculas).
- Si quitas un grupo de `DEFAULT_GROUPS`, deja de ser predeterminado y se trata como un grupo normal (ya se puede renombrar).

## Datos guardados
Todo vive en `localStorage`, solo en ese navegador:
- `ticketProfiles`: perfiles (diseño, grupos, sorteos, ajustes de impresión), último perfil usado y cuáles perfiles predeterminados ya se crearon.
- `ticketTemplate`: plantilla personalizada para perfiles nuevos (opcional).
- `ticketUI`: lado y ancho del panel y pestaña activa.

Para llevar un perfil a otro equipo usa **JSON → copiar** e impórtalo allá (la plantilla y las preferencias de interfaz no viajan en el JSON).
