# Modelo de datos — AutoCuenta

> Diseño detallado de la base SQLite: campos, validaciones, consultas y migraciones. Complementa la sección 6 de [SPEC.md](SPEC.md).

## 1. Convenciones

- Base local `SQLite` con `expo-sqlite`; versión del esquema en `PRAGMA user_version`.
- Claves primarias `INTEGER AUTOINCREMENT` llamadas `id`.
- Fechas como texto ISO `AAAA-MM-DD` (y `AAAA-MM-DDTHH:MM` cuando hay hora). Nunca se guardan textos como «hoy».
- Montos como `REAL` en bolivianos; se redondean a 2 decimales al guardar y se formatean al mostrar.
- Booleanos como `INTEGER` 0/1.
- Los textos se guardan sin espacios sobrantes (`trim()`).
- Todas las consultas usan parámetros (`?`), nunca concatenan texto del usuario.
- Las claves foráneas se activan con `PRAGMA foreign_keys = ON` al abrir la base.

## 2. Relaciones

| Campo | Apunta a | Comportamiento |
|---|---|---|
| `asignaciones.vehiculo_id` | `vehiculos` | Sin borrado en cascada |
| `asignaciones.conductor_id` | `conductores` | Sin borrado en cascada |
| `movimientos.vehiculo_id` | `vehiculos` | Sin borrado en cascada |
| `movimientos.conductor_id` | `conductores` | Sin borrado en cascada |
| `liquidaciones.conductor_id` | `conductores` | Sin borrado en cascada |

```
vehiculos  ──<  asignaciones, movimientos
conductores  ──<  asignaciones, movimientos, liquidaciones
asignaciones
movimientos
liquidaciones
```

## 3. Detalle por tabla

### 3.1 `vehiculos`

Cada vehículo de la flota.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `placa` | TEXT → `string` | Texto (mayúsculas) | Obligatorio; Se guarda en mayúsculas; 5 a 10 caracteres alfanuméricos; Único en la tabla | «Placa no válida» |
| `modelo` | TEXT → `string` | Texto | Obligatorio; Texto de 1 a 80 caracteres sin espacios sobrantes | «Escribí modelo» |
| `anio` | INTEGER → `number \| null` | Numérico entero | Opcional; Entero mayor o igual a 0 | «Ingresá un número entero» |
| `activo` | INTEGER → `number` | Interruptor | Obligatorio; 0 o 1; Valor inicial `1` | — |

**Consultas base** (en `src/db/vehiculos.ts`):

```ts
listar(): Promise<Vehiculo[]>            // SELECT * FROM vehiculos ORDER BY id DESC
obtener(id: number): Promise<Vehiculo | null>   // SELECT * FROM vehiculos WHERE id = ?
crear(datos: NuevoVehiculo): Promise<number>   // INSERT INTO vehiculos (placa, modelo, anio, activo) VALUES (?, ?, ?, ?)
actualizar(id: number, datos: NuevoVehiculo): Promise<void>   // UPDATE vehiculos SET placa = ?, modelo = ?, anio = ?, activo = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM vehiculos WHERE id = ?
```

### 3.2 `conductores`

Conductores con su teléfono.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `nombre` | TEXT → `string` | Texto | Obligatorio; Texto de 1 a 80 caracteres sin espacios sobrantes | «Escribí nombre» |
| `telefono` | TEXT → `string \| null` | Teléfono | Opcional; Solo dígitos, 7 a 15 caracteres (se ignoran espacios y guiones) | «Teléfono no válido» |
| `activo` | INTEGER → `number` | Interruptor | Obligatorio; 0 o 1; Valor inicial `1` | — |

**Consultas base** (en `src/db/conductores.ts`):

```ts
listar(): Promise<Conductor[]>            // SELECT * FROM conductores ORDER BY id DESC
obtener(id: number): Promise<Conductor | null>   // SELECT * FROM conductores WHERE id = ?
crear(datos: NuevoConductor): Promise<number>   // INSERT INTO conductores (nombre, telefono, activo) VALUES (?, ?, ?)
actualizar(id: number, datos: NuevoConductor): Promise<void>   // UPDATE conductores SET nombre = ?, telefono = ?, activo = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM conductores WHERE id = ?
```

### 3.3 `asignaciones`

