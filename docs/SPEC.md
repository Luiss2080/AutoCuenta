# Especificación del MVP — AutoCuenta

> Documento vivo. Si una decisión cambia, se actualiza acá primero y después en el código.

## 1. Resumen

**Propuesta de valor:** Que el dueño sepa cuánto gana cada vehículo y cuánto le corresponde a cada conductor cada semana, sin discusiones.

**Problema:** Los ingresos y gastos de cada auto se anotan en cuadernos o mensajes; al liquidar con el conductor hay discusiones sobre combustible, repuestos y porcentajes, y no se sabe qué vehículo es realmente rentable.

**Cómo se resuelve hoy:** Cuaderno, planillas de Excel y mensajes de WhatsApp.

**Usuario objetivo:** Dueño de 1 a 20 vehículos con conductores a porcentaje o a cuota, que liquida semanalmente.

**Métricas de éxito del MVP:**

- Registrar un ingreso o gasto en menos de 15 segundos.
- Generar la liquidación semanal de un conductor en un toque.
- Ver la rentabilidad de cada vehículo del período.

## 2. Alcance del MVP

### Incluido

- Registro de vehículos y conductores, con sus asignaciones.
- Registro de ingresos y gastos por unidad, como combustible o mantenimiento.
- Liquidación semanal por conductor.
- Cálculo de rentabilidad por vehículo en un periodo.
- Recordatorios de mantenimiento y de documentos.
- Exportación a CSV para contabilidad.

### Fuera del MVP

- Cuentas de usuario, inicio de sesión y multiusuario.
- Servidor propio, nube o sincronización entre dispositivos.
- Pagos en línea o pasarelas de cobro.
- Publicación en App Store (iOS).
- Cualquier servicio de pago o con costo recurrente.

## 3. Reglas de negocio

Reglas que el código **debe** cumplir. Cada una se cubre con pruebas automáticas cuando es lógica pura.

- **RN-01** Un movimiento es de tipo ingreso o gasto y siempre pertenece a un vehículo; el conductor es opcional.
- **RN-02** Un vehículo tiene a lo sumo un conductor asignado vigente a la vez.
- **RN-03** Categorías base de gasto: combustible, mantenimiento, repuestos, lavado, multas, documentos, otros.
- **RN-04** Liquidación del conductor = ingresos del período × porcentaje del conductor − gastos marcados «a cargo del conductor».
- **RN-05** Si la liquidación es negativa, queda como saldo a favor del dueño y se arrastra a la semana siguiente.
- **RN-06** Una liquidación cerrada no se edita; los errores se corrigen con un movimiento de ajuste en la semana siguiente.
- **RN-07** Rentabilidad del vehículo = ingresos − gastos del período; margen = rentabilidad ÷ ingresos.

## 4. Historias de usuario y criterios de aceptación

Formato: **Dado** un contexto, **Cuando** el usuario hace algo, **Entonces** pasa algo verificable.

### HU-01 — Registrar vehículos y conductores

**Como** dueño **quiero** cargar mis vehículos y conductores y asignarlos **para** saber quién maneja qué.

**Fase:** 2 — Vehículos, conductores y movimientos

**Criterios de aceptación:**

1. **Dado:** un vehículo libre. **Cuando:** lo asigno a un conductor con 50%. **Entonces:** queda vigente la asignación con su porcentaje.
2. **Dado:** un vehículo con conductor vigente. **Cuando:** intento asignar otro. **Entonces:** la app pide cerrar la asignación actual primero.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-02 — Registrar ingresos y gastos

**Como** dueño **quiero** anotar ingresos y gastos por vehículo **para** tener las cuentas al día.

**Fase:** 2 — Vehículos, conductores y movimientos

**Criterios de aceptación:**

1. **Dado:** un gasto de combustible de 200 Bs. **Cuando:** lo registro con la categoría y el vehículo. **Entonces:** aparece en el historial del vehículo.
2. **Dado:** un monto menor o igual a 0. **Cuando:** intento guardar. **Entonces:** la app lo rechaza.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-03 — Liquidación semanal por conductor

**Como** dueño **quiero** generar la liquidación de la semana **para** pagar al conductor lo justo.

**Fase:** 3 — Liquidación semanal

**Criterios de aceptación:**

1. **Dado:** ingresos de 2.000 Bs con 50% y 150 Bs de combustible a cargo del conductor. **Cuando:** genero la liquidación. **Entonces:** el total es 850 Bs y se muestra el detalle.
2. **Dado:** una liquidación cerrada. **Cuando:** intento editarla. **Entonces:** solo puedo consultarla.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-04 — Rentabilidad por vehículo

**Como** dueño **quiero** ver ingresos, gastos y margen por vehículo en un período **para** decidir qué unidades conservar.

