# Roadmap de AutoCuenta

> Que el dueño sepa cuánto gana cada vehículo y cuánto le corresponde a cada conductor cada semana, sin discusiones.

Ruta de desarrollo del MVP, fase por fase. Cada fase termina con algo que se puede **usar y probar en un teléfono**. Trabajá una fase a la vez y no pases a la siguiente hasta cumplir su criterio de salida.

## Resumen

- **Objetivo del MVP:** Control de ingresos y gastos por vehículo y conductor, con liquidación semanal y exportación CSV.
- **Usuario:** Dueño de 1 a 20 vehículos con conductores a porcentaje o a cuota, que liquida semanalmente.
- **Duración estimada:** ~19 días de trabajo efectivo (~4 semanas a ritmo tranquilo).
- **Costo:** $0 (sin servidores, sin suscripciones, todo offline en SQLite).

## Línea de tiempo

| Fase | Nombre | Objetivo | Días | Semana acumulada |
|---|---|---|---|---|
| 1 | Estructura base y navegación | Crear el proyecto Expo con base de datos, navegación, tema y CI. | 3 | 1 |
| 2 | Vehículos, conductores y movimientos | Vehículos, conductores y movimientos. | 5 | 2 |
| 3 | Liquidación semanal | Liquidación semanal. | 4 | 3 |
| 4 | Reportes de rentabilidad | Reportes de rentabilidad. | 3 | 3 |
| 5 | Recordatorios y exportación | Recordatorios y exportación. | 4 | 4 |

## Hitos

### Hito 1 — Estructura base y navegación

**Demo:** La app abre en Expo Go, navega entre secciones vacías y el CI pasa en verde.

### Hito 2 — Vehículos, conductores y movimientos

**Demo:** Puedo cargar 5 vehículos, 5 conductores y 30 movimientos y verlos tras reiniciar.

### Hito 3 — Liquidación semanal

**Demo:** La liquidación coincide con 4 casos de prueba calculados a mano.

### Hito 4 — Reportes de rentabilidad

**Demo:** Las sumas por período coinciden con los movimientos.

### Hito 5 — Recordatorios y exportación

**Demo:** Los avisos llegan en un teléfono real y el CSV abre bien en Excel.

## Detalle por fase

### Fase 1 — Estructura base y navegación

- **Objetivo:** dejar el proyecto creado y funcionando con la base de datos y la navegación.
- **Historias cubiertas:** ninguna (infraestructura).
- **Depende de:** nada.
- **Criterio de salida:** la app abre, navega y el CI está en verde.

### Fase 2 — Vehículos, conductores y movimientos

- **Objetivo:** Vehículos, conductores y movimientos.
- **Historias cubiertas:** HU-01 Registrar vehículos y conductores; HU-02 Registrar ingresos y gastos
- **Depende de:** fase 1.
- **Duración:** 5 días.
- **Criterio de salida:**
  - Puedo cargar 5 vehículos, 5 conductores y 30 movimientos y verlos tras reiniciar.

### Fase 3 — Liquidación semanal

- **Objetivo:** Liquidación semanal.
- **Historias cubiertas:** HU-03 Liquidación semanal por conductor
- **Depende de:** fase 2.
- **Duración:** 4 días.
- **Criterio de salida:**
  - La liquidación coincide con 4 casos de prueba calculados a mano.

### Fase 4 — Reportes de rentabilidad

- **Objetivo:** Reportes de rentabilidad.
- **Historias cubiertas:** HU-04 Rentabilidad por vehículo
- **Depende de:** fase 3.
- **Duración:** 3 días.
- **Criterio de salida:**
  - Las sumas por período coinciden con los movimientos.

### Fase 5 — Recordatorios y exportación

- **Objetivo:** Recordatorios y exportación.
- **Historias cubiertas:** HU-05 Recordatorios de mantenimiento y documentos; HU-06 Exportar para contabilidad
- **Depende de:** fase 4.
- **Duración:** 4 días.
- **Criterio de salida:**
  - Los avisos llegan en un teléfono real y el CSV abre bien en Excel.

## Estado

- [ ] **Fase 1:** Estructura base y navegación
- [ ] **Fase 2:** Vehículos, conductores y movimientos
- [ ] **Fase 3:** Liquidación semanal
- [ ] **Fase 4:** Reportes de rentabilidad
- [ ] **Fase 5:** Recordatorios y exportación

## Dependencias y supuestos

- Un teléfono Android real para probar (Expo Go) y, al final, para instalar el APK.
- Node.js 20+ y la app Expo Go.
- Todo lo demás (cuentas, nube, pagos) queda fuera del MVP.

## Después del MVP (v1.1 en adelante)

- [ ] Modo oscuro.
- [ ] Búsqueda y filtros avanzados en las listas.
- [ ] Estadísticas y gráficos simples en la pantalla de inicio.
- [ ] Publicación en Google Play (cuenta de desarrollador, pago único de USD 25).
- [ ] Respaldo y restauración de la base de datos.
- [ ] Sincronización opcional entre dispositivos (solo si hay demanda real).

## Cómo usar este documento

1. [SPEC.md](SPEC.md) define **qué** debe hacer la app (historias, reglas, pantallas, datos).
2. [PLANNING.md](PLANNING.md) define **cómo** construirla fase por fase (tareas, pruebas, issues).
3. Al cerrar una fase: marcá el estado acá, actualizá el [CHANGELOG](../CHANGELOG.md) y fusioná la rama a `main`.

## Trazabilidad: historias por fase

| Historia | Fase | Entrega |
|---|---|---|
| HU-01 Registrar vehículos y conductores | 2 | Vehículos, conductores y movimientos |
| HU-02 Registrar ingresos y gastos | 2 | Vehículos, conductores y movimientos |
| HU-03 Liquidación semanal por conductor | 3 | Liquidación semanal |
| HU-04 Rentabilidad por vehículo | 4 | Reportes de rentabilidad |
| HU-05 Recordatorios de mantenimiento y documentos | 5 | Recordatorios y exportación |
| HU-06 Exportar para contabilidad | 5 | Recordatorios y exportación |

## Indicadores de avance

| Indicador | Cómo se mide | Meta |
|---|---|---|
| Avance de tareas | Issues cerrados / 37 | 100 % al lanzar |
| Historias aceptadas | Casos CP- marcados ✔ / 9 | 100 % |
| Reglas cubiertas | RN con prueba / 7 | 100 % |
| Ritmo | Días reales vs estimados por fase | Desvío menor al 50 % |
| Calidad | Errores abiertos al cierre de fase | 0 críticos |

## Revisión de fin de fase

Antes de pasar a la siguiente fase respondé por escrito (5 minutos):

1. ¿Se cumplieron todos los criterios de salida?
2. ¿Cuántos días tomó realmente frente a lo estimado?
3. ¿Qué se recortó o se dejó para después?
4. ¿Hay algo que cambie el SPEC? Actualizalo antes de seguir.

## Documentos de apoyo

- [DATOS.md](DATOS.md): campos, validaciones, consultas y migraciones.
- [PANTALLAS.md](PANTALLAS.md): ruta, estados y bosquejo de cada pantalla.
- [PRUEBAS.md](PRUEBAS.md): casos de prueba por regla, historia y borde.
- [BACKLOG.md](BACKLOG.md): issues listos para GitHub con milestones y etiquetas.
