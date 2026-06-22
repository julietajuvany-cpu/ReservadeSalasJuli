# 📅 Sistema de Reserva de Salas - Onboarding

## 🚀 Inicio Rápido

**Proyecto:** Sistema de reserva de salas multi-usuario con disponibilidad en tiempo real
**Stack:** HTML/CSS/JS vanilla + Supabase (base de datos)
**Deploy:** Netlify (automático al hacer push a GitHub)

---

## 📂 Estructura del Proyecto

```
/calendario-humand/
├── index.html          ← ÚNICO ARCHIVO (todo en uno: HTML + CSS + JS)
├── netlify.toml        ← Config de Netlify
├── _redirects          ← Redirects para servir index.html
└── ONBOARDING.md       ← Esta guía
```

**No hay carpetas, no hay build process.** Todo está en `index.html`.

---

## 🔧 Antes de Empezar

### Acceso a Supabase
- **URL:** https://supabase.com/dashboard
- **Proyecto:** `calendario-salas`
- **Tabla:** `reservations` con columnas:
  - `id` (texto) - Identificador único
  - `room_id` (texto) - room-a, room-b, room-c
  - `date` (texto) - Formato YYYY-MM-DD
  - `start_time` (texto) - Formato HH:MM (08:00-18:00)
  - `end_time` (texto) - Formato HH:MM
  - `name` (texto) - Nombre del evento
  - `requester` (texto) - Quién reserva
  - `notes` (texto) - Notas adicionales

### Credenciales Supabase (YA CONFIGURADAS)
```javascript
const SUPABASE_URL = 'https://qiwmuqeunuumtdzqryui.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InFpd211cWV1bnV1bXRkenFyeXVpIiwicm9sZSI6ImFub24iLCJpYXQiOjE3ODAzMzIxOTksImV4cCI6MjA5NTkwODE5OX0.b064_zxooRgqc_jllML8VDfeun22tYRDr0fJZn4ZpNU';
```

---

## 📋 Los 4 Modos del Sistema

### ADMIN (crear/eliminar reservas)
```
https://lighthearted-tarsier-f2e94e.netlify.app/?mode=admin
```
- Selector de salas
- Formulario para crear reservas
- Botón eliminar en eventos
- Puede ver/editar todas las salas

### USUARIO - Sala A
```
https://lighthearted-tarsier-f2e94e.netlify.app/?mode=view&sala=A
```
- Solo lectura (no puede crear/editar)
- Ve solo reservas de Sala A
- Muestra disponibilidad en tiempo real

### USUARIO - Sala B
```
https://lighthearted-tarsier-f2e94e.netlify.app/?mode=view&sala=B
```

### USUARIO - Sala C
```
https://lighthearted-tarsier-f2e94e.netlify.app/?mode=view&sala=C
```

---

## 🔑 Variables Clave en el Código

```javascript
// Salas disponibles (línea ~226)
const ROOMS = [
  { id: 'room-a', name: 'Sala 1 - 3er Piso', color: '#496be3' },
  { id: 'room-b', name: 'Sala 2 - 3er Piso', color: '#28c040' },
  { id: 'room-c', name: 'Sala 4to Piso', color: '#d42e2e' },
];

// Estado global (línea ~241)
let state = {
  mode: 'view',              // 'view' o 'admin'
  currentRoom: 'room-a',     // Sala seleccionada
  currentDate: new Date(),   // Fecha actual
  view: 'month',             // 'month', 'week', 'day'
  selectedReservation: null,
  currentEventInModal: null,
};

// Cliente Supabase (inicializa en init())
window.supabaseClient = null;  // Se crea en init()
```

---

## 🛠️ Cómo Hacer Cambios

### 1. Clonar el Repo
```bash
git clone https://github.com/federicacorrea-ctrl/ReservadeSalas.git
cd ReservadeSalas/calendario-humand
```

### 2. Editar `index.html`
- Abre el archivo con cualquier editor (VS Code, Sublime, etc.)
- Busca la sección que quieres cambiar:
  - **HTML:** Líneas 110-222
  - **CSS:** Líneas 9-107
  - **JS:** Líneas 224-765

### 3. Probar Localmente
```bash
# Opción 1: Python (si lo tienes)
python3 -m http.server 8000

# Opción 2: Node.js
npx http-server

# Opción 3: Usar Live Server (extensión VS Code)
```
Luego abre: `http://localhost:8000/?mode=admin`

### 4. Hacer Commit y Push
```bash
git add index.html
git commit -m "feat: Descripción del cambio"
git push origin main
```
**Netlify deployará automáticamente en 1-2 minutos.**

---

## 📝 Funciones Principales (JS)

### Obtener Reservas de Supabase
```javascript
async function getReservations(roomId) {
  // Devuelve array de reservaciones para una sala
}
```

### Crear Nueva Reserva
```javascript
async function addReservation(reservation) {
  // {id, date, start_time, end_time, name, requester, notes, roomId}
}
```

### Eliminar Reserva
```javascript
async function deleteReservation(reservationId, roomId) {
  // Elimina por ID
}
```

### Renderizar Calendario
```javascript
async function renderCalendar() {
  // Renderiza según state.view ('month', 'week', 'day')
}
```

### Detectar Conflictos
```javascript
async function hasConflict(roomId, dateStr, startTime, endTime) {
  // true si hay solapamiento de horarios
}
```

---

## 🎨 Modificaciones Comunes

### Cambiar Nombre de una Sala
Línea ~226, busca:
```javascript
{ id: 'room-a', name: 'Sala 1 - 3er Piso', color: '#496be3' },
```
Cambia `name` a lo que quieras.

### Cambiar Horarios Permitidos
Línea ~154 y ~159, busca:
```javascript
<input type="time" id="booking-start" ... min="08:00" max="17:00" ...>
<input type="time" id="booking-end" ... min="08:30" max="18:00" ...>
```
Modifica `min` y `max`.

### Cambiar Colores de Salas
Línea ~226, modifica el `color` de cada sala (formato hex #RRGGBB).

### Agregar Más Salas
1. Agrega a `ROOMS` (línea ~226)
2. Crea datos de ejemplo en `initSampleData()` (línea ~720)
3. Agrega opción en HTML si es necesario

---

## 🔄 Flujo de Datos

```
Usuario Admin llena formulario
         ↓
submitBooking() valida y crea objeto
         ↓
addReservation() → Supabase
         ↓
window.supabaseClient.insert() → BD
         ↓
Caché se limpia (cachedReservations = {})
         ↓
renderCalendar() recarga desde Supabase
         ↓
Todas las ventanas ven actualización en tiempo real (cada 5 segundos)
```

---

## 🚨 Debugging

### Ver errores en navegador
1. Abre DevTools (F12)
2. Pestaña "Console"
3. Busca mensajes rojo (errores)

### Logs útiles
```javascript
console.log(state);  // Ver estado global
console.log(window.supabaseClient);  // Verificar Supabase conectado
```

### Problemas Comunes

**"Identifier 'supabase' has already been declared"**
- Asegúrate de usar `window.supabaseClient` NO `supabase`

**Las reservas no aparecen**
- Verifica que Supabase está conectado en consola
- Revisa que hay datos en la tabla `reservations`

**Deploy no actualiza**
- Espera 2-3 minutos a que Netlify compile
- Recarga con Ctrl+Shift+R para limpiar caché

---

## 📚 Próximas Features (Roadmap)

- [ ] Recurrencia de reservas (semanal, mensual)
- [ ] Notificaciones por email
- [ ] Integración con Google Calendar
- [ ] Reportes de uso por sala
- [ ] Bloques de tiempo personalizados
- [ ] Aprobación de reservas (workflow)

---

## 🆘 Necesitas Ayuda?

Si tienes dudas que Claude no puede resolver:
1. **Consulta el código:** El archivo tiene comentarios explicativos
2. **Revisa Git log:** `git log --oneline` para ver qué cambios se hicieron
3. **Prueba en consola:** Usa DevTools para inspeccionar valores
4. **Pregunta a Federica:** Ella creó el sistema original

---

**¡Listo para empezar! 🚀**