**Fase:** 4 — Reportes de rentabilidad

**Criterios de aceptación:**

1. **Dado:** un período elegido. **Cuando:** abro Rentabilidad. **Entonces:** veo cada vehículo con ingresos, gastos, resultado y margen ordenados de mayor a menor.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-05 — Recordatorios de mantenimiento y documentos

**Como** dueño **quiero** recibir avisos de vencimientos **para** evitar multas y averías.

**Fase:** 5 — Recordatorios y exportación

**Criterios de aceptación:**

1. **Dado:** un SOAT que vence en 30 días. **Cuando:** llega la fecha del aviso. **Entonces:** recibo una notificación local con el vehículo.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-06 — Exportar para contabilidad

**Como** dueño **quiero** exportar movimientos y liquidaciones a CSV **para** pasarlos al contador.

**Fase:** 5 — Recordatorios y exportación

**Criterios de aceptación:**

1. **Dado:** movimientos del mes. **Cuando:** toco «Exportar CSV». **Entonces:** se genera un archivo con una fila por movimiento y sus categorías.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

## 5. Pantallas y navegación

| Pantalla | Qué muestra | Acciones principales |
|---|---|---|
| Inicio | Resultado de la semana, vehículos y liquidaciones pendientes | Registrar ingreso o gasto |
| Vehículos | Lista con conductor actual y resultado del mes | Crear, editar |
| Conductores | Lista con vehículo asignado | Crear, editar |
| Asignaciones | Historial por vehículo con porcentaje | Asignar, cerrar asignación |
| Nuevo movimiento | Tipo, vehículo, conductor, categoría, monto, a cargo del conductor, fecha | Guardar |
| Liquidación semanal | Detalle por conductor y semana | Cerrar, compartir |
| Rentabilidad | Ranking de vehículos por período | Cambiar período, exportar |

**Navegación:** la pantalla de inicio es el punto de partida; desde ahí se llega a cada sección con un toque y se vuelve con el botón atrás. Las acciones frecuentes (nuevo registro) están siempre a la vista. La navegación se implementa con Expo Router (rutas por archivo en `app/`).

## 6. Modelo de datos

Base SQLite local versionada con `PRAGMA user_version`. Las fechas se guardan como texto ISO 8601 y los montos como números reales (en bolivianos).

### `vehiculos`

Cada vehículo de la flota.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `placa` | TEXT | Obligatorio; Único |
| `modelo` | TEXT | Obligatorio |
| `anio` | INTEGER | Opcional |
| `activo` | INTEGER | Obligatorio; Por defecto 1 |

### `conductores`

Conductores con su teléfono.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `nombre` | TEXT | Obligatorio |
| `telefono` | TEXT | Opcional |
| `activo` | INTEGER | Obligatorio; Por defecto 1 |

### `asignaciones`

Qué conductor maneja qué vehículo, desde cuándo y con qué porcentaje (nueva respecto al README: materializa «asignaciones»).

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `vehiculo_id` | INTEGER | Obligatorio; Referencia a vehiculos |
| `conductor_id` | INTEGER | Obligatorio; Referencia a conductores |
| `desde` | TEXT | Obligatorio |
| `hasta` | TEXT | Opcional |
| `porcentaje_conductor` | REAL | Obligatorio; Por defecto 50 |

### `movimientos`

Ingresos y gastos por unidad, con categoría y si el gasto es a cargo del conductor.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `vehiculo_id` | INTEGER | Obligatorio; Referencia a vehiculos |
| `conductor_id` | INTEGER | Referencia a conductores |
| `tipo` | TEXT | Obligatorio |
| `categoria` | TEXT | Obligatorio |
| `monto` | REAL | Obligatorio |
| `a_cargo_conductor` | INTEGER | Obligatorio; Por defecto 0 |
| `nota` | TEXT | Opcional |
| `fecha` | TEXT | Obligatorio |

### `liquidaciones`

Cierres semanales por conductor.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `conductor_id` | INTEGER | Obligatorio; Referencia a conductores |
| `desde` | TEXT | Obligatorio |
| `hasta` | TEXT | Obligatorio |
| `total` | REAL | Obligatorio |
| `cerrada` | INTEGER | Obligatorio; Por defecto 0 |

Detalle de la política de migraciones: nunca editar una migración publicada; agregar una nueva al final de la lista en `src/db/schema.ts`.

## 7. Casos borde a contemplar

- Conductor que cambia de vehículo a mitad de semana: la liquidación suma por movimiento, no por asignación vigente.
- Gasto compartido entre dos vehículos: registrar un movimiento por vehículo.
- Porcentaje que cambia a mitad de período: aplica desde la fecha de la nueva asignación.
- Vehículo vendido: se archiva sin perder historial.
- Movimiento con fecha futura: se advierte pero se permite.