Qué conductor maneja qué vehículo, desde cuándo y con qué porcentaje (nueva respecto al README: materializa «asignaciones»).

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `vehiculo_id` | INTEGER → `number` | Selector de vehiculos | Obligatorio; Debe existir en `vehiculos` | «Elegí un registro» |
| `conductor_id` | INTEGER → `number` | Selector de conductores | Obligatorio; Debe existir en `conductores` | «Elegí un registro» |
| `desde` | TEXT → `string` | Fecha | Obligatorio; Fecha real en formato AAAA-MM-DD | «Fecha no válida» |
| `hasta` | TEXT → `string \| null` | Fecha | Opcional; Fecha real en formato AAAA-MM-DD | «Fecha no válida» |
| `porcentaje_conductor` | REAL → `number` | Numérico decimal | Obligatorio; Mayor o igual a 0; Hasta 2 decimales; Valor inicial `50` | «Ingresá un número válido» |

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_asignaciones_vehiculo_id ON asignaciones(vehiculo_id);
CREATE INDEX IF NOT EXISTS idx_asignaciones_conductor_id ON asignaciones(conductor_id);
```

**Consultas base** (en `src/db/asignaciones.ts`):

```ts
listar(): Promise<Asignaciion[]>            // SELECT * FROM asignaciones ORDER BY id DESC
obtener(id: number): Promise<Asignaciion | null>   // SELECT * FROM asignaciones WHERE id = ?
crear(datos: NuevoAsignaciion): Promise<number>   // INSERT INTO asignaciones (vehiculo_id, conductor_id, desde, hasta, porcentaje_conductor) VALUES (?, ?, ?, ?, ?)
actualizar(id: number, datos: NuevoAsignaciion): Promise<void>   // UPDATE asignaciones SET vehiculo_id = ?, conductor_id = ?, desde = ?, hasta = ?, porcentaje_conductor = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM asignaciones WHERE id = ?
listarPorVehiculo(vehiculo_id: number): Promise<Asignaciion[]>   // WHERE vehiculo_id = ?
listarPorConductor(conductor_id: number): Promise<Asignaciion[]>   // WHERE conductor_id = ?
```

### 3.4 `movimientos`

Ingresos y gastos por unidad, con categoría y si el gasto es a cargo del conductor.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `vehiculo_id` | INTEGER → `number` | Selector de vehiculos | Obligatorio; Debe existir en `vehiculos` | «Elegí un registro» |
| `conductor_id` | INTEGER → `number \| null` | Selector de conductores | Opcional; Debe existir en `conductores` | «Elegí un registro» |
| `tipo` | TEXT → `string` | Selector / fichas | Obligatorio; Uno de: `ingreso`, `gasto` | «Elegí una opción» |
| `categoria` | TEXT → `string` | Selector / fichas | Obligatorio; Uno de: `viajes`, `otros_ingresos`, `combustible`, `mantenimiento`, `repuestos`, `lavado`, `multas`, `documentos`, `otros` | «Elegí una opción» |
| `monto` | REAL → `number` | Numérico decimal | Obligatorio; Mayor a 0; Hasta 2 decimales | «Ingresá un monto mayor a 0» |
| `a_cargo_conductor` | INTEGER → `number` | Numérico entero | Obligatorio; Entero mayor o igual a 0; Valor inicial `0` | «Ingresá un número entero» |
| `nota` | TEXT → `string \| null` | Texto largo | Opcional; Hasta 280 caracteres | «Máximo 280 caracteres» |
| `fecha` | TEXT → `string` | Fecha | Obligatorio; Fecha real en formato AAAA-MM-DD | «Fecha no válida» |

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_movimientos_vehiculo_id ON movimientos(vehiculo_id);
CREATE INDEX IF NOT EXISTS idx_movimientos_conductor_id ON movimientos(conductor_id);
CREATE INDEX IF NOT EXISTS idx_movimientos_fecha ON movimientos(fecha);
```

**Consultas base** (en `src/db/movimientos.ts`):

