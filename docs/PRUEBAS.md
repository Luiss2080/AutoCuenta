# Plan de pruebas — AutoCuenta

> Qué probar, cuándo y cómo. Cada historia y cada regla de negocio tiene casos concretos. Complementa a [SPEC.md](SPEC.md).

## 1. Estrategia

| Nivel | Herramienta | Qué cubre | Cuándo |
|---|---|---|---|
| Unitarias | jest-expo (`npm test`) | Lógica pura de `src/lib/` (cálculos, validaciones, formatos) | En cada commit (CI) |
| Manuales por historia | Teléfono con Expo Go | Criterios de aceptación de SPEC (casos CP-) | Al cerrar cada tarea de pantalla |
| Regresión por fase | Teléfono real | Lo ya terminado sigue andando | Al cerrar cada fase |
| Aceptación final | APK en teléfono limpio, modo avión | Criterios de lanzamiento | Antes de v1.0.0 |

Meta de cobertura: **100 % de las reglas de negocio** con prueba automática o caso manual, y **100 % de los criterios de aceptación** ejecutados al menos una vez en un teléfono real.

## 2. Pruebas unitarias por regla de negocio

Crear un archivo `src/lib/<tema>.test.ts` por tema. Una prueba por regla como mínimo; agregar los bordes de la sección 4.

| ID | Regla | Tipo de prueba | Casos mínimos |
|---|---|---|---|
| RN-01 | Un movimiento es de tipo ingreso o gasto y siempre pertenece a un vehículo; el conductor es opcional. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-02 | Un vehículo tiene a lo sumo un conductor asignado vigente a la vez. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-03 | Categorías base de gasto: combustible, mantenimiento, repuestos, lavado, multas, documentos, otros. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-04 | Liquidación del conductor = ingresos del período × porcentaje del conductor − gastos marcados «a cargo del conductor». | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-05 | Si la liquidación es negativa, queda como saldo a favor del dueño y se arrastra a la semana siguiente. | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-06 | Una liquidación cerrada no se edita; los errores se corrigen con un movimiento de ajuste en la semana siguiente. | Unitaria (fechas) | Hoy, mañana, fin de mes, año bisiesto |
| RN-07 | Rentabilidad del vehículo = ingresos − gastos del período; margen = rentabilidad ÷ ingresos. | Unitaria o manual | Caso válido, caso inválido, límite |

## 3. Casos manuales por historia

Formato: ejecutar en el teléfono, marcar ✔ o ✘ y anotar la fecha. Si falla, abrir un issue con el ID del caso.

### HU-01 — Registrar vehículos y conductores (fase 2)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-01-1 | un vehículo libre | lo asigno a un conductor con 50% | queda vigente la asignación con su porcentaje | ☐ |
| CP-01-2 | un vehículo con conductor vigente | intento asignar otro | la app pide cerrar la asignación actual primero | ☐ |

Casos transversales: **CP-01-T1** modo avión · **CP-01-T2** cerrar y reabrir la app (persistencia) · **CP-01-T3** texto del sistema en tamaño grande.

### HU-02 — Registrar ingresos y gastos (fase 2)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-02-1 | un gasto de combustible de 200 Bs | lo registro con la categoría y el vehículo | aparece en el historial del vehículo | ☐ |
| CP-02-2 | un monto menor o igual a 0 | intento guardar | la app lo rechaza | ☐ |

Casos transversales: **CP-02-T1** modo avión · **CP-02-T2** cerrar y reabrir la app (persistencia) · **CP-02-T3** texto del sistema en tamaño grande.

### HU-03 — Liquidación semanal por conductor (fase 3)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-03-1 | ingresos de 2.000 Bs con 50% y 150 Bs de combustible a cargo del conductor | genero la liquidación | el total es 850 Bs y se muestra el detalle | ☐ |
| CP-03-2 | una liquidación cerrada | intento editarla | solo puedo consultarla | ☐ |

Casos transversales: **CP-03-T1** modo avión · **CP-03-T2** cerrar y reabrir la app (persistencia) · **CP-03-T3** texto del sistema en tamaño grande.

### HU-04 — Rentabilidad por vehículo (fase 4)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-04-1 | un período elegido | abro Rentabilidad | veo cada vehículo con ingresos, gastos, resultado y margen ordenados de mayor a menor | ☐ |

