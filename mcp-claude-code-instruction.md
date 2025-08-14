# MCP Setup Instructions for Claude Code

## Overview
Model Context Protocol (MCP) allows Claude Code to connect to external servers and tools, extending its capabilities with database access, web automation, and filesystem operations.

## Prerequisites
- Node.js and npm installed
- Claude Code configured and running
- Access to the services you want to connect (PostgreSQL, filesystem paths, etc.)

## Step-by-Step Setup

### 1. Configuration File Location
The `.mcp.json` configuration file should be placed in your project root directory:
```
/mnt/d/claude-code/.mcp.json
```

### 2. MCP Directory Structure
Create the MCP working directory:
```bash
mkdir -p /mnt/d/claude-code/.claude/mcp
```

### 3. Configure MCP Servers

#### Database Server (PostgreSQL)
```json
"database": {
  "command": "npx",
  "args": ["@modelcontextprotocol/server-postgres"],
  "env": {
    "POSTGRES_CONNECTION_STRING": "postgresql://localhost:5432/myapp"
  }
}
```

**Setup Steps:**
1. Ensure PostgreSQL is running on your system
2. Update the connection string with your actual database details:
   - Host: `localhost` (or your database host)
   - Port: `5432` (or your database port)
   - Database name: `myapp` (replace with your database name)
   - Add username/password if required: `postgresql://username:password@localhost:5432/myapp`

#### Playwright Server (Web Automation)
```json
"playwright": {
  "command": "npx",
  "args": ["@playwright/mcp@latest", "/mnt/d/claude-code/.claude/mcp"]
}
```

**Setup Steps:**
1. The server will be installed automatically when first used
2. The path `/mnt/d/claude-code/.claude/mcp` is where Playwright will store browser data and screenshots

#### Filesystem Server
```json
"filesystem": {
  "command": "npx",
  "args": ["@modelcontextprotocol/server-filesystem", "/mnt/d/claude-code/.claude/mcp"]
}
```

**Setup Steps:**
1. The server provides access to the specified directory
2. Update the path if you need access to a different directory
3. For security, the server only has access to the specified path and its subdirectories

### 4. Path Configuration Best Practices

#### Recommended Paths:
- **Project-specific**: `/mnt/d/claude-code/.claude/mcp` (current setup)
- **Home directory**: `$HOME/claude-mcp` 
- **Temporary directory**: `/tmp/claude-mcp`

#### Security Considerations:
- Never point filesystem server to root (`/`) or sensitive directories
- Use specific project directories rather than broad system paths
- For database connections, use environment variables for credentials

### 5. Starting Claude Code with MCP

Once configured, restart Claude Code. It should automatically detect and load the MCP servers defined in `.mcp.json`.

### 6. Verification

You can verify MCP is working by:
1. Checking if Claude Code shows MCP server connections in startup logs
2. Testing database queries (if database server is configured)
3. Using web automation features (if Playwright server is configured)
4. Accessing filesystem operations (if filesystem server is configured)

### 7. Troubleshooting

#### Common Issues:
- **Server not starting**: Check if the required npm packages are available
- **Database connection failed**: Verify PostgreSQL is running and connection string is correct
- **Permission denied**: Ensure the filesystem path exists and has proper permissions
- **Port conflicts**: Check if required ports are available

#### Debug Commands:
```bash
# Test PostgreSQL connection
psql "postgresql://localhost:5432/myapp"

# Check if directory exists and is accessible
ls -la /mnt/d/claude-code/.claude/mcp

# Verify Node.js and npm are available
node --version && npm --version
```

## Environment Variables

For sensitive data like database passwords, consider using environment variables:

```bash
export POSTGRES_CONNECTION_STRING="postgresql://username:password@localhost:5432/myapp"
```

Then reference in `.mcp.json`:
```json
"env": {
  "POSTGRES_CONNECTION_STRING": "${POSTGRES_CONNECTION_STRING}"
}
```

## Security Notes
- Keep database credentials secure
- Limit filesystem access to necessary directories only
- Regularly update MCP server packages
- Monitor server logs for any suspicious activity