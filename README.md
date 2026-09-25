# POS / Comandera Digital

Sistema web (HTML/CSS/JS) de un **POS a medida** enfocado en operación rápida en celular:
- **Captura de pedidos (comandero)**
- **Dashboard de ventas** (global y por sucursal)
- **Inventario básico con alertas**

> Para sincronización multi-dispositivo (tiempo real) se requiere **Firebase/Firestore**.

---

## Vistas

### 1) Comandero (captura de pedidos)
Archivo: `comand.html`

- Selección de sucursal (según rol).
- Tipos de servicio:
  - Para comer aquí
  - Para llevar (requiere nombre)
  - A domicilio (requiere nombre + dirección)
  - A domicilio + Plataforma (no requiere nombre/dirección; usa folio/referencia)
- Menú por categorías con **+ / −** y muy pocos clicks.
- Categorías con **dropdown (accordion)** colapsables (pueden quedar todas colapsadas).
- Vista **Texto / Íconos** para las 6 tarjetas principales (servicio y pago).
- Nota de consumo (impresión) con sucursal/dirección y datos fiscales placeholders.
- Envío manual de resumen por WhatsApp (abre `wa.me`).

#### Limpieza visual del menú
- En tacos se elimina el prefijo repetido “Taco de …” (solo visual).
- En tortas se elimina el prefijo repetido “Torta de …” (solo visual).
- Consomé usa labels cortos: “De costilla” / “Para llevar”.

---

### 2) Ventas / Status (dashboard)
Archivo: `ventas.html`

- Scope: **Global / Tamulté / Atasta**.
- Dashboard oscuro con:
  - Totales por sucursal (según scope)
  - **Unidades por categoría (según menú)**: 🌮 Tacos / 🥪 Tortas / 🍲 Consomé / 🥤 Bebidas
- KPIs: total, órdenes, ticket promedio, última venta.
- Top vendidos (por unidades) con barras.
- Ritmo por hora con barras coloreadas.
- Datos demo:
  - **Generar día demo** (resistente a cuota de `localStorage`)
  - `+10 ventas`
  - Seed marketing (clientes/mensajes demo)

---

### 3) Inventario / Alertas
Archivo: `inventario.html`

- Scope: **Global / Tamulté / Atasta**.
- Inventario por sucursal con:
  - mínimos
  - alertas visuales
  - ajustes rápidos (+/−)
  - log de movimientos (demo)

---

### 4) Admin Info (log de accesos)
Archivo: `infdmn.html`

- Vista para gerencia/admin para visualizar la colección `audit_log` (quién entró, cuándo y desde qué página).

---

### 5) Arquitectura / Hardware
Archivo: `arqtec.html`

- Diagrama de instalación mínima por sucursal: **📱 1 celular + 🧾 1 impresora térmica 80mm**.
- Cotización orientativa de hardware (impresora/consumibles).

---

## Autenticación y permisos (roles)

Login por correo/contraseña con **Firebase Auth** (Email/Password) en `comand.html`.

### Roles definidos (por correo)
- **Admin / Operación**: `op@losdecostilla.com`, `ger@losdecostilla.com`, `the@unknownshoppers.com`
  - Puede operar y administrar (cuando se activen módulos): catálogo, cancelaciones, inventario.
- **Comandero**:
  - `ta01@...` → Tamulté
  - `at01@...` → Atasta
  - Puede capturar pedidos y cerrar ventas.
- **Visor**: `visor@losdecostilla.com`
  - Pensado para matriz (tablet) con transacciones en tiempo real cuando se conecte Firestore.

> Nota: la seguridad “real” (que no puedan escribir/leer lo que no deben) se garantiza con **reglas de Firestore**. La UI por sí sola no basta.

### Cancelaciones (regla de negocio)
- Comanderos **no cancelan**.
- Admin/operación cancelan con **nota de motivo** (flujo/implementación completa: fase siguiente).

---

## Gramajes / recetas (base)

Los gramajes aplican a la **proteína** (estándar inicial):
- Taco: **32 g**
- Torta: **58 g**

Consomé: porción / pendiente (definición posterior).

> Aún no se aplica consumo automático en inventario; se deja como módulo a activar cuando confirmen recetas/mermas.

---

## Auditoría (audit_log)

Se registra un evento “ligero” de acceso (A):
- quién (email/uid)
- cuándo (serverTimestamp)
- desde qué página (comand/ventas/inventario/infdmn)
- rol y sucursal inferidos por email

Requiere:
- Firestore habilitado
- reglas que permitan escribir/leer `audit_log` según rol

---

## Datos y persistencia (estado actual)

Actualmente la operación usa **`localStorage`** para persistencia local:
- Ventas: `ldc_sales`
- Folios: `ldc_folio`
- Preferencias UI (vista íconos, categorías abiertas, etc.)

Esto significa:
- Funciona perfecto en un dispositivo.
- **No sincroniza entre dispositivos** hasta conectar Firestore realtime.

---

## Roadmap

- ✅ Firebase Auth (integrado en `comand.html`)
- ⏳ Guards por rol/redirect (bloquear páginas privadas sin login; visor solo visor, comandero solo comand)
- ⏳ Firestore realtime: ventas en vivo entre sucursales y matriz
- ⏳ Catálogo editable por admin: nombre/precio/agotado + alta de productos
- ⏳ Cancelaciones solo admin + motivo (antes del cierre)
- ⏳ Recetas/gramajes + consumo automático + mermas
- ⏳ Reportes automáticos (correo) / WhatsApp API (módulo extra)

---

## Cómo abrir

Servidor local (ej. `python3 -m http.server 2200`):
- `http://localhost:2200/propuesta.html`
- `http://localhost:2200/comand.html`
- `http://localhost:2200/ventas.html`
- `http://localhost:2200/inventario.html`
- `http://localhost:2200/infdmn.html`
- `http://localhost:2200/arqtec.html`

GitHub Pages:
- `https://unknownshopper.github.io/jinetes/propuesta.html`
- `https://unknownshopper.github.io/jinetes/comand.html`
- `https://unknownshopper.github.io/jinetes/ventas.html`
- `https://unknownshopper.github.io/jinetes/inventario.html`
- `https://unknownshopper.github.io/jinetes/infdmn.html`
- `https://unknownshopper.github.io/jinetes/arqtec.html`
