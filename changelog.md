# Changelog

Todos los cambios notables de este proyecto se documentan aquí.

---

## [0.2.0] - 2026-02-13

### Added
- **Tool `list_domains()`** - Nuevo tool MCP que consulta `/api/v1/domains` de OpenMetadata para listar dominios y subdominios con tipo, descripción y jerarquía padre/hijo. ([#1](https://github.com/ronaldmego/openmetadata-mcp-client/issues/1))
- **Debug expander en UI** - Cada respuesta del agente ahora incluye un expander colapsable que muestra: tool usada, parámetros, decisión del LLM y resultado crudo de la API. Permite diagnosticar problemas de routing sin tocar código. ([#1](https://github.com/ronaldmego/openmetadata-mcp-client/issues/1))

### Fixed
- El agente ahora puede responder correctamente preguntas sobre dominios y subdominios del catálogo. Antes usaba `list_databases` como fallback. ([#1](https://github.com/ronaldmego/openmetadata-mcp-client/issues/1))

---

## [0.1.0] - 2026-02-12

### Added
- Chat UI con Streamlit
- Agente conversacional con Gemini 2.5 Pro
- 6 tools MCP: `search_catalog`, `list_tables`, `get_table_details`, `list_databases`, `get_lineage`, `list_glossary_terms`
- Configuración via `.env`
- Respuestas en español
