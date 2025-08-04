# Controller Module

The controller module is a crucial part of the Cline extension, serving as the intermediary between user interactions and task execution. It manages state distribution and coordinates communications across components.

## Overview

The controller acts as the central hub for managing the extension's various states and operations:

- Handles messages from webview UI
- Creates and manages Task instances
- Coordinates with MCP servers and API providers
- Maintains persistent storage (settings, task history)

```tree
controller/
├── index.ts          # Core controller logic
├── stateManager.ts   # Manages global and workspace states
├── mcpClient.ts      # Handles MCP server connections
├── apiConfig.ts      # Manages API provider configurations
└── ...
```

## Key Components

### State Management

The Controller maintains multiple types of persistent storage:
- **Global State**: Stored across all VSCode instances. Used for settings and data that should persist globally.
- **Workspace State**: Specific to the current workspace. Used for task-specific data and settings.
- **Secrets**: Secure storage for sensitive information like API keys.

### MCP Integration

The Controller manages connections to external MCP servers through the McpHub service:
- Connects/disconnects from servers
- Monitors server status
- Handles auto-restart logic

### Task Management

When a user initiates a task, the Controller:
1. Creates a new Task instance
2. Sets up the initial context and state
3. Executes the task through the Task's API request loop
4. Updates webview with task progress/results
5. Handles error recovery and resource cleanup

## Communication Flow

1. User interacts with UI -> Webview sends message to Controller
2. Controller creates a Task instance
3. Task executes operations (API calls, tool uses)
4. Results are passed back through the Controller
5. Controller updates webview state

This structure ensures modularity, maintainability, and clear separation of concerns within the controller module.
