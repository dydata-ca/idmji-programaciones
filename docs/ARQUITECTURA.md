# ADR-001 — Publicación estática de noviembre

- **Estado:** propuesta para aprobación de despliegue.
- **Contexto:** se requiere consultar una programación existente, sin modificaciones.
- **Decisión:** HTML/CSS/JavaScript estáticos alojados en GitHub Pages, repositorio dedicado, sin backend ni base de datos.
- **Ventajas:** costo de infraestructura bajo, despliegue simple, versionado y reversión por Git.
- **Riesgos:** acceso universal a los datos; posible indexación/copia; datos embebidos y JSON duplicados deben mantenerse sincronizados; la vista Hoy queda limitada a noviembre 2026.
- **Controles:** datos públicos minimizados, revisión manual previa, no publicar fuentes con nombres completos, `noindex`, validación en móvil, historial de commits.
- **Fuera de alcance:** generación diciembre, gestión de voluntarios, reemplazos, autenticación, producción institucional.
