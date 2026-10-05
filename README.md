# McpServerTools

Tool manifests for the QBrain.AI tool registry. This repository acts as the primary **tool bucket** — a collection of tool definitions that can be browsed, installed, and synced via the QBrain.AI tool registry API.

## Structure

Each tool is defined as a JSON manifest file in the root directory:

```
tool-name.json
```

Distribution artifacts live beside the manifests:

```
powershell/
packages/
```

The `powershell/` directory contains the current McpServer helper modules published by this bucket. The `packages/` directory contains the current `SharpNinja.McpServer.Repl` NuGet global-tool package used by the `mcpserver-repl` manifest.

The `mcpserver-*-plugin.json` manifests expose sibling agent plugin repositories. `qbrain-ai-*-plugin.json` manifests resolve the same GitHub repositories. Both names stay through 1.x. Repository URLs stay on the current GitHub names until the repository rename. `/qbrainai/*` is the canonical product prefix; `/mcpserver/*` remains through 1.x. `/mcp-transport` is unchanged. Plugin manifests clone or update from GitHub into a caller-provided parent directory.

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

### Register this bucket with QBrain.AI

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
