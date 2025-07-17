[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/do-boo-mcp-synolink-badge.png)](https://mseep.ai/app/do-boo-mcp-synolink)

# SynoLink MCP Server

A Node.js server implementing Model Context Protocol (MCP) for Synology NAS file operations. This server allows you to interact with your Synology NAS device through Claude or other compatible AI assistants.

## Features

- Login/logout to Synology DSM
- List files and folders
- Download file contents
- Upload files
- Create folders
- Delete files/folders
- Move/rename files and folders
- Search functionality
- Create and list sharing links
- Get server information
- Get quota information

## Prerequisites

- Node.js 18 or higher
- npm or yarn
- Synology NAS with DSM 6.0 or higher
- Network access to your Synology NAS

## Installation

Clone this repository:

```bash
git clone https://github.com/Do-Boo/MCP-SynoLink.git
cd MCP-SynoLink
```

Install dependencies:

```bash
npm install
```

Build the project:

```bash
npm run build
```

## Usage with Claude Desktop

### Node.js Method

Add this to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "synolink": {
      "command": "node",
      "args": [
        "/path/to/MCP-SynoLink/dist/index.js",
        "https://your-synology-url:port",
        "your-username",
        "your-password"
      ]
    }
  }
}
```

### Docker Method

Build the Docker image:

```bash
docker build -t mcp/synolink .
```

Then add this to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "synolink": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "mcp/synolink",
        "https://your-synology-url:port",
        "your-username",
        "your-password"
      ]
    }
  }
}
```

### Security Note

Always be careful with credentials. The current implementation sends the password as a command-line argument, which may be visible in process listings. For improved security in a production environment, consider implementing alternative authentication methods.

## API Documentation

The server provides the following tools:

### Authentication Tools

- **login**
  - Authenticates with the Synology NAS
  - No parameters required (uses credentials from command line)

- **logout**
  - Logs out from the Synology NAS
  - No parameters required

### File Management Tools

- **list_folders**
  - Lists files and folders in a directory
  - Input: `path` (string) - Path to list files from, e.g., '/photos'

- **get_file**
  - Gets the content of a file
  - Input: `path` (string) - Full path to the file on Synology NAS

- **upload_file**
  - Uploads a file to Synology NAS
  - Inputs:
    - `path` (string) - Destination path on Synology NAS including filename
    - `content` (string) - Content of the file to upload

- **create_folder**
  - Creates a new folder
  - Inputs:
    - `path` (string) - Full path to create folder at
    - `name` (string) - Name of the new folder

- **delete_item**
  - Deletes a file or folder
  - Input: `path` (string) - Full path to the file or folder to delete

- **move_item**
  - Moves or renames a file or folder
  - Inputs:
    - `source` (string) - Full path to the source file or folder
    - `destination` (string) - Full path to the destination location

### Search and Information Tools

- **search**
  - Searches for files and folders
  - Inputs:
    - `keyword` (string) - Search keyword
    - `path` (string, optional) - Path to search in, defaults to "/"

- **get_share_links**
  - Gets or creates sharing links for a file or folder
  - Input: `path` (string) - Path to get share links for

- **get_server_info**
  - Gets information about the Synology server
  - No parameters required

- **get_quota_info**
  - Gets quota information for volumes
  - Input: `volume` (string, optional) - Volume name, if omitted shows all volumes

## License

MIT License

## Acknowledgements

- [Synology Web API](https://global.download.synology.com/download/Document/Software/DeveloperGuide/Package/FileStation/All/enu/Synology_File_Station_API_Guide.pdf)
- [Model Context Protocol](https://modelcontextprotocol.io/)