Casos transversales: **CP-04-T1** modo avión · **CP-04-T2** cerrar y reabrir la app (persistencia) · **CP-04-T3** texto del sistema en tamaño grande.

### HU-05 — Recordatorios de mantenimiento y documentos (fase 5)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-05-1 | un SOAT que vence en 30 días | llega la fecha del aviso | recibo una notificación local con el vehículo | ☐ |

Casos transversales: **CP-05-T1** modo avión · **CP-05-T2** cerrar y reabrir la app (persistencia) · **CP-05-T3** texto del sistema en tamaño grande.

### HU-06 — Exportar para contabilidad (fase 5)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-06-1 | movimientos del mes | toco «Exportar CSV» | se genera un archivo con una fila por movimiento y sus categorías | ☐ |

Casos transversales: **CP-06-T1** modo avión · **CP-06-T2** cerrar y reabrir la app (persistencia) · **CP-06-T3** texto del sistema en tamaño grande.

## 4. Casos borde

| ID | Caso | Resultado esperado | Resultado |
|---|---|---|---|
| CB-01 | Conductor que cambia de vehículo a mitad de semana: la liquidación suma por movimiento, no por asignación vigente. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-02 | Gasto compartido entre dos vehículos: registrar un movimiento por vehículo. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-03 | Porcentaje que cambia a mitad de período: aplica desde la fecha de la nueva asignación. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-04 | Vehículo vendido: se archiva sin perder historial. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-05 | Movimiento con fecha futura: se advierte pero se permite. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |

## 5. Pruebas de datos y migraciones

| ID | Prueba | Esperado |
|---|---|---|
| CD-01 | Primer arranque en teléfono limpio | La base se crea con todas las tablas y `user_version` queda en 1 |
| CD-02 | Actualizar de una versión anterior con datos cargados | Las migraciones nuevas se aplican y no se pierde ningún registro |
| CD-03 | Borrar un registro padre | Los hijos se borran o se bloquea con mensaje, según el esquema |
| CD-04 | Intentar guardar con campos obligatorios vacíos | No se inserta nada y se marcan los campos |
| CD-05 | Cargar 1.000 registros de prueba | Las listas siguen fluidas (scroll sin trabas) y abren en menos de 2 s |

## 6. Regresión al cerrar cada fase

**Fase 1 — Estructura base y navegación**

- [ ] La app abre y navega sin errores

**Fase 2 — Vehículos, conductores y movimientos**

- [ ] Puedo cargar 5 vehículos, 5 conductores y 30 movimientos y verlos tras reiniciar.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 3 — Liquidación semanal**

- [ ] La liquidación coincide con 4 casos de prueba calculados a mano.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 4 — Reportes de rentabilidad**

- [ ] Las sumas por período coinciden con los movimientos.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 5 — Recordatorios y exportación**

- [ ] Los avisos llegan en un teléfono real y el CSV abre bien en Excel.
- [ ] Los criterios de las fases anteriores siguen funcionando

## 7. Matriz de dispositivos

| Dispositivo | Android | Qué se verifica |
|---|---|---|
| Gama baja (2–3 GB de RAM) | 8–10 | Fluidez de listas, tiempo de arranque |
| Gama media | 11–13 | Flujo completo y APK |
| Pantalla pequeña (5") | cualquiera | Que nada quede cortado ni oculto por el teclado |
| Texto del sistema grande | cualquiera | Que los formularios sigan siendo usables |

## 8. Lista de verificación para el lanzamiento (v1.0.0)

- [ ] Todas las historias ejecutadas en teléfono real (casos CP-)
- [ ] Todos los casos borde (CB-) ejecutados
- [ ] Pruebas de datos (CD-) ejecutadas
- [ ] APK instalado en un teléfono limpio sin Expo Go
- [ ] Probado en modo avión durante todo el flujo principal
- [ ] Rotación de pantalla no pierde datos del formulario
- [ ] Permisos solicitados solo cuando se necesitan y con explicación
- [ ] Sin errores en la consola durante un recorrido completo
- [ ] README con capturas y CHANGELOG al día

## 9. Informe de errores

```
Título: [Bug] <qué pasó>
ID de caso: CP-XX-X
Pasos: 1... 2... 3...
Esperado: ...
Obtenido: ...
Dispositivo / Android: ...
Captura o video: ...
```
