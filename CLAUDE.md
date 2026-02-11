# OpenMetadata Agent - Capa Agéntica para OpenMetadata

## Tabla de Contenidos

- [Puerto](#puerto)
- [Visión y Filosofía](#visión-y-filosofía)
- [Quick Start](#quick-start)
- [Arquitectura](#arquitectura)
- [Configuración](#configuración)
- [Comandos Frecuentes](#comandos-frecuentes)
- [Filosofía de Desarrollo](#filosofía-de-desarrollo)
- [Deploy para Cliente](#deploy-para-cliente)
- [Seguridad](#seguridad)
- [Recursos](#recursos)

---

## Puerto

| Item | Valor |
|------|-------|
| Puerto Prod | `4004` (Tailscale only) |
| Bind | `100.64.216.28` |
| URL | http://100.64.216.28:4004 |
| Proceso | `streamlit run app.py` |

---

## Visión y Filosofía

**Dar a cualquier organización un asistente conversacional para su catálogo de datos, sin depender de Claude Desktop.**

Este proyecto agrega una capa agéntica sobre OpenMetadata, permitiendo que usuarios de negocio interactúen con el catálogo de datos usando lenguaje natural.

### Principios

1. **Standalone** - No requiere Claude Desktop ni infraestructura Anthropic
2. **Portable** - Deployable en cualquier servidor con Docker o Python
3. **Configurable** - Cada cliente usa su propia API key de Gemini y su OpenMetadata
4. **Evolutivo** - MVP primero, enterprise features después

---

## Quick Start

```bash
cd ~/projects/openmetadata-mcp-client

# Configurar
cp .env.example .env
# Editar .env con tus credenciales

# Instalar dependencias
pip install -r requirements.txt

# Ejecutar
streamlit run app.py --server.port 4004

# Acceder: http://localhost:4004
```

---

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
                        - get_glossary_terms
```

### Estructura del Proyecto

```
openmetadata-mcp-client/
├── app.py              # Streamlit UI (chat interface)
├── agent.py            # Lógica del agente conversacional
├── server.py           # Tools MCP para OpenMetadata
├── .env                # Configuración local (no commitear)
├── .env.example        # Template de configuración
├── requirements.txt    # Dependencias Python
├── CLAUDE.md           # Visión y estándares del proyecto
├── README.md           # Documentación pública
└── test_*.py           # Scripts de testing
```

### Configuración

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `GOOGLE_API_KEY` | API key de Gemini (pagada recomendada) | `AIzaSy...` |
| `OPENMETADATA_URL` | URL de la instancia OpenMetadata | `http://localhost:8585` |
| `OPENMETADATA_TOKEN` | JWT token de bot de OpenMetadata | `eyJhbG...` |
| `GEMINI_MODEL` | Modelo a usar | `gemini-2.5-pro` |

---

## Comandos Frecuentes

```bash
# --- Desarrollo ---
streamlit run app.py --server.port 4004                          # Levantar en dev
streamlit run app.py --server.port 4004 --server.address 0.0.0.0 # Exponer en red

# --- Testing ---
python test_connection.py          # Probar conexión a OpenMetadata
python test_agent.py               # Probar agente end-to-end

# --- Producción ---
# El proceso corre como servicio en el VPS, bind a Tailscale
streamlit run app.py --server.port 4004 --server.address 100.64.216.28

# --- Logs / Debug ---
# Streamlit imprime logs en stdout
# Para ver variables de entorno cargadas:
python -c "from dotenv import load_dotenv; load_dotenv(); import os; print(os.getenv('OPENMETADATA_URL'))"

# --- Dependencias ---
pip install -r requirements.txt    # Instalar
pip freeze > requirements.txt      # Actualizar (con cuidado)
```

---

## Filosofía de Desarrollo

### Stack

- **UI**: Streamlit (chat interface)
- **LLM**: Google Gemini 2.5 Pro via `google-genai`
- **Backend**: OpenMetadata REST API
- **Lenguaje**: Python 3.10+

### Convenciones de Código

- **Idioma del código**: inglés (variables, funciones, clases)
- **Idioma de respuestas al usuario**: español
- **Naming**: snake_case para funciones y variables, PascalCase para clases
- **Docstrings**: en español, solo cuando la función no es autoexplicativa
- **Archivos**: un archivo por responsabilidad (`app.py` = UI, `agent.py` = lógica, `server.py` = tools)

### Patrones

- Las tools de OpenMetadata se definen en `server.py` usando FastMCP
- El agente en `agent.py` orquesta las llamadas al LLM y las tools
- `app.py` es solo la capa de presentación (Streamlit)
- Toda configuración sensible va en `.env`, nunca hardcoded
- Las respuestas del agente siempre son en español

---

## Deploy para Cliente

### Requisitos del servidor

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

---

## Seguridad

- **Credenciales**: siempre en `.env`, nunca en código ni commits
- **HTTPS**: usar nginx + certbot en producción
- **Acceso**: restringir por IP, VPN o Tailscale
- **Tokens**: rotar tokens de OpenMetadata periódicamente
- **API Gemini**: monitorear uso para evitar costos inesperados
- **Puerto**: `4004` registrado en `~/.claude/port-registry.md`

---

## Recursos

- [Roadmap del proyecto](./ROADMAP.md)
- [OpenMetadata API Docs](https://docs.open-metadata.org/latest/main-concepts/metadata-standard/apis)
- [Gemini API Docs](https://ai.google.dev/gemini-api/docs)
- [Streamlit Docs](https://docs.streamlit.io)
- [FastMCP Docs](https://gofastmcp.com)
- [GalacticaIA](https://galacticaia.com) - Producto: Khipu (AI Agent Governance)
