# Roadmap - OpenMetadata Agent

## MVP (Actual)

- [x] Chat UI con Streamlit
- [x] Agente con Gemini 2.5 Pro
- [x] 6 tools de OpenMetadata (search, tables, details, lineage, databases, glossary)
- [x] Configuración via .env
- [x] Respuestas en español

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
