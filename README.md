# POS / Comandera Digital

Sistema web (HTML/CSS/JS) de un **POS a medida** enfocado en operación rápida en celular:
- **Captura de pedidos (comandero)**
- **Dashboard de ventas** (global y por sucursal)
- **Inventario básico con alertas**

> El modo multi-dispositivo en tiempo real requiere Firebase/Firestore.

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

#### Roles (auth)
Login por correo/contraseña con Firebase Auth (Email/Password).

- `op@losdecostilla.com` / `ger@losdecostilla.com`: rol **admin/operación**.
- `at01@...`: comandero **Atasta**.
- `ta01@...`: comandero **Tamulté**.
- `visor@...`: pensado para vista de dashboard en matriz (solo lectura cuando se conecte DB).

> El control de permisos “real” se asegura con reglas de base de datos (Firestore). En la demo se controla UI/flujo.

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
- Recetas/consumo automático: módulo futuro.

---

## Datos y persistencia (demo)

Por ahora se usa **`localStorage`** para persistencia local:
- Ventas: `ldc_sales`
- Folios: `ldc_folio`
- Preferencias UI (vista iconos, categorías abiertas, etc.)

Esto significa:
- Funciona perfecto en un dispositivo.
- **No sincroniza entre dispositivos** hasta conectar una base de datos.

---

## Roadmap (cuando confirmen / paguen)

- ✅ Firebase Auth (ya integrado en `comand.html`)
- ⏳ Firestore realtime para sincronizar ventas entre sucursales y matriz
- ⏳ Catálogo editable por admin (nombre/precio/agotado)
- ⏳ Cancelaciones solo admin + motivo (antes del cierre)
- ⏳ Reportes automáticos (correo) / WhatsApp API (módulo extra)
- ⏳ Recetas por proteína (gramajes) + consumo automático + mermas

---

## Cómo abrir la demo

Si tienes un servidor local (ej. `python3 -m http.server 2200`), abre:

- `http://localhost:2200/comand.html`
- `http://localhost:2200/ventas.html`
- `http://localhost:2200/inventario.html`

En GitHub Pages:
- `https://unknownshopper.github.io/jinetes/comand.html`
- `https://unknownshopper.github.io/jinetes/ventas.html`
- `https://unknownshopper.github.io/jinetes/inventario.html`
