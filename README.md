# 🏀 Evaluación Deportiva

Plugin de WordPress para evaluación integral de jugadores en clubes y academias deportivas multi-disciplina.

**Versión actual:** 2.2.0-beta
**Estado:** En desarrollo activo — ~72% completado (Fases 1-4 listas, Fase 5.2-6 en curso)
**Autor:** [Ilderim Digital Solutions](https://ilderim.mx)
**Licencia:** GPL v2 (código) + licenciamiento comercial de uso (ver [Licenciamiento](#-licenciamiento))

---

## 📋 ¿Qué es?

Sistema completo para que clubes deportivos (fútbol, básquetbol, y otras disciplinas) evalúen jugadores de forma estructurada, den seguimiento a su progreso y generen credenciales visuales tipo "baseball card" con foto sin fondo.

Actualmente buscando colaboradores para validar la funcionalidad

### Características principales

- **5 roles con permisos granulares:** Jugador, Coach, Entrenador, Director, Admin WP
- **Evaluaciones jerárquicas:** Evaluación → Categorías → Métricas, con 17 tipos de respuesta distintos
- **Flujo de captura ponderado:** puntuación 0–10 con pesos configurables por métrica
- **Registro de jugadores** con estados pendiente/aprobado/rechazado
- **Dashboards por rol** en frontend (sin exponer wp-admin a jugadores/coaches)
- **Estampa de jugador ("baseball card")** con foto de perfil, estilo capas CSS
- **Remoción de fondo de fotos** (servicio propio Rembg vía API REST) con degradación elegante si la API no está disponible
- **Conversión automática a WebP** para optimizar peso de imágenes

## 🏗️ Arquitectura

```
evaluacion-deportiva-plugin/
├── admin/                      # Pantallas de administración WP (settings, jugadores, equipos, etc.)
├── includes/                   # Lógica del plugin
│   ├── class-db-schema.php     # Definición e instalación del esquema (14+ tablas)
│   ├── class-evaluaciones.php  # Motor de evaluaciones y captura
│   ├── class-registro-jugadores.php
│   ├── class-roles-permisos.php
│   ├── class-badges.php        # Insignias automáticas
│   ├── class-import-export.php
│   ├── class-notificaciones.php
│   ├── class-auditoria.php
│   └── class-rembg.php         # Conector a la API de remoción de fondo
├── public/                     # Frontend (dashboards por rol, shortcodes)
├── templates/                  # Plantillas de reportes, emails, import/export
├── assets/                     # CSS/JS/imágenes
├── schema-evaluacion-deportiva-v2.0.sql
├── plugin.php
└── LICENSE
```

### Roles y permisos

| Rol | Puede |
|---|---|
| **Jugador** (`edwp_jugador`) | Ver su perfil, su historial de evaluaciones y subir su foto |
| **Coach** (`edwp_coach`) | Capturar evaluaciones asignadas |
| **Entrenador** (`edwp_entrenador`) | Crear evaluaciones, asignarlas, validarlas y definir la siguiente instancia |
| **Director** (`edwp_director`) | Gestionar staff, aprobar fotos, supervisar el club |
| **Admin WP** | Configuración global del plugin |

### Flujo de evaluación

```
Entrenador crea plantilla de evaluación
        ↓
Se asigna a un jugador (bajo demanda, no automático)
        ↓
Coach captura la evaluación (0–10 ponderado por métrica)
        ↓
Entrenador valida / ajusta
        ↓
Se define la siguiente instancia
```

## 💻 Requisitos

- WordPress 7.0+
- PHP 8.5+
- MariaDB 11.4.12+
- (Opcional) Servicio propio de remoción de fondo — ver [`rembg-api`](https://github.com/ilderim/rembg-api) para desplegar el tuyo, o cualquier endpoint compatible

## 🔧 Instalación

1. Copia la carpeta `evaluacion-deportiva-plugin/` en `wp-content/plugins/`
2. Activa el plugin desde **WordPress → Plugins** (el esquema de base de datos se crea automáticamente vía `dbDelta`)
3. Ve a **Evaluación Deportiva → Configuración** y define:
   - Nombre, siglas y logo del club
   - (Opcional) URL de la API de remoción de fondo — endpoint que reciba `POST` multipart (`file`) y devuelva un PNG/WebP sin fondo
   - Página del dashboard donde vive el shortcode `[evaluacion_deportiva_dashboard]`

Si no configuras la API de remoción de fondo, el plugin sigue funcionando con normalidad usando la foto original (degradación elegante).

## 📊 Esquema de base de datos

14+ tablas, entre ellas: clubs, categorías, equipos, jugadores (y su relación M:M con equipos), evaluaciones (plantilla vs. instancia), badges, fotos de perfil, notificaciones, auditoría y configuración. Ver `schema-evaluacion-deportiva-v2.0.sql` para el detalle completo.

## 🗺️ Roadmap

- [x] Fase 1 — Esquema de base de datos
- [x] Fase 2 — Backend core + import/export
- [x] Fase 3 — Admin UI
- [x] Fase 4 — Flujo de evaluaciones + badges + registro de jugadores + roles
- [ ] Fase 5.2 — Dashboards de Coach / Entrenador / Director
- [ ] Fase 6 — Reportes en PDF

## 💰 Licenciamiento

El **código** de este repositorio se distribuye bajo **GPL v2** (compatible con WordPress). El **uso comercial soportado** (actualizaciones, soporte, hosting del servicio de fotos) se ofrece bajo tres planes:

Para detalles de cada plan, contacta a [Ilderim Digital Solutions](https://ilderim.mx).

## 📞 Contacto

**Ilderim Digital Solutions**
🌐 [ilderim.mx](https://ilderim.mx)
📧 contacto@ilderim.mx

## 📄 Licencia

GPL v2 — ver [`LICENSE`](./LICENSE)

---

*Última actualización: julio 2026*
