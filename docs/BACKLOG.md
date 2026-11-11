# Backlog de issues — AutoCuenta

> Lista lista para pasar a GitHub Issues, con milestones, etiquetas, estimaciones y trazabilidad a las historias. Se puede crear todo de una vez con [crear-issues.sh](crear-issues.sh).

## 1. Milestones

| Milestone | Fase | Días | Tareas | Horas est. |
|---|---|---|---|---|
| Fase 1 — Estructura base y navegación | 1 | 3 | 8 | 20 |
| Fase 2 — Vehículos, conductores y movimientos | 2 | 5 | 8 | 32 |
| Fase 3 — Liquidación semanal | 3 | 4 | 7 | 24.5 |
| Fase 4 — Reportes de rentabilidad | 4 | 3 | 7 | 17.5 |
| Fase 5 — Recordatorios y exportación | 5 | 4 | 7 | 24.5 |

Total: **37 tareas**, ~119 horas (19 días a 6 horas efectivas).

## 2. Etiquetas

| Etiqueta | Uso |
|---|---|
| ui | Pantallas y componentes |
| datos | Tablas, consultas y migraciones |
| lógica | Reglas de negocio y cálculos |
| pruebas | Pruebas automáticas y manuales |
| integración | Cámara, notificaciones, PDF, compartir |
| tarea | Otras tareas |
| bug | Errores |
| mejora | Ideas para v1.1+ |

## 3. Issues del MVP

### Fase 1 — Estructura base y navegación

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F1-T1` | Crear el proyecto Expo con TypeScript y Expo Router | — | tarea | Alta | 2.5 |
| `F1-T2` | Alinear versiones y herramientas con los repos hermanos | — | tarea | Alta | 2.5 |
| `F1-T3` | Crear la estructura de carpetas | — | tarea | Alta | 2.5 |
| `F1-T4` | Implementar apertura de base y migración 1 | — | datos | Alta | 2.5 |
| `F1-T5` | Definir la navegación principal | — | ui | Alta | 2.5 |
| `F1-T6` | Agregar componente Pantalla, tema e inicio | — | ui | Alta | 2.5 |
| `F1-T7` | Configurar scripts y CI | — | tarea | Alta | 2.5 |
| `F1-T8` | Primera prueba de src/lib | — | pruebas | Alta | 2.5 |

### Fase 2 — Vehículos, conductores y movimientos

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F2-T1` | CRUD de vehículos (placa única) y conductores. | HU-01, HU-02 | tarea | Alta | 4 |
| `F2-T2` | Asignaciones con regla de un conductor vigente por vehículo. | HU-01, HU-02 | lógica | Alta | 4 |
| `F2-T3` | Formulario de movimientos con categorías base. | HU-01, HU-02 | ui, datos | Alta | 4 |
| `F2-T4` | Historial de movimientos por vehículo y por conductor. | HU-01, HU-02 | tarea | Alta | 4 |
| `F2-T5` | Pruebas de validación de montos y asignaciones. | HU-01, HU-02 | lógica, pruebas | Alta | 4 |
| `F2-T6` | Validaciones, estados vacíos y mensajes en español | HU-01, HU-02 | lógica | Alta | 4 |
| `F2-T7` | Pruebas automáticas y verificación manual de criterios | HU-01, HU-02 | pruebas | Alta | 4 |
| `F2-T8` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-01, HU-02 | pruebas | Alta | 4 |

### Fase 3 — Liquidación semanal

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F3-T1` | Función pura de liquidación (ingresos × porcentaje − gastos a cargo) con pruebas. | HU-03 | lógica, pruebas | Alta | 3.5 |
| `F3-T2` | Arrastre de saldo negativo a la semana siguiente. | HU-03 | tarea | Alta | 3.5 |
| `F3-T3` | Pantalla de liquidación con detalle y semana seleccionable. | HU-03 | ui | Alta | 3.5 |
| `F3-T4` | Cierre de liquidación (solo lectura) y compartir en PDF. | HU-03 | integración | Alta | 3.5 |
| `F3-T5` | Validaciones, estados vacíos y mensajes en español | HU-03 | lógica | Alta | 3.5 |
| `F3-T6` | Pruebas automáticas y verificación manual de criterios | HU-03 | pruebas | Alta | 3.5 |
| `F3-T7` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-03 | pruebas | Alta | 3.5 |

### Fase 4 — Reportes de rentabilidad

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F4-T1` | Consultas de ingresos, gastos y margen por vehículo y período. | HU-04 | datos | Media | 2.5 |
| `F4-T2` | Pantalla Rentabilidad con ranking. | HU-04 | ui | Media | 2.5 |
| `F4-T3` | Desglose por categoría de gasto. | HU-04 | tarea | Media | 2.5 |
| `F4-T4` | Filtros: semana, mes y rango libre. | HU-04 | tarea | Media | 2.5 |
| `F4-T5` | Validaciones, estados vacíos y mensajes en español | HU-04 | lógica | Media | 2.5 |
| `F4-T6` | Pruebas automáticas y verificación manual de criterios | HU-04 | pruebas | Media | 2.5 |
| `F4-T7` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-04 | pruebas | Media | 2.5 |

### Fase 5 — Recordatorios y exportación

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F5-T1` | Vencimientos de mantenimiento y documentos por vehículo. | HU-05, HU-06 | tarea | Media | 3.5 |
| `F5-T2` | Notificaciones locales con expo-notifications. | HU-05, HU-06 | integración | Media | 3.5 |
| `F5-T3` | Exportación CSV de movimientos y liquidaciones con expo-file-system y expo-sharing. | HU-05, HU-06 | integración | Media | 3.5 |
| `F5-T4` | Pruebas del generador CSV. | HU-05, HU-06 | pruebas, integración | Media | 3.5 |
| `F5-T5` | Validaciones, estados vacíos y mensajes en español | HU-05, HU-06 | lógica | Media | 3.5 |
| `F5-T6` | Pruebas automáticas y verificación manual de criterios | HU-05, HU-06 | pruebas | Media | 3.5 |
| `F5-T7` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-05, HU-06 | pruebas | Media | 3.5 |

## 4. Orden recomendado de ataque

1. Terminá cada fase completa antes de abrir la siguiente; dentro de la fase, de arriba hacia abajo.
2. Las tareas `lógica` y `datos` van antes que las `ui` que las usan.
3. Cada issue se cierra con un commit o PR que lo mencione (`Cierra #N`).
4. Nada de abrir issues de v1.1 hasta tener el APK de la v1.0.0.

## 5. Ideas para después del MVP

| Idea | Esfuerzo | Valor |
|---|---|---|
| Modo oscuro | Bajo | Medio |
| Búsqueda y filtros avanzados | Medio | Alto |
| Estadísticas y gráficos simples en inicio | Medio | Medio |
| Respaldo y restauración de la base | Medio | Alto |
| Publicación en Google Play | Bajo | Alto |
| Sincronización opcional entre dispositivos | Alto | Depende de demanda |
