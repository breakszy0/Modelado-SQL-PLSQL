# AquaChile · MVP Web de Evaluación Psicolaboral

Proyecto académico — **DSY1104 Full Stack II (Duoc UC)**.
Aplicación web Full Stack para gestionar el ciclo de una evaluación psicolaboral:
registrar candidatos → crear solicitudes → asignar evaluador → actualizar estado → registrar evaluación → consultar dashboard.

> ⚠️ **MVP académico con datos 100 % ficticios.** No es un sistema productivo ni oficial de AquaChile.
> No se usan ni deben subirse datos, CVs, credenciales o información interna reales.

## Arquitectura

```
React (CRA)  ──HTTP/JSON + JWT──▶  Node.js + Express (monolito)  ──SQL──▶  SQLite (backend/data/aquachile.db)
  frontend/                          backend/src/routes, middleware           archivo local, se crea solo
```

| Capa | Tecnología |
|---|---|
| Frontend | React 18, React Router 6, CSS propio |
| Backend | Node.js, Express 4, JWT, bcryptjs |
| Base de datos | SQLite (better-sqlite3) |

Fuera del alcance (según el caso): Power Automate, Copilot, Planner, OneDrive/SharePoint, microservicios.

## Estructura

```
├── backend/
│   └── src/  server.js · app.js · db.js · schema.sql · seed.js · utils.js
│            middleware/ (auth, errores) · routes/ (auth, candidatos, solicitudes, dashboard, catalogos)
├── frontend/
│   └── src/  services/api.js · context/AuthContext.jsx · pages/ · components/ · utils/validaciones.js
└── README.md
```

## Cómo ejecutarlo

Requisitos: Node.js 18 o superior.

```bash
# Terminal 1 – API en http://localhost:4000
cd backend
cp .env.example .env
npm install
npm run dev

# Terminal 2 – Web en http://localhost:3000
cd frontend
cp .env.example .env
npm install
npm start
```

La primera vez el backend crea `backend/data/aquachile.db` y carga datos ficticios.
Para reiniciar los datos: detén el backend, borra ese archivo y vuelve a iniciarlo.

## Usuarios de prueba (contraseña: `demo1234`)

| Rol | Correo | Puede |
|---|---|---|
| Administrador | admin@aquachile.test | Todo |
| Analista de Reclutamiento | analista@aquachile.test | Crear/editar candidatos, crear solicitudes, consultar |
| Profesional Evaluador | evaluador1@aquachile.test, evaluador2@aquachile.test | Ver solo sus solicitudes asignadas, cambiar estado, registrar evaluación |
| Jefatura | jefatura@aquachile.test | Solo lectura: dashboard y listados |

## Requerimientos funcionales

| RF | Descripción | Dónde |
|---|---|---|
| RF01 | Ingreso con usuarios/roles simulados | Login + JWT |
| RF02 | Registrar candidatos | Candidatos → Nuevo |
| RF03 | Editar y consultar candidato | Candidatos → Editar |
| RF04 | Solicitud asociada a un candidato | Solicitudes → Nueva |
| RF05 | Cargo, familia, fecha y responsable | Formulario de solicitud |
| RF06 | Listado y detalle de solicitudes | Solicitudes / Detalle |
| RF07 | Filtros por estado, fecha, cargo, candidato | Solicitudes |
| RF08 | Actualizar estado | Detalle de solicitud |
| RF09 | Registrar evaluación | Detalle de solicitud |
| RF10 | Dashboard con indicadores y listado de postulantes | Dashboard |

### Reglas de negocio
- Estados: **Pendiente → En proceso → Finalizada** (no se retrocede).
- No se puede finalizar una solicitud sin evaluación registrada.
- Registrar una evaluación en una solicitud Pendiente la pasa a En proceso.
- Cada cambio de estado queda en `historial_estados` (trazabilidad).
- Los permisos se validan en el **backend** (HTTP 403), no solo en la interfaz.

## API

Endpoints, roles permitidos y reglas: ver [`backend/README.md`](backend/README.md).

## Escenarios de prueba del caso

| Escenario | Cómo reproducirlo |
|---|---|
| Solicitud pendiente | Entrar como Analista → Solicitudes → Nueva |
| Solicitud en proceso | Entrar como Evaluador → abrir su solicitud → «Pasar a En proceso» |
| Evaluación finalizada | Evaluador → guardar evaluación → «Pasar a Finalizada» |
| Formulario incompleto | Enviar cualquier formulario vacío: se muestran errores por campo |
| Búsqueda por estado | Dashboard o Solicitudes → filtro «Estado» |

## Límites del MVP
Sin carga de documentos ni CVs, sin recuperación de contraseña, sin administración de usuarios desde la interfaz
(se gestionan con el seed), SQLite local (para despliegue gratuito se recomienda migrar a PostgreSQL).
