# AguaPro 💧 — Sistema de Gestión de Agua en Sachet

Aplicación web especializada para empresas de producción y distribución de agua en sachet 400ml.

---

## 🗄️ 1. Configurar Supabase

### Paso 1 — Crear proyecto
1. Ve a [supabase.com](https://supabase.com) → New Project

### Paso 2 — Crear tablas
1. **SQL Editor** → pega el SQL que aparece en la pantalla de configuración de la app → **Run**

### Paso 3 — Obtener credenciales
1. **Settings → API** → copia **Project URL** y **anon public key**

---

## 🚀 2. Desplegar en Vercel

```bash
# Opción CLI
npm install -g vercel
vercel --prod

# Opción GitHub: sube los archivos a un repo y conecta en vercel.com
```

---

## 📱 3. Usar en Android

Abre la URL en Chrome Android → menú → **"Agregar a pantalla de inicio"**

---

## 📋 Módulos

### Módulo 1 — Producción
- Registro de lotes por hora y día con operario
- Contador en tiempo real (sachet producidos hoy, esta hora)
- Registro rápido por franja horaria (06:00-07:00, etc.)
- Historial diario con gráfico por hora
- Registro de pérdidas (rotura, sellado, contaminación, etc.)
- Panel de stock: producido vs vendido vs pérdidas

### Módulo 2 — Mapa PDV
- Mapa OpenStreetMap real con marcadores numerados
- Datos completos del punto de venta: propietario, teléfono, dirección, tipo
- Foto de la fachada tomada con la cámara del dispositivo
- GPS real del dispositivo para georeferenciar
- **Ruteo óptimo** — algoritmo de vecino más cercano que ordena los PDV por proximidad y dibuja la ruta en el mapa
- Popup informativo en cada marcador con ventas y pedidos pendientes

### Módulo 3 — Ventas
- **Ventas diarias por PDV**: selector de fecha, métricas del día, tarjetas por PDV con porcentaje del total
- **Pre-ventas**: registrar pedidos a futuro con fecha de entrega y precio
- **Gestión de pedidos**: pendientes y entregados, marcar como entregado
- Detalle completo por PDV al tocar la tarjeta

---

## 🗃️ Tablas Supabase

| Tabla | Descripción |
|---|---|
| `produccion` | Lotes registrados con hora, operario y cantidad |
| `perdidas` | Registro de merma con motivo |
| `pdvs` | Puntos de venta con GPS, foto y datos del propietario |
| `ventas` | Pre-ventas y ventas con estado pendiente/entregado |
| `config` | Configuración como stock máximo |

---

## 🔄 Nuevas tablas (v2) — ejecutar en Supabase SQL Editor

```sql
create table if not exists sessions (
  id bigint primary key generated always as identity,
  user_id uuid,
  user_name text,
  user_role text,
  action text,
  device text,
  created_at timestamptz default now()
);

create table if not exists route_history (
  id bigint primary key generated always as identity,
  fecha date,
  user_name text,
  user_id uuid,
  total_pdvs integer,
  start_lat double precision,
  start_lng double precision,
  stops jsonb,
  created_at timestamptz default now()
);

alter table sessions enable row level security;
alter table route_history enable row level security;
create policy "open" on sessions for all using (true) with check (true);
create policy "open" on route_history for all using (true) with check (true);
```

También habilitar autenticación en Supabase: **Authentication → Providers → Email** (activar).

