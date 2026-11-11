# Plan de desarrollo — AutoCuenta

Guía para construir el MVP **fase por fase**. Complementa a [ROADMAP.md](ROADMAP.md) (qué y cuándo) y [SPEC.md](SPEC.md) (qué debe hacer la app).

## Forma de trabajo

1. **Una fase por rama:** `git checkout -b fase-N-nombre`.
2. **Un issue por tarea** (usá los títulos de abajo) y un milestone por fase en GitHub.
3. **Commits chicos** con prefijo: `feat:`, `fix:`, `test:`, `docs:`, `chore:`.
4. **Pull request al cerrar la fase** con la lista de criterios de salida marcada.
5. **Primero la lógica pura y sus pruebas** (en `src/lib/`), después la pantalla que la usa.
6. **Probá en el teléfono** al terminar cada tarea de pantalla, no solo al final de la fase.

### Definición de «terminado» (vale para toda tarea y fase)

- [ ] `npm run lint`, `npx tsc --noEmit` y `npm test` sin errores.
- [ ] Funciona en modo avión.
- [ ] Estados vacíos, de carga y de error resueltos, con textos en español.
- [ ] Los datos persisten al reiniciar la app.
- [ ] La regla de negocio correspondiente tiene prueba o verificación manual anotada.
- [ ] CHANGELOG actualizado al cerrar la fase.

## Calendario sugerido

| Fase | Nombre | Días | Acumulado |
|---|---|---|---|
| 1 | Estructura base y navegación | 3 | 3 |
| 2 | Vehículos, conductores y movimientos | 5 | 8 |
| 3 | Liquidación semanal | 4 | 12 |
| 4 | Reportes de rentabilidad | 3 | 15 |
| 5 | Recordatorios y exportación | 4 | 19 |

Total: **19 días** (~4 semanas). Si una fase se pasa del doble de lo estimado, recortá alcance dentro de la fase en lugar de saltar a la siguiente.

---

## Fase 1 — Estructura base y navegación

**Estimación:** 3 días.

**Objetivo:** crear el proyecto Expo con base de datos, navegación, tema y CI. Este repositorio hoy solo tiene documentación; la fase 1 agrega el código base.

**Tareas**

- [ ] **F1-T1** Crear el proyecto en una carpeta temporal con `npx create-expo-app@latest` (plantilla con TypeScript y Expo Router) y copiar su contenido a este repositorio sin pisar README, LICENSE ni los archivos de comunidad.
- [ ] **F1-T2** Alinear versiones y herramientas con los repositorios hermanos (por ejemplo PasanakuYa): Expo SDK 58, `expo-sqlite`, `jest-expo`, ESLint, TypeScript estricto, `.editorconfig` y `.prettierrc`.
- [ ] **F1-T3** Crear la estructura `app/`, `src/db/`, `src/components/`, `src/lib/` y `src/theme.ts`.
- [ ] **F1-T4** Implementar `src/db/index.ts` (apertura de la base) y `src/db/schema.ts` con la migración 1 tomada de la sección 6 del [SPEC](SPEC.md).
- [ ] **F1-T5** Definir la navegación principal en `app/_layout.tsx` con una ruta por sección.
- [ ] **F1-T6** Agregar el componente `Pantalla`, el tema y la pantalla de inicio con accesos a cada sección.
- [ ] **F1-T7** Configurar scripts `lint`, `test`, `build:apk` y el workflow de GitHub Actions (`.github/workflows/ci.yml`).
- [ ] **F1-T8** Escribir una primera prueba de `src/lib` para validar que Jest funciona.

**Criterio de salida:** la app abre, navega entre secciones vacías y no da errores; lint, tipos y tests en verde.

---

## Fase 2 — Vehículos, conductores y movimientos

**Estimación:** 5 días. **Rama sugerida:** `fase-2-vehiculos-conductores-y-movimientos`.

**Objetivo:** Vehículos, conductores y movimientos.

**Historias que se completan:** HU-01 (Registrar vehículos y conductores), HU-02 (Registrar ingresos y gastos).

**Tareas**

- [ ] **F2-T1** CRUD de vehículos (placa única) y conductores.
- [ ] **F2-T2** Asignaciones con regla de un conductor vigente por vehículo.
- [ ] **F2-T3** Formulario de movimientos con categorías base.
- [ ] **F2-T4** Historial de movimientos por vehículo y por conductor.
- [ ] **F2-T5** Pruebas de validación de montos y asignaciones.
- [ ] **F2-T6** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F2-T7** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F2-T8** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-01 — un vehículo libre → lo asigno a un conductor con 50% → queda vigente la asignación con su porcentaje.
- [ ] HU-01 — un vehículo con conductor vigente → intento asignar otro → la app pide cerrar la asignación actual primero.
- [ ] HU-02 — un gasto de combustible de 200 Bs → lo registro con la categoría y el vehículo → aparece en el historial del vehículo.
- [ ] HU-02 — un monto menor o igual a 0 → intento guardar → la app lo rechaza.

