# Core Architecture

The core module is the backbone of the Cline extension, managing state and coordinating communication between components. Here's a high-level overview:

## Extension Flow

Extension entry point (extension.ts) -> webview -> controller -> task

```tree
core/
├── webview/      # Manages webview lifecycle and communication
├── controller/   # Handles webview messages, manages state, coordinates tasks
├── task/         # Executes API requests, tools, and maintains context
└── ...           # Additional components for message parsing, etc.
```

## Key Components

### WebviewProvider

- Manages the creation of webviews (HTML panels) in VSCode
- Implements communication between frontend (React) and backend via gRPC
- Handles lifecycle events like panel disposal and visibility changes

### Controller

- Single source of truth for extension state
- Manages API configurations, task history, settings, and MCP servers
- Coordinates state distribution across components
- Handles webview message passing

### Task

- Executes individual tasks (API requests, tool operations)
- Maintains context for each task instance
- Supports checkpoints to save progress at each step
- Handles errors and resource cleanup

## Communication Flow

1. User interacts with the webview UI
2. Webview sends messages to Controller via gRPC
3. Controller creates a Task instance
4. Task executes operations and returns results through Controller
5. Controller updates the webview state accordingly

This architecture ensures separation of concerns, maintainable codebase, and robust state management across components.
