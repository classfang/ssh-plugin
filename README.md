# SSH Plugin for Claude

A Claude plugin that enables SSH remote command execution capabilities by integrating with ssh-mcp-server.

## Features

- 🔒 **Secure SSH Connections**: Supports both password and private key authentication
- 🛡️ **Command Security Control**: Whitelist/blacklist mechanism to control executable commands
- 📂 **File Transfer**: Upload and download files to/from servers
- 🔄 **Multi-Server Support**: Manage multiple SSH connections simultaneously
- 🌐 **Proxy Support**: SOCKS proxy support for connections

## Installation

1. Clone or download this plugin to your local machine
2. Load this plugin in Claude

## Configuration

### Basic Configuration

Edit the `.claude-plugin/mcp.json` file and add your SSH server configuration:

#### Using Password Authentication

```json
{
  "mcpServers": {
    "ssh-mcp-server": {
      "command": "npx",
      "args": [
        "-y",
        "@fangjunjie/ssh-mcp-server",
        "--host", "your-server-ip",
        "--port", "22",
        "--username", "your-username",
        "--password", "your-password"
      ]
    }
  }
}
```

#### Using Private Key Authentication

```json
{
  "mcpServers": {
    "ssh-mcp-server": {
      "command": "npx",
      "args": [
        "-y",
        "@fangjunjie/ssh-mcp-server",
        "--host", "your-server-ip",
        "--port", "22",
        "--username", "your-username",
        "--privateKey", "~/.ssh/id_rsa"
      ]
    }
  }
}
```

### Advanced Configuration

#### Adding Command Whitelist (Recommended)

```json
{
  "mcpServers": {
    "ssh-mcp-server": {
      "command": "npx",
      "args": [
        "-y",
        "@fangjunjie/ssh-mcp-server",
        "--host", "your-server-ip",
        "--port", "22",
        "--username", "your-username",
        "--password", "your-password",
        "--whitelist", "^ls( .*)?,^cat .*,^df.*,^pwd,^whoami"
      ]
    }
  }
}
```

#### Adding Command Blacklist

```json
{
  "mcpServers": {
    "ssh-mcp-server": {
      "command": "npx",
      "args": [
        "-y",
        "@fangjunjie/ssh-mcp-server",
        "--host", "your-server-ip",
        "--port", "22",
        "--username", "your-username",
        "--password", "your-password",
        "--blacklist", "^rm .*,^shutdown.*,^reboot.*"
      ]
    }
  }
}
```

#### Multi-Server Configuration

Create a configuration file `ssh-config.json`:

```json
[
  {
    "name": "dev",
    "host": "dev-server-ip",
    "port": 22,
    "username": "dev-user",
    "password": "dev-password"
  },
  {
    "name": "prod",
    "host": "prod-server-ip",
    "port": 22,
    "username": "prod-user",
    "privateKey": "~/.ssh/prod_key"
  }
]
```

Then reference it in `mcp.json`:

```json
{
  "mcpServers": {
    "ssh-mcp-server": {
      "command": "npx",
      "args": [
        "-y",
        "@fangjunjie/ssh-mcp-server",
        "--config-file", "ssh-config.json"
      ]
    }
  }
}
```

## Usage

After installing and configuring the plugin, you can perform the following operations through Claude:

### Execute Commands

```
Execute ls -la command on the server
```

### Upload Files

```
Upload local file /path/to/local/file.txt to server /path/to/remote/
```

### Download Files

```
Download /path/to/remote/file.txt from server to local /path/to/local/
```

### List Servers

```
List all available SSH servers
```

### Multi-Server Operations

```
Execute df -h command on prod server
```

## Available Tools

- **execute-command**: Execute SSH commands on remote servers
- **upload**: Upload files to remote servers
- **download**: Download files from remote servers
- **list-servers**: List all available SSH server configurations

## Security Recommendations

1. **Use Command Whitelist**: Strongly recommended to configure `--whitelist` parameter to limit executable commands
2. **Protect Private Keys**: Ensure private key file permissions are correct (600)
3. **Avoid Plain Text Passwords**: Prefer private key authentication over password authentication
4. **Regular Audits**: Regularly review command execution history
5. **Principle of Least Privilege**: Use user accounts with minimal permissions

## Troubleshooting

### Connection Failed

- Check if server IP, port, username, and password are correct
- Confirm SSH service is running on the server
- Check firewall settings

### Private Key Authentication Failed

- Confirm private key file path is correct
- Check private key file permissions (should be 600)
- If private key has a password, ensure `--passphrase` parameter is provided

### Command Rejected

- Check if command matches whitelist rules
- Check if command is in the blacklist
- Confirm user has permission to execute the command

## Related Links

- [ssh-mcp-server GitHub](https://github.com/classfang/ssh-mcp-server)
- [ssh-mcp-server NPM](https://www.npmjs.com/package/@fangjunjie/ssh-mcp-server)

## License

MIT