**Criterio de salida:**

- [ ] Puedo cargar 5 vehículos, 5 conductores y 30 movimientos y verlos tras reiniciar.
- [ ] Definición de «terminado» cumplida.

---

## Fase 3 — Liquidación semanal

**Estimación:** 4 días. **Rama sugerida:** `fase-3-liquidacion-semanal`.

**Objetivo:** Liquidación semanal.

**Historias que se completan:** HU-03 (Liquidación semanal por conductor).

**Tareas**

- [ ] **F3-T1** Función pura de liquidación (ingresos × porcentaje − gastos a cargo) con pruebas.
- [ ] **F3-T2** Arrastre de saldo negativo a la semana siguiente.
- [ ] **F3-T3** Pantalla de liquidación con detalle y semana seleccionable.
- [ ] **F3-T4** Cierre de liquidación (solo lectura) y compartir en PDF.
- [ ] **F3-T5** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F3-T6** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F3-T7** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-03 — ingresos de 2.000 Bs con 50% y 150 Bs de combustible a cargo del conductor → genero la liquidación → el total es 850 Bs y se muestra el detalle.
- [ ] HU-03 — una liquidación cerrada → intento editarla → solo puedo consultarla.

**Criterio de salida:**

- [ ] La liquidación coincide con 4 casos de prueba calculados a mano.
- [ ] Definición de «terminado» cumplida.

---

## Fase 4 — Reportes de rentabilidad

**Estimación:** 3 días. **Rama sugerida:** `fase-4-reportes-de-rentabilidad`.

**Objetivo:** Reportes de rentabilidad.

**Historias que se completan:** HU-04 (Rentabilidad por vehículo).

**Tareas**

- [ ] **F4-T1** Consultas de ingresos, gastos y margen por vehículo y período.
- [ ] **F4-T2** Pantalla Rentabilidad con ranking.
- [ ] **F4-T3** Desglose por categoría de gasto.
- [ ] **F4-T4** Filtros: semana, mes y rango libre.
- [ ] **F4-T5** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F4-T6** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F4-T7** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-04 — un período elegido → abro Rentabilidad → veo cada vehículo con ingresos, gastos, resultado y margen ordenados de mayor a menor.

**Criterio de salida:**

- [ ] Las sumas por período coinciden con los movimientos.
- [ ] Definición de «terminado» cumplida.

---

## Fase 5 — Recordatorios y exportación

**Estimación:** 4 días. **Rama sugerida:** `fase-5-recordatorios-y-exportacion`.

**Objetivo:** Recordatorios y exportación.

**Historias que se completan:** HU-05 (Recordatorios de mantenimiento y documentos), HU-06 (Exportar para contabilidad).

**Tareas**

- [ ] **F5-T1** Vencimientos de mantenimiento y documentos por vehículo.
- [ ] **F5-T2** Notificaciones locales con expo-notifications.
- [ ] **F5-T3** Exportación CSV de movimientos y liquidaciones con expo-file-system y expo-sharing.
- [ ] **F5-T4** Pruebas del generador CSV.
- [ ] **F5-T5** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F5-T6** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F5-T7** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-05 — un SOAT que vence en 30 días → llega la fecha del aviso → recibo una notificación local con el vehículo.
- [ ] HU-06 — movimientos del mes → toco «Exportar CSV» → se genera un archivo con una fila por movimiento y sus categorías.

**Criterio de salida:**

- [ ] Los avisos llegan en un teléfono real y el CSV abre bien en Excel.
- [ ] Definición de «terminado» cumplida.

---

## Lanzamiento del MVP (v1.0.0)

- [ ] Revisar todas las historias de [SPEC.md](SPEC.md) y marcar sus criterios.
- [ ] Capturas de pantalla para el README.
- [ ] Revisar textos, íconos y nombre de la app en `app.json`.
- [ ] `npm run build:apk` y probar el APK en un teléfono limpio, sin Expo Go y en modo avión.
- [ ] Publicar el release v1.0.0 con el APK adjunto.
- [ ] Pasar las ideas pendientes a issues para la versión 1.1.

## Plantilla de issue

Copiá esto al crear cada issue:

```
Título: [F2-T1] <descripción de la tarea>
Fase: N — <nombre>
Historia: HU-XX
Criterios:
- [ ] ...
Notas técnicas: ...
```

