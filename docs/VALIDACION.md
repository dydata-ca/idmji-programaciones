# IDMJI v1.1 — Validación y pendientes

**Fecha:** 2026-11-05. **Estado:** pausa voluntaria tras la mejora de numeración.

## Confirmado por el usuario

- [x] La programación pública carga y navega.
- [x] El panel administrativo carga y muestra la programación.
- [x] La página pública muestra correctamente las guías de Enseñanza y Alabanzas.
- [x] Estudio bíblico permanece sin secuencia.
- [x] Se incorporó la consulta `fetch()` a la API pública de programación.

## Pendiente de verificación

- [ ] Registrar **un reemplazo autorizado** (o prueba aislada) y comprobar respuesta de la API.
- [ ] Verificar persistencia y auditoría sin duplicar eventos.
- [ ] Comprobar que panel y página pública muestran el mismo reemplazo.
- [ ] Revisar el estado de error/carga para no mostrar programación embebida desactualizada.
- [ ] Verificar controles de acceso, CSRF y que el endpoint público no exponga datos privados.
- [ ] Confirmar que la fuente de producción y GitHub están sincronizadas antes de cualquier despliegue.
- [ ] Definir numeración continua para meses siguientes.
- [ ] Comparar todas las asignaciones con los PDF originales y confirmar autorización de publicación.

## Incidencias históricas de transcripción

Se retiraron etiquetas de diciembre interpretadas erróneamente como colaboradores de noviembre:
- 2026-11-25 / Interpretación EN: `Dec 1.`
- 2026-11-26 / Interpretación EN: `Dec 2., Dec 3.`

Aún requieren revisión humana:
- 2026-11-02 / Interpretación EN: nombre duplicado.
- 2026-11-05 / Interpretación EN: sin nombre consignado.

No corregir por inferencia: contrastar con los PDF privados.

## Criterio de cierre

La v1.1 no debe considerarse completamente validada hasta probar el circuito de reemplazos y revisar la exposición de información personal. El funcionamiento visual y la numeración sí fueron confirmados.
