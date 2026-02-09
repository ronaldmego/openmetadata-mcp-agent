# OpenMetadata Agent - Capa Agéntica para OpenMetadata

## Visión

**Dar a cualquier organización un asistente conversacional para su catálogo de datos, sin depender de Claude Desktop.**

Este proyecto agrega una capa agéntica sobre OpenMetadata, permitiendo que usuarios de negocio interactúen con el catálogo de datos usando lenguaje natural.

## Filosofía

1. **Standalone** - No requiere Claude Desktop ni infraestructura Anthropic
2. **Portable** - Deployable en cualquier servidor con Docker o Python
3. **Configurable** - Cada cliente usa su propia API key de Gemini y su OpenMetadata
4. **Evolutivo** - MVP primero, enterprise features después

## Quick Start (MVP)

```bash
cd ~/projects/openmetadata-mcp

# Configurar
cp .env.example .env
# Editar .env con tus credenciales

# Ejecutar
pip install -r requirements.txt
streamlit run app.py --server.port 4004

# Acceder
# http://localhost:4004
```

## Arquitectura

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Streamlit     │────▶│   Agent         │────▶│  OpenMetadata   │
│   Chat UI       │     │   (Gemini LLM)  │     │  REST API       │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │                       │
        ▼                       ▼
   Usuario              Tools MCP:
   (browser)            - search_catalog
                        - list_tables
                        - get_table_details
                        - get_lineage
                        - list_databases
```

## Configuración

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `GOOGLE_API_KEY` | API key de Gemini (pagada recomendada) | `AIzaSy...` |
| `OPENMETADATA_URL` | URL de la instancia OpenMetadata | `http://localhost:8585` |
| `OPENMETADATA_TOKEN` | JWT token de bot de OpenMetadata | `eyJhbG...` |
| `GEMINI_MODEL` | Modelo a usar | `gemini-2.5-pro` |

## Roadmap

### ✅ MVP (Actual)
- [x] Chat UI con Streamlit
- [x] Agente con Gemini 2.5 Pro
- [x] 6 tools de OpenMetadata (search, tables, details, lineage, databases, glossary)
- [x] Configuración via .env
- [x] Respuestas en español

### 🔄 Fase 2: Multi-Usuario
- [ ] Sesiones por usuario (Streamlit session state)
- [ ] Historial de conversación persistente
- [ ] Límite de requests por sesión
- [ ] Logging de queries por usuario

### 🔐 Fase 3: Autenticación
- [ ] Login con usuario/password
- [ ] Integración con SSO corporativo (SAML/OIDC)
- [ ] Roles: admin, analyst, viewer
- [ ] Permisos granulares por schema/database

### 📊 Fase 4: Auditoría Enterprise
- [ ] Log de todas las queries a base de datos
- [ ] Dashboard de uso (queries por usuario, por día)
- [ ] Alertas de queries sensibles
- [ ] Export de logs para compliance
- [ ] Integración con SIEM

### 🚀 Fase 5: Producción
- [ ] Dockerfile optimizado
- [ ] Docker Compose con nginx/traefik
- [ ] Health checks
- [ ] Rate limiting
- [ ] Caché de respuestas frecuentes
- [ ] Backup de configuración

## Estructura del Proyecto

```
openmetadata-mcp/
├── app.py              # Streamlit UI (chat interface)
├── agent.py            # Lógica del agente conversacional
├── server.py           # Tools MCP para OpenMetadata
├── .env                # Configuración local (no commitear)
├── .env.example        # Template de configuración
├── requirements.txt    # Dependencias Python
├── CLAUDE.md           # Este archivo - visión del proyecto
├── README.md           # Documentación pública
└── test_*.py           # Scripts de testing
```

## Deploy para Cliente

### Requisitos del servidor cliente
- Python 3.10+
- Acceso a OpenMetadata (URL + token)
- API key de Gemini (pagada recomendada)
- Puerto disponible (default: 4004)

### Pasos
1. Clonar/copiar el proyecto
2. Configurar `.env` con credenciales del cliente
3. `pip install -r requirements.txt`
4. `streamlit run app.py --server.port 4004`
5. (Opcional) Configurar nginx como reverse proxy
6. (Opcional) Configurar systemd para auto-start

### Seguridad para producción
- Usar HTTPS (nginx + certbot)
- Restringir acceso por IP o VPN
- Rotar tokens de OpenMetadata periódicamente
- Monitorear uso de API de Gemini

## Referencias

- OpenMetadata API: https://docs.open-metadata.org/latest/main-concepts/metadata-standard/apis
- Gemini API: https://ai.google.dev/gemini-api/docs
- Streamlit: https://docs.streamlit.io
- FastMCP: https://gofastmcp.com

## Contacto

Proyecto desarrollado por GalacticaIA para clientes enterprise.
- Web: https://galacticaia.com
- Producto: Khipu (AI Agent Governance)

---

*Versión: MVP 1.0*
*Última actualización: 2026-02-09*