---

## Cronograma día por día

Guía para no perder el hilo: cada día tiene tareas concretas. Si un día se atrasa, mové la tarea al siguiente, no la saltees.

### Fase 1 — Estructura base y navegación

- **Día 1** (día 1 de la fase): `F1-T1` Crear el proyecto Expo con TypeScript y Expo Router; `F1-T2` Alinear versiones y herramientas con los repos hermanos; `F1-T3` Crear la estructura de carpetas.
- **Día 2** (día 2 de la fase): `F1-T4` Implementar apertura de base y migración 1; `F1-T5` Definir la navegación principal; `F1-T6` Agregar componente Pantalla, tema e inicio.
- **Día 3** (día 3 de la fase): `F1-T7` Configurar scripts y CI; `F1-T8` Primera prueba de src/lib.

### Fase 2 — Vehículos, conductores y movimientos

- **Día 4** (día 1 de la fase): `F2-T1` CRUD de vehículos (placa única) y conductores.; `F2-T2` Asignaciones con regla de un conductor vigente por vehículo..
- **Día 5** (día 2 de la fase): `F2-T3` Formulario de movimientos con categorías base.; `F2-T4` Historial de movimientos por vehículo y por conductor..
- **Día 6** (día 3 de la fase): `F2-T5` Pruebas de validación de montos y asignaciones.; `F2-T6` Validaciones, estados vacíos y mensajes en español.
- **Día 7** (día 4 de la fase): `F2-T7` Pruebas automáticas y verificación manual de criterios; `F2-T8` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.
- **Día 8** (día 5 de la fase): colchón: terminar pendientes, probar en el teléfono y ajustar detalles.

### Fase 3 — Liquidación semanal

- **Día 9** (día 1 de la fase): `F3-T1` Función pura de liquidación (ingresos × porcentaje − gastos a cargo) con pruebas.; `F3-T2` Arrastre de saldo negativo a la semana siguiente..
- **Día 10** (día 2 de la fase): `F3-T3` Pantalla de liquidación con detalle y semana seleccionable.; `F3-T4` Cierre de liquidación (solo lectura) y compartir en PDF..
- **Día 11** (día 3 de la fase): `F3-T5` Validaciones, estados vacíos y mensajes en español; `F3-T6` Pruebas automáticas y verificación manual de criterios.
- **Día 12** (día 4 de la fase): `F3-T7` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

### Fase 4 — Reportes de rentabilidad

- **Día 13** (día 1 de la fase): `F4-T1` Consultas de ingresos, gastos y margen por vehículo y período.; `F4-T2` Pantalla Rentabilidad con ranking.; `F4-T3` Desglose por categoría de gasto..
- **Día 14** (día 2 de la fase): `F4-T4` Filtros: semana, mes y rango libre.; `F4-T5` Validaciones, estados vacíos y mensajes en español; `F4-T6` Pruebas automáticas y verificación manual de criterios.
- **Día 15** (día 3 de la fase): `F4-T7` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

### Fase 5 — Recordatorios y exportación

- **Día 16** (día 1 de la fase): `F5-T1` Vencimientos de mantenimiento y documentos por vehículo.; `F5-T2` Notificaciones locales con expo-notifications..
- **Día 17** (día 2 de la fase): `F5-T3` Exportación CSV de movimientos y liquidaciones con expo-file-system y expo-sharing.; `F5-T4` Pruebas del generador CSV..
- **Día 18** (día 3 de la fase): `F5-T5` Validaciones, estados vacíos y mensajes en español; `F5-T6` Pruebas automáticas y verificación manual de criterios.
- **Día 19** (día 4 de la fase): `F5-T7` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

## Rutina diaria

1. Empezá releyendo la tarea del día en [BACKLOG.md](BACKLOG.md) y su regla o historia en [SPEC.md](SPEC.md).
2. Escribí primero la prueba de la lógica, después el código.
3. Probá en el teléfono antes de hacer commit.
4. Cerrá el día con un commit, el issue marcado y una línea en el CHANGELOG si corresponde.

## Qué hacer si te trabás

- Más de 2 horas sin avanzar: simplificá la tarea al caso mínimo y anotá lo que quedó fuera como issue `mejora`.
- Duda de alcance: la respuesta está en la sección 2 del [SPEC](SPEC.md); si no está, no entra al MVP.
- Duda de datos o validaciones: [DATOS.md](DATOS.md).
- Duda de pantallas: [PANTALLAS.md](PANTALLAS.md).
- Duda de cómo probar: [PRUEBAS.md](PRUEBAS.md).
