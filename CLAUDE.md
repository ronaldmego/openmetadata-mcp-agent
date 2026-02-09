# OpenMetadata MCP Server

## Quick Start

```bash
cd ~/projects/openmetadata-mcp
python3 server.py
```

## Verificado Funcionando

- ✅ 32 tablas catalogadas
- ✅ Schemas: becgi, public, galacticaia
- ✅ Token configurado en .env

## Tools Disponibles

| Tool | Uso |
|------|-----|
| `search_catalog(query)` | Buscar en catálogo |
| `list_tables(limit)` | Listar tablas |
| `get_table_details(name)` | Detalle de tabla |
| `get_lineage(name)` | Linaje de asset |
| `list_databases()` | Databases |
| `list_glossary_terms()` | Glosario |

## Uso con mcp-use

```python
from mcp_use import MCPAgent, MCPClient
from langchain_openai import ChatOpenAI

config = {
    "mcpServers": {
        "openmetadata": {
            "command": "python3",
            "args": ["/home/adminmgo/projects/openmetadata-mcp/server.py"]
        }
    }
}

client = MCPClient.from_dict(config)
agent = MCPAgent(llm=ChatOpenAI(model="gpt-4o"), client=client)
result = await agent.run("¿Qué tablas tenemos de leads?")
```

## Archivos

- `server.py` - MCP server (FastMCP)
- `.env` - Token de OpenMetadata (no commitear)
- `requirements.txt` - Dependencias