```ts
listar(): Promise<Movimiento[]>            // SELECT * FROM movimientos ORDER BY id DESC
obtener(id: number): Promise<Movimiento | null>   // SELECT * FROM movimientos WHERE id = ?
crear(datos: NuevoMovimiento): Promise<number>   // INSERT INTO movimientos (vehiculo_id, conductor_id, tipo, categoria, monto, a_cargo_conductor, nota, fecha) VALUES (?, ?, ?, ?, ?, ?, ?, ?)
actualizar(id: number, datos: NuevoMovimiento): Promise<void>   // UPDATE movimientos SET vehiculo_id = ?, conductor_id = ?, tipo = ?, categoria = ?, monto = ?, a_cargo_conductor = ?, nota = ?, fecha = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM movimientos WHERE id = ?
listarPorVehiculo(vehiculo_id: number): Promise<Movimiento[]>   // WHERE vehiculo_id = ?
listarPorConductor(conductor_id: number): Promise<Movimiento[]>   // WHERE conductor_id = ?
```

### 3.5 `liquidaciones`

Cierres semanales por conductor.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `conductor_id` | INTEGER → `number` | Selector de conductores | Obligatorio; Debe existir en `conductores` | «Elegí un registro» |
| `desde` | TEXT → `string` | Fecha | Obligatorio; Fecha real en formato AAAA-MM-DD | «Fecha no válida» |
| `hasta` | TEXT → `string` | Fecha | Obligatorio; Fecha real en formato AAAA-MM-DD | «Fecha no válida» |
| `total` | REAL → `number` | Numérico decimal | Obligatorio; Mayor o igual a 0; Hasta 2 decimales | «Ingresá un número válido» |
| `cerrada` | INTEGER → `number` | Interruptor | Obligatorio; 0 o 1; Valor inicial `0` | — |

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_liquidaciones_conductor_id ON liquidaciones(conductor_id);
```

**Consultas base** (en `src/db/liquidaciones.ts`):

```ts
listar(): Promise<Liquidaciion[]>            // SELECT * FROM liquidaciones ORDER BY id DESC
obtener(id: number): Promise<Liquidaciion | null>   // SELECT * FROM liquidaciones WHERE id = ?
crear(datos: NuevoLiquidaciion): Promise<number>   // INSERT INTO liquidaciones (conductor_id, desde, hasta, total, cerrada) VALUES (?, ?, ?, ?, ?)
actualizar(id: number, datos: NuevoLiquidaciion): Promise<void>   // UPDATE liquidaciones SET conductor_id = ?, desde = ?, hasta = ?, total = ?, cerrada = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM liquidaciones WHERE id = ?
listarPorConductor(conductor_id: number): Promise<Liquidaciion[]>   // WHERE conductor_id = ?
```

## 4. Tipos TypeScript

Copiar a `src/db/tipos.ts`:

```ts
export interface Vehiculo {
  id: number;
  placa: string;
  modelo: string;
  anio: number | null;
  activo: number;
}
export type NuevoVehiculo = Omit<Vehiculo, 'id'>;

export interface Conductor {
  id: number;
  nombre: string;
  telefono: string | null;
  activo: number;
}
export type NuevoConductor = Omit<Conductor, 'id'>;

export interface Asignaciion {
  id: number;
  vehiculo_id: number;
  conductor_id: number;
  desde: string;
  hasta: string | null;
  porcentaje_conductor: number;
}
export type NuevoAsignaciion = Omit<Asignaciion, 'id'>;

export interface Movimiento {
  id: number;
  vehiculo_id: number;
  conductor_id: number | null;
  tipo: string;
  categoria: string;
  monto: number;
  a_cargo_conductor: number;
  nota: string | null;
  fecha: string;
}
export type NuevoMovimiento = Omit<Movimiento, 'id'>;

export interface Liquidaciion {
  id: number;
  conductor_id: number;
  desde: string;
  hasta: string;
  total: number;
  cerrada: number;
}
export type NuevoLiquidaciion = Omit<Liquidaciion, 'id'>;

