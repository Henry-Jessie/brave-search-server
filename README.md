A Model Context Protocol (MCP) server implementation that provides access to the Brave Search API with built-in proxy support. This server is specifically designed to work in environments where direct access to Brave's API might be restricted or unstable.

## Introduction

The Brave Search Server is part of the Model Context Protocol (MCP) ecosystem, which standardizes communication between model servers and clients. This implementation focuses on providing robust search capabilities through Brave's API while handling proxy requirements for reliable access.

## Features

- Web search functionality using Brave Search API
- Local business search with detailed information
- Built-in proxy support for reliable API access
- Rate limiting implementation (1 request/second, 15,000 requests/month)
- Seamless integration with Claude desktop client

## Prerequisites

- Node.js (Latest LTS version recommended)
- npm package manager
- A running proxy server (default configuration expects proxy at http://127.0.0.1:7890)
- Brave Search API key
- Claude desktop client (for integration)

## Local Build Guide

1. Create a new server project using MCP's template:
```bash
npx @modelcontextprotocol/create-server brave-search-server
cd brave-search-server
```

2. Install required dependencies:
```bash
# Install MCP SDK and networking dependencies
npm install @modelcontextprotocol/sdk node-fetch@2 https-proxy-agent

# Install TypeScript type definitions
npm i --save-dev @types/node-fetch
```

3. Configure the proxy in `src/index.ts`. Add these imports at the top:
```typescript
import fetch from 'node-fetch';
import { HttpsProxyAgent } from 'https-proxy-agent';

// Configure your proxy URL
const PROXY_URL = 'http://127.0.0.1:7890'
const proxyAgent = new HttpsProxyAgent(PROXY_URL);
```

4. Modify all fetch requests to use the proxy agent. For each fetch call, add the agent configuration:
```typescript
const response = await fetch(url, {
    headers: {
        'Accept': 'application/json',
        'Accept-Encoding': 'gzip',
        'X-Subscription-Token': BRAVE_API_KEY
    },
    agent: proxyAgent  // Add this line to enable proxy
});
```

5. Build the project:
```bash
npm run build
```

## Integration with Claude Desktop Client

1. Open Claude's Settings and navigate to the Developer tab
2. Click \"Edit Config\" to open the configuration file
3. Add the following configuration:
```json
    \"mcpServers\": {
        \"brave-search-server\": {
            \"command\": \"node\",
            \"args\": [
                \"/path/to/your/brave-search-server/build/index.js\"
            ],
            \"env\": {
                \"BRAVE_API_KEY\": \"YOUR-API-KEY-HERE\"
            }
        }
    }
```
Note: Replace `/path/to/your` with the actual path to your server's build directory.

4. Restart Claude to apply the changes