## 8. Requisitos no funcionales

- **Offline total:** ninguna función del MVP requiere internet.
- **Rendimiento:** la app abre en menos de 2 segundos; las listas responden de inmediato con cientos de registros (consultas con índices y listas virtualizadas).
- **Privacidad:** los datos nunca salen del teléfono, salvo lo que el usuario comparte a propósito.
- **Idioma y formato:** español de Bolivia, montos en bolivianos (Bs) con dos decimales, fechas día/mes/año.
- **Usabilidad:** botones grandes, formularios cortos y flujos de pocos toques; pensado para usarse de pie y con una mano.
- **Accesibilidad básica:** contraste suficiente, etiquetas de accesibilidad en botones y tamaño de texto que respeta el ajuste del sistema.
- **Calidad:** `npm run lint`, `npx tsc --noEmit` y `npm test` pasan en cada commit (CI en GitHub Actions).
- **Compatibilidad:** Android 8 o superior (probado con Expo Go y con el APK).

### Capacidades de Expo que usa

| Paquete | Para qué | Permiso |
|---|---|---|
| `expo-notifications` | Recordatorios locales (programados en el teléfono, sin servidor) | Notificaciones |
| `expo-file-system` | Guardar y leer archivos locales (CSV, respaldos) | — |
| `expo-sharing` | Compartir PDF, CSV o respaldos con otras apps | — |

## 9. Riesgos y mitigaciones

| Riesgo | Mitigación |
|---|---|
| Disputas por el criterio de liquidación | Reglas visibles en pantalla y detalle de cada línea. |
| Datos incompletos por no registrar a tiempo | Registro rápido y recordatorio diario opcional. |
| Cambios de esquema que rompan datos | Migraciones con `PRAGMA user_version`; nunca editar una migración publicada |
| El alcance crece | Todo lo que no esté en la sección 2 va a «Después del MVP» en el [ROADMAP](ROADMAP.md) |

## 10. Criterios para lanzar el MVP (v1.0.0)

- [ ] Todas las historias de la sección 4 cumplen sus criterios de aceptación.
- [ ] Todas las reglas de negocio de la sección 3 tienen pruebas o verificación manual.
- [ ] Probado en un teléfono Android real, en modo avión.
- [ ] APK generado con `npm run build:apk` e instalado en un teléfono limpio.
- [ ] README con capturas de pantalla y CHANGELOG actualizado.
- [ ] Release v1.0.0 publicado con el APK adjunto.

## 11. Glosario

| Término | Significado en AutoCuenta |
|---|---|
| `vehiculos` | Cada vehículo de la flota. |
| `conductores` | Conductores con su teléfono. |
| `asignaciones` | Qué conductor maneja qué vehículo, desde cuándo y con qué porcentaje (nueva respecto al README: materializa «asignaciones»). |
| `movimientos` | Ingresos y gastos por unidad, con categoría y si el gasto es a cargo del conductor. |
| `liquidaciones` | Cierres semanales por conductor. |
| `movimientos.tipo` | Valores: ingreso, gasto |
| `movimientos.categoria` | Valores: viajes, otros_ingresos, combustible, mantenimiento, repuestos, lavado, multas, documentos, otros |

## 12. Mensajes estándar

| Situación | Mensaje |
|---|---|
| Campo obligatorio vacío | Completá este campo |
| Número inválido | Ingresá un número válido |
| Monto cero o negativo | El monto debe ser mayor a 0 |
| Fecha inválida | Fecha no válida |
| Guardado correcto | Guardado |
| Error al guardar | No se pudo guardar. Intentá de nuevo |
| Confirmar borrado | ¿Borrar «{nombre}»? No se puede deshacer |
| Lista vacía | Todavía no hay nada acá. Tocá «+» para empezar |

## 13. Trazabilidad

| Historia | Fase | Casos de prueba |
|---|---|---|
| HU-01 Registrar vehículos y conductores | 2 | CP-01-1, CP-01-2 |
| HU-02 Registrar ingresos y gastos | 2 | CP-02-1, CP-02-2 |
| HU-03 Liquidación semanal por conductor | 3 | CP-03-1, CP-03-2 |
| HU-04 Rentabilidad por vehículo | 4 | CP-04-1 |
| HU-05 Recordatorios de mantenimiento y documentos | 5 | CP-05-1 |
| HU-06 Exportar para contabilidad | 5 | CP-06-1 |

## 14. Documentos relacionados

- [DATOS.md](DATOS.md) · [PANTALLAS.md](PANTALLAS.md) · [PRUEBAS.md](PRUEBAS.md) · [BACKLOG.md](BACKLOG.md) · [ROADMAP.md](ROADMAP.md) · [PLANNING.md](PLANNING.md)
