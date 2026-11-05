# IDMJI v1.1 — Arquitectura, decisiones y operación

**Fecha de corte:** 2026-11-05  
**Entorno:** producción pequeña en hosting compartido MochaHost; no confundir con Dydata Lab/Proxmox.  
**Estado:** pausa funcional; guías verificadas visualmente; flujo de reemplazos pendiente de prueba extremo a extremo.

## Componentes

- `index.html`: interfaz pública Hoy/Mes, calendario de noviembre, números de guía calculados en JavaScript y `fetch('./api/public-programacion.php')`.
- `admin/index.html` y `admin/dashboard.html`: acceso y administración de asignaciones/reemplazos.
- `api/login.php`, `session.php`, `logout.php`, `csrf.php`: autenticación y sesión administrativa.
- `api/programacion.php`: lectura autenticada de programación efectiva y colaboradores.
- `api/reemplazos.php`: escritura autenticada de reemplazos; requiere token CSRF.
- `api/public-programacion.php`: GET sin autenticación, solo lectura, programación efectiva.
- `idmji-data/programacion.json`: programación base inmutable.
- `idmji-data/colaboradores.json`: catálogo privado.
- `idmji-data/reemplazos.json`: reemplazos vigentes e historial de auditoría.
- `idmji-data/registrar-reemplazo.php`: lógica de validación y persistencia con bloqueo exclusivo y escritura atómica.

## Contratos conceptuales

`GET /idmji/api/public-programacion.php` devuelve un objeto con `month`, `days` y otros metadatos de la programación. Cada día contiene `date`, `service_type`, `start_time` y `assignments`, con `role` y `people`. El endpoint aplica los reemplazos vigentes sobre una copia antes de responder. No debe revelar catálogo privado, historial, credenciales ni rutas internas en errores.

`POST /idmji/api/reemplazos.php` recibe `date`, `assignment_index`, `person_index`, `replacement_id`; exige sesión de administrador y encabezado `X-CSRF-Token`. La persistencia debe conservar el historial. El flujo todavía requiere validación extremo a extremo con un reemplazo autorizado.

## Reglas de guías (ADR-001)

**Decisión:** calcular solo un número secuencial; no incorporar textos de guías.

| Tipo de servicio | Etiqueta | Referencia | Máximo |
|---|---|---|---|
| Alabanzas | Guía de coros | 2026-11-04 = 27 | 43 |
| Enseñanza | Guía de himnos y coros | 2026-11-05 = 60 | 82 |
| Estudio bíblico | Sin secuencia | — | — |

Cada servicio del mismo tipo avanza una unidad, con retorno a 1 al superar el máximo. Las secuencias son independientes. La implementación actual cuenta eventos dentro del arreglo `DATA.days` del mes de noviembre: no extrapolar a meses futuros sin diseñar continuidad y persistencia de la referencia.

## Separación de datos (ADR-002)

**Decisión:** conservar el original y registrar reemplazos como modificaciones independientes, con auditoría. La API aplica cambios al leer; no reescribe el archivo base. Ventajas: trazabilidad y reversión operativa; riesgo: incoherencia si se altera el orden de asignaciones sin migrar índices.

## API pública de lectura (ADR-003)

**Decisión:** endpoint GET separado de la API autenticada; respuesta limitada a programación efectiva. Ventaja: no divulgar catálogo ni auditoría. Riesgos pendientes: exposición pública de nombres abreviados, caché y HTML con datos iniciales obsoletos.

## Despliegue y reversión

La instalación operativa reside en MochaHost, no en GitHub Pages. Los cambios v1.1 se aplicaron manualmente en el hosting. Se mencionaron respaldos en servidor, incluidos `index-pre-api-publica.html.bak`, `index-pre-guias.html.bak`, `dashboard-pre-guardado.html.bak` y `reemplazos-api-pre-activacion.php.bak`; verificar existencia y contenido antes de utilizarlos. No publicar esos respaldos. Para revertir, restaurar exclusivamente el archivo correspondiente desde su copia verificada y comprobar la página; cualquier reversión de datos de reemplazos exige conservar auditoría y autorización.

## Riesgos y trabajo pendiente

1. **Alto:** sin prueba completa de POST autorizado, persistencia y propagación pública.
2. **Medio:** HTML público incluye datos embebidos como estado inicial; posible desactualización.
3. **Medio:** numeración válida para noviembre; sin continuidad intermensual.
4. **Medio:** datos personales inferibles por participación religiosa; revisar autorización de publicación.
5. **Medio:** repositorio GitHub no sincronizado con fuentes PHP/HTML actuales del hosting.
6. **Bajo:** anomalías de transcripción previamente identificadas, pendientes de contraste con PDF.

## Reanudación segura

No crear reemplazos ficticios en producción. Antes de probar, confirmar respaldo del archivo de reemplazos y escoger una operación autorizada o entorno aislado. Verificar una sola vez: respuesta HTTP, cambio persistido, historial, panel y página pública. No repetir POST ante un error sin comprobar primero si se guardó. Después sincronizar código sanitizado con GitHub y documentar commit, fecha y verificación.
