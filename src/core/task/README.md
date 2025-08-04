# Task Module

The task module is responsible for executing individual tasks within the Cline extension, handling API requests, tool operations, and maintaining context throughout each task's lifecycle.

## Overview

Each task runs in isolation to ensure proper state management:

- Handles user input and processes it through APIs
- Executes tools as needed (file editing, command execution)
- Maintains a conversation history for context preservation
- Supports checkpoints to save progress at critical points

```tree
task/
├── index.ts          # Core task logic, API request handling
├── toolExecutor.ts   # Executes tools with approval handling
├── checkpoint.ts     # Handles saving and restoring task state
└── ...
```

## Key Components

### Task Lifecycle

1. **Initialization**: Task is created with initial context
2. **Execution Loop**:
   - Makes API request to external model
   - Processes response, extracting content blocks
   - Executes tools as needed with user approval
   - Preserves context for subsequent requests
3. **Completion/Interruption**: Saves final state and cleans up resources

### Context Management

Tasks maintain a conversation history that includes:
- User messages
- Assistant responses
- Tool execution results
- API request/responses

This ensures consistent context across the task's lifecycle, even during interruptions.

### Tool Execution

Tasks execute tools through a structured process:

1. Checks auto-approval settings
2. Requests user approval if needed
3. Executes tool and captures result
4. Saves checkpoint before proceeding

### Error Handling

The Task class includes robust error handling:
- Automatic retry for transient API errors
- User-prompted retries for persistent issues
- State cleanup on failure
- Resource release after task completion or interruption

## Technical Details

### Conversation History

Tasks maintain a conversation history with the following structure:

```typescript
type ConversationHistoryEntry = {
  role: "user" | "assistant" | "system";
  content: string;
  ts: number; // Timestamp
};
```

### API Request Management

The `attemptApiRequest` method handles:
- Context window management (truncating conversation when needed)
- Automatic retry for failed requests
- Error recovery with user prompts

## Best Practices

1. **Keep Tasks Focused**: Each task should focus on a single unit of work
2. **Handle Errors Gracefully**: Implement proper error handling and retries
3. **Manage Resources**: Clean up after task completion or interruption
4. **Preserve Context**: Maintain conversation history for context continuity
5. **Use Checkpoints**: Save progress at critical points to prevent data loss

By following these guidelines, tasks can be executed efficiently and reliably within the Cline extension.