```

## 5. Valores permitidos (enumeraciones)

SQLite no tiene tipo enumerado: se valida en la capa de datos y se muestra con la etiqueta de la tabla.

**`movimientos.tipo`**

| Valor guardado | Etiqueta en pantalla |
|---|---|
| `ingreso` | Ingreso |
| `gasto` | Gasto |

**`movimientos.categoria`**

| Valor guardado | Etiqueta en pantalla |
|---|---|
| `viajes` | Viajes |
| `otros_ingresos` | Otros ingresos |
| `combustible` | Combustible |
| `mantenimiento` | Mantenimiento |
| `repuestos` | Repuestos |
| `lavado` | Lavado |
| `multas` | Multas |
| `documentos` | Documentos |
| `otros` | Otros |

```ts
export const MOVIMIENTOS_TIPO = ['ingreso', 'gasto'] as const;
export const MOVIMIENTOS_CATEGORIA = ['viajes', 'otros_ingresos', 'combustible', 'mantenimiento', 'repuestos', 'lavado', 'multas', 'documentos', 'otros'] as const;
```

## 6. Migraciones

Migración 1 = esquema inicial (ya en `src/db/schema.ts` tras la fase 1). Cada cambio posterior es una migración nueva al final de la lista; nunca se edita una ya publicada.

| Versión | Fase | Motivo |
|---|---|---|
| 2 | 5 | Mantenimiento y documentos por vehículo para los recordatorios. |
| 3 | 5 | Días de anticipación de los avisos. |

**Migración 2** (fase 5)

```sql
CREATE TABLE vencimientos (id INTEGER PRIMARY KEY AUTOINCREMENT, vehiculo_id INTEGER NOT NULL REFERENCES vehiculos(id) ON DELETE CASCADE, tipo TEXT NOT NULL, fecha TEXT NOT NULL, nota TEXT);
```

**Migración 3** (fase 5)

```sql
CREATE TABLE ajustes (clave TEXT PRIMARY KEY, valor TEXT NOT NULL);
```

## 7. Esquema inicial completo

```sql
CREATE TABLE vehiculos (id INTEGER PRIMARY KEY AUTOINCREMENT, placa TEXT NOT NULL UNIQUE, modelo TEXT NOT NULL, anio INTEGER, activo INTEGER NOT NULL DEFAULT 1);
CREATE TABLE conductores (id INTEGER PRIMARY KEY AUTOINCREMENT, nombre TEXT NOT NULL, telefono TEXT, activo INTEGER NOT NULL DEFAULT 1);
CREATE TABLE asignaciones (id INTEGER PRIMARY KEY AUTOINCREMENT, vehiculo_id INTEGER NOT NULL REFERENCES vehiculos(id), conductor_id INTEGER NOT NULL REFERENCES conductores(id), desde TEXT NOT NULL, hasta TEXT, porcentaje_conductor REAL NOT NULL DEFAULT 50);
CREATE TABLE movimientos (id INTEGER PRIMARY KEY AUTOINCREMENT, vehiculo_id INTEGER NOT NULL REFERENCES vehiculos(id), conductor_id INTEGER REFERENCES conductores(id), tipo TEXT NOT NULL, categoria TEXT NOT NULL, monto REAL NOT NULL, a_cargo_conductor INTEGER NOT NULL DEFAULT 0, nota TEXT, fecha TEXT NOT NULL);
CREATE TABLE liquidaciones (id INTEGER PRIMARY KEY AUTOINCREMENT, conductor_id INTEGER NOT NULL REFERENCES conductores(id), desde TEXT NOT NULL, hasta TEXT NOT NULL, total REAL NOT NULL, cerrada INTEGER NOT NULL DEFAULT 0);
```

## 8. Reglas de integridad que NO delega a la base

- **RN-01** Un movimiento es de tipo ingreso o gasto y siempre pertenece a un vehículo; el conductor es opcional.
- **RN-02** Un vehículo tiene a lo sumo un conductor asignado vigente a la vez.
- **RN-03** Categorías base de gasto: combustible, mantenimiento, repuestos, lavado, multas, documentos, otros.
- **RN-04** Liquidación del conductor = ingresos del período × porcentaje del conductor − gastos marcados «a cargo del conductor».
- **RN-05** Si la liquidación es negativa, queda como saldo a favor del dueño y se arrastra a la semana siguiente.
- **RN-06** Una liquidación cerrada no se edita; los errores se corrigen con un movimiento de ajuste en la semana siguiente.
- **RN-07** Rentabilidad del vehículo = ingresos − gastos del período; margen = rentabilidad ÷ ingresos.

Estas reglas se implementan en `src/lib/` como funciones puras y se prueban de forma automática (ver [PRUEBAS.md](PRUEBAS.md)).
