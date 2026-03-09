# McpServerTools

Tool manifests for the MCP Server tool registry. This repository acts as the primary **tool bucket** — a collection of tool definitions that can be browsed, installed, and synced via the MCP Server's tool registry API.

## Structure

Each tool is defined as a JSON manifest file in the root directory:

```
tool-name.json
```

### Manifest Format

```json
{
  "name": "tool-name",
  "description": "Human-readable description of what this tool does",
  "tags": ["keyword1", "keyword2"],
  "parameterSchema": "{ ... optional JSON schema ... }",
  "commandTemplate": "command --arg {{param1}}"
}
```

## Usage

### Register this bucket with MCP Server

```bash
curl -X POST http://localhost:7147/mcpserver/tools/buckets \
  -H "Content-Type: application/json" \
  -d '{
    "name": "official",
    "owner": "sharpninja",
    "repo": "McpServerTools",
    "branch": "main",
    "manifestPath": "/"
  }'
```

### Browse available tools

```bash
curl http://localhost:7147/mcpserver/tools/buckets/official/browse
```

### Install a tool from this bucket

```bash
curl -X POST "http://localhost:7147/mcpserver/tools/buckets/official/install?toolName=screenshot"
```

### Sync to pick up updates

```bash
curl -X POST http://localhost:7147/mcpserver/tools/buckets/official/sync
```

## Contributing

Add a new `.json` manifest file following the format above and submit a pull request.
