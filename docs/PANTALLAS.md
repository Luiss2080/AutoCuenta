# Pantallas y flujos — AutoCuenta

> Guía de interfaz para construir cada pantalla: ruta, contenido, estados y bosquejo. Complementa la sección 5 de [SPEC.md](SPEC.md).

## 1. Mapa de rutas

| Pantalla | Archivo (Expo Router) | Tipo |
|---|---|---|
| Inicio | `app/index.tsx` | Inicio |
| Vehículos | `app/vehiculos.tsx` | Lista / vista |
| Conductores | `app/conductores.tsx` | Lista / vista |
| Asignaciones | `app/asignaciones.tsx` | Lista / vista |
| Nuevo movimiento | `app/nuevo-movimiento.tsx` | Formulario |
| Liquidación semanal | `app/liquidacion-semanal.tsx` | Lista / vista |
| Rentabilidad | `app/rentabilidad.tsx` | Lista / vista |

**Convenciones de interfaz**

- Una acción principal por pantalla, en un botón grande abajo o en un botón flotante.
- Los formularios se abren como pantalla completa, validan al enviar y muestran el error junto al campo.
- Toda lista tiene estado vacío con una frase y un botón que lleva a la acción.
- Las acciones que borran piden confirmación explícita con el nombre del registro.
- El botón atrás nunca pierde datos sin avisar: si el formulario tiene cambios, se pregunta antes de salir.
- Textos en español de Bolivia (voseo opcional, siempre claros); montos como `Bs 1.250,00`.

## 2. Detalle por pantalla

### 2.1 Inicio

- **Archivo:** `app/index.tsx`
- **Qué muestra:** Resultado de la semana, vehículos y liquidaciones pendientes
- **Acciones:** Registrar ingreso o gasto

**Bosquejo**

```
┌──────────────────────────────┐
│ AutoCuenta                   │
├──────────────────────────────┤
│ [ Resumen y alertas ]        │
│                              │
│  ▸ Vehículos                │
│  ▸ Conductores              │
│  ▸ Asignaciones             │
│  ▸ Liquidación semanal      │
│                              │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Resultado de la semana, vehículos y liquidaciones pendientes |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.2 Vehículos

- **Archivo:** `app/vehiculos.tsx`
- **Qué muestra:** Lista con conductor actual y resultado del mes
- **Acciones:** Crear, editar
- **Historias relacionadas:** HU-01, HU-02, HU-03, HU-04 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Vehículos                   │
├──────────────────────────────┤
│ placa · anio                 │
│ placa · anio                 │
│ placa · anio                 │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay vehiculos. Tocá «Crear» para empezar. |
| Con datos | Lista con conductor actual y resultado del mes |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.3 Conductores

- **Archivo:** `app/conductores.tsx`
- **Qué muestra:** Lista con vehículo asignado
- **Acciones:** Crear, editar
- **Historias relacionadas:** HU-01, HU-02, HU-03, HU-04 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Conductores                 │
├──────────────────────────────┤
│ nombre · activo              │
│ nombre · activo              │
│ nombre · activo              │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay conductores. Tocá «Crear» para empezar. |
| Con datos | Lista con vehículo asignado |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.4 Asignaciones

- **Archivo:** `app/asignaciones.tsx`
- **Qué muestra:** Historial por vehículo con porcentaje
- **Acciones:** Asignar, cerrar asignación
- **Historias relacionadas:** HU-01, HU-02, HU-04 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Asignaciones                │
├──────────────────────────────┤
│ desde · vehiculo_id          │
│ desde · vehiculo_id          │
│ desde · vehiculo_id          │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay asignaciones. Tocá «Asignar» para empezar. |
| Con datos | Historial por vehículo con porcentaje |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.5 Nuevo movimiento

- **Archivo:** `app/nuevo-movimiento.tsx`
- **Qué muestra:** Tipo, vehículo, conductor, categoría, monto, a cargo del conductor, fecha
- **Acciones:** Guardar
- **Historias relacionadas:** HU-01, HU-02, HU-03, HU-04, HU-06 (sugerido por coincidencia de temas; confirmar al construir).

**Campos del formulario**

| Campo | Control | Obligatorio | Validación |
|---|---|---|---|
| vehiculo id | Selector de vehiculos | Sí | Debe existir en `vehiculos` |
| conductor id | Selector de conductores | No | Debe existir en `conductores` |
| tipo | Selector / fichas | Sí | Uno de: `ingreso`, `gasto` |
| categoria | Selector / fichas | Sí | Uno de: `viajes`, `otros_ingresos`, `combustible`, `mantenimiento`, `repuestos`, `lavado`, `multas`, `documentos`, `otros` |
| monto | Numérico decimal | Sí | Mayor a 0; Hasta 2 decimales |
| a cargo conductor | Numérico entero | Sí | Entero mayor o igual a 0 |
| nota | Texto largo | No | Hasta 280 caracteres |
| fecha | Fecha | Sí | Fecha real en formato AAAA-MM-DD |

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Nuevo movimiento            │
├──────────────────────────────┤
│ vehiculo id                  │
│ [____________________]       │
│ conductor id                 │
│ [____________________]       │
│ tipo                         │
│ [____________________]       │
│ categoria                    │
│ [____________________]       │
│ monto                        │
│ [____________________]       │
│                              │
│ [        Guardar        ]    │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay movimientos. Tocá «Guardar» para empezar. |
| Con datos | Tipo, vehículo, conductor, categoría, monto, a cargo del conductor, fecha |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.6 Liquidación semanal

- **Archivo:** `app/liquidacion-semanal.tsx`
- **Qué muestra:** Detalle por conductor y semana
- **Acciones:** Cerrar, compartir
- **Historias relacionadas:** HU-01, HU-03, HU-06 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Liquidación semanal         │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Detalle por conductor y semana |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.7 Rentabilidad

- **Archivo:** `app/rentabilidad.tsx`
- **Qué muestra:** Ranking de vehículos por período
- **Acciones:** Cambiar período, exportar
- **Historias relacionadas:** HU-01, HU-02, HU-04, HU-06 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Rentabilidad                │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Ranking de vehículos por período |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

## 3. Flujos principales

**HU-01 — Registrar vehículos y conductores**

```
lo asigno a un conductor con 50%  →  queda vigente la asignación con su porcentaje
```

**HU-02 — Registrar ingresos y gastos**

```
lo registro con la categoría y el vehículo  →  aparece en el historial del vehículo
```

**HU-03 — Liquidación semanal por conductor**

```
genero la liquidación  →  el total es 850 Bs y se muestra el detalle
```

**HU-04 — Rentabilidad por vehículo**

```
abro Rentabilidad  →  veo cada vehículo con ingresos, gastos, resultado y margen ordenados de mayor a menor
```

**HU-05 — Recordatorios de mantenimiento y documentos**

```
llega la fecha del aviso  →  recibo una notificación local con el vehículo
```

**HU-06 — Exportar para contabilidad**

```
toco «Exportar CSV»  →  se genera un archivo con una fila por movimiento y sus categorías
```
