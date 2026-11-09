<div align="center">

<img src="docs/assets/logo.svg" alt="AutoCuenta" width="96" height="96"/>

# AutoCuenta

**Control de ingresos y gastos por vehículo y conductor, con liquidación semanal y exportación CSV. Pensado para flotas pequeñas, offline.**

<p>
<img src="https://img.shields.io/badge/Expo-000020?style=for-the-badge&logo=expo&logoColor=white" alt="Expo"/>
<img src="https://img.shields.io/badge/React_Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React Native"/>
<img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript"/>
<img src="https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQLite"/>
<img src="https://img.shields.io/badge/Licencia-MIT-green?style=for-the-badge" alt="MIT"/>
</p>

[🚀 Características](#-características) · [🧰 Stack](#-stack) · [📂 Estructura](#-estructura) · [🔧 Configuración](#-configuración) · [🧪 Pruebas](#-pruebas) · [🤝 Contribuir](#-contribuir) · [📄 Licencia](#-licencia)

</div>

---

## 🚀 Características

- **Vehículos y conductores** con asignaciones.
- **Ingresos y gastos** por unidad (combustible, mantenimiento, etc.).
- **Liquidación semanal** por conductor.
- **Rentabilidad por vehículo** en el período.
- **Recordatorios** de mantenimiento y documentos.
- **Exportación CSV** para contabilidad.

> Sin servidores, sin suscripciones y sin servicios de pago: todos los datos viven en el dispositivo (SQLite).

## 🧰 Stack

| Capa | Tecnología |
| --- | --- |
| App | React Native + Expo (TypeScript) |
| Datos | SQLite local (`expo-sqlite`) |
| Navegación | Expo Router |
| Distribución | APK / EAS Build local |

### Modelo de datos

- `vehiculos (id, placa, modelo, anio)`
- `conductores (id, nombre, telefono)`
- `movimientos (id, vehiculo_id, conductor_id, tipo, categoria, monto, fecha)`
- `liquidaciones (id, conductor_id, desde, hasta, total)`

## 🗺️ Roadmap

- [ ] 1. Estructura base y navegación
- [ ] 2. Vehículos, conductores y movimientos
- [ ] 3. Liquidación semanal
- [ ] 4. Reportes de rentabilidad
- [ ] 5. Recordatorios y exportación

## 📂 Estructura

```text
AutoCuenta/
├── app/            # Pantallas y rutas (Expo Router)
├── src/
│   ├── db/         # Esquema y consultas SQLite
│   ├── components/ # Componentes reutilizables
│   └── lib/        # Utilidades
├── docs/assets/    # Logo y recursos del README
└── .github/        # Plantillas, CI y Dependabot
```

## 🔧 Configuración

Requisitos: Node.js 20+ y la app Expo Go (o un emulador).

```bash
git clone https://github.com/Luiss2080/AutoCuenta.git
cd AutoCuenta
npm install
npx expo start
```

> El repositorio está en fase inicial: la estructura y la documentación están listas y el código de la app se construye siguiendo el roadmap.

## 🧪 Pruebas

```bash
npm test
npx tsc --noEmit
```

## 🤝 Contribuir

Lee la [guía de contribución](CONTRIBUTING.md) y el [código de conducta](CODE_OF_CONDUCT.md). Para reportar vulnerabilidades consulta la [política de seguridad](SECURITY.md); para dudas, [SUPPORT.md](SUPPORT.md).

## 📄 Licencia

Distribuido bajo licencia MIT. Consulta [LICENSE](LICENSE).

---

<div align="center">

Hecho por [Luiss2080](https://github.com/Luiss2080)

</div>
