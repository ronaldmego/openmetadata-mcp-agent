# Roadmap - OpenMetadata Agent

## MVP (Actual)

- [x] Chat UI con Streamlit
- [x] Agente con Gemini 2.5 Pro
- [x] 7 tools de OpenMetadata (search, tables, details, lineage, databases, glossary, domains)
- [x] Configuración via .env
- [x] Respuestas en español
- [x] Debug expander en UI (ver tool usada, parámetros y resultado crudo)

## Fase 1.5: Agente Inteligente

Evolución de la capa agéntica para que sea más autónoma y robusta.

- [ ] **Descubrimiento dinámico de tools** - Que el agente lea las tools disponibles del MCP server en vez de tener una lista hardcoded en `TOOLS_INFO`. Así al agregar un tool en `server.py` se refleja automáticamente.
- [ ] **Eliminar routing manual (if/elif)** - Usar function calling nativo de Gemini en vez de parsear texto. Gemini soporta `tools` parameter con schema JSON, lo cual elimina el `if/elif` y el parseo de strings.
- [ ] **Loop de reintento** - Si la primera herramienta no devuelve resultados útiles, que el agente pueda repensar y probar otra estrategia (como hace Claude Desktop).
- [ ] **Cobertura completa de la API** - Agregar tools para: data products, tags, policies, teams, users, pipelines. Ver tabla en [technical_notes.md](./technical_notes.md).
- [ ] **Memoria conversacional** - Que el agente use el historial del chat para resolver referencias ("la tabla que mencionaste antes").
- [ ] **Multi-tool por query** - Que el agente pueda usar más de una herramienta por pregunta (ej: listar dominios + contar tablas por dominio).

## Fase 2: Multi-Usuario

- [ ] Sesiones por usuario (Streamlit session state)
- [ ] Historial de conversación persistente
- [ ] Límite de requests por sesión
- [ ] Logging de queries por usuario

## Fase 3: Autenticación

- [ ] Login con usuario/password
- [ ] Integración con SSO corporativo (SAML/OIDC)
- [ ] Roles: admin, analyst, viewer
- [ ] Permisos granulares por schema/database

## Fase 4: Auditoría Enterprise

- [ ] Log de todas las queries a base de datos
- [ ] Dashboard de uso (queries por usuario, por día)
- [ ] Alertas de queries sensibles
- [ ] Export de logs para compliance
- [ ] Integración con SIEM

## Fase 5: Producción

- [ ] Dockerfile optimizado
- [ ] Docker Compose con nginx/traefik
- [ ] Health checks
- [ ] Rate limiting
- [ ] Caché de respuestas frecuentes
- [ ] Backup de configuración
