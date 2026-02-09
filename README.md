# OpenMetadata MCP Server

MCP server para interactuar con OpenMetadata via conversación natural.

## Instalación

```bash
cd ~/projects/openmetadata-mcp
pip install -r requirements.txt
```

## Configuración

Necesitas el token JWT de OpenMetadata:

```bash
export OPENMETADATA_URL="http://100.64.216.28:8585"
export OPENMETADATA_TOKEN="tu-jwt-token-aqui"
```

### Obtener el token

1. Ir a http://100.64.216.28:8585
2. Login como admin
3. Settings → Bots → Crear bot o usar existente
4. Copiar el JWT token

## Uso

### Standalone (para testing)

```bash
python server.py
```

### Con mcp-use (agente conversacional)

```python
from mcp_use import MCPAgent, MCPClient
from langchain_openai import ChatOpenAI

config = {
    "mcpServers": {
        "openmetadata": {
            "command": "python",
            "args": ["/home/adminmgo/projects/openmetadata-mcp/server.py"],
            "env": {
                "OPENMETADATA_TOKEN": "tu-token"
            }
        }
    }
}

client = MCPClient.from_dict(config)
agent = MCPAgent(llm=ChatOpenAI(model="gpt-4o"), client=client)

# Ejemplo
result = await agent.run("¿Qué tablas tenemos relacionadas con customers?")
print(result)
```

## Tools disponibles

| Tool | Descripción |
|------|-------------|
| `search_catalog` | Buscar assets en el catálogo |
| `list_tables` | Listar tablas (filtrar por database/schema) |
| `get_table_details` | Detalles de una tabla (columnas, owner, tags) |
| `get_lineage` | Linaje upstream/downstream de un asset |
| `list_databases` | Listar databases/services |
| `list_glossary_terms` | Términos del glosario de negocio |

## Ejemplos de conversación

```
Usuario: "¿Qué tablas tenemos?"
→ Usa list_tables

Usuario: "Muéstrame el linaje de la tabla orders"
→ Usa get_lineage("orders")

Usuario: "¿Qué columnas tiene la tabla customers?"
→ Usa get_table_details("customers")

Usuario: "Busca todo lo relacionado con ventas"
→ Usa search_catalog("ventas")
```

## Estructura

```
openmetadata-mcp/
├── server.py          # MCP server principal
├── requirements.txt   # Dependencias
└── README.md          # Esta documentación
```

## TODO

- [ ] Agregar tools de escritura (add_description, add_tag)
- [ ] Soporte para pipelines y dashboards
- [ ] Cache de resultados frecuentes
- [ ] Tests
