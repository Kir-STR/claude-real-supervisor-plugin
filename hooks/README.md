# Plugin Hooks

This directory contains plugin-level hooks that are automatically activated when the Real Supervisor plugin is enabled.

## Hooks Overview

The Real Supervisor plugin defines the following hooks:

### SubagentStop Hook
**Event**: Triggered when a subagent completes execution
**Type**: Prompt-based
**Purpose**: Logs subagent outputs to the supervisor session log for complete audit trail

This hook ensures that all worker agent outputs are captured in `.supervisor/session_log.json`, providing transparency and enabling session resumption.

### SessionStart Hook
**Event**: Triggered when a new Claude Code session starts
**Type**: Prompt-based
**Purpose**: Checks for interrupted supervisor sessions that can be resumed

When you start Claude Code with this plugin active, it automatically detects any `.supervisor/state.json` files from previous sessions and prompts you to resume if applicable.

### PostToolUse Hook (Write|Edit)
**Event**: Triggered after Write or Edit tool usage
**Type**: Prompt-based
**Purpose**: Validates files in the `.supervisor/` directory

Ensures that state files (`state.json`, `session_log.json`) and other supervisor artifacts are properly formatted as valid JSON or Markdown, preventing corruption.

## Hook Structure

Plugin hooks use the standard Claude Code hooks format with these characteristics:

- **Automatic activation**: Hooks are enabled when the plugin is loaded
- **Environment variables**: Access to `${CLAUDE_PLUGIN_ROOT}` and `${CLAUDE_PROJECT_DIR}`
- **Merged execution**: Run alongside user and project hooks for the same events
- **Scoped behavior**: Only active when working with Real Supervisor workflows

## Customizing Hooks

To add or modify hooks:

1. Edit `hooks/hooks.json`
2. Follow the standard hook format:
   ```json
   {
     "EventName": [
       {
         "matcher": "ToolPattern",  // Optional for tool-based events
         "hooks": [
           {
             "type": "prompt",
             "prompt": "Your prompt instructions"
           }
         ]
       }
     ]
   }
   ```
3. Reload plugins in Claude Code: `/reload-plugins`

## Available Events

Plugin hooks can respond to these events:

- **PreToolUse**: Before a tool is invoked
- **PostToolUse**: After a tool completes
- **PermissionRequest**: When permissions are requested
- **Notification**: On notification events
- **UserPromptSubmit**: When user submits a prompt
- **Stop**: When main session stops
- **SubagentStop**: When a subagent completes (currently used)
- **PreCompact**: Before conversation compaction
- **Setup**: During initial setup
- **SessionStart**: When session begins (currently used)
- **SessionEnd**: When session ends

## Hook Types

### Prompt-Based Hooks
```json
{
  "type": "prompt",
  "prompt": "Instructions for Claude to follow"
}
```
Best for: Logging, validation, state management

### Command-Based Hooks
```json
{
  "type": "command",
  "command": "${CLAUDE_PLUGIN_ROOT}/scripts/script.sh",
  "timeout": 30
}
```
Best for: External tool integration, file processing, automation

## Best Practices

1. **Keep hooks focused**: Each hook should do one thing well
2. **Use descriptive prompts**: Clear instructions produce better results
3. **Set appropriate timeouts**: For command hooks, allow enough time
4. **Test thoroughly**: Verify hooks work correctly after changes
5. **Document behavior**: Update this README when adding new hooks

## Debugging Hooks

If hooks aren't behaving as expected:

1. Check hook syntax in `hooks.json`
2. Verify the plugin is loaded: `/plugins`
3. Test with `/reload-plugins`
4. Check Claude Code logs for errors
5. Ensure file paths use `${CLAUDE_PLUGIN_ROOT}` correctly

## References

- [Claude Code Hooks Documentation](https://code.claude.com/docs/en/hooks)
- [Plugin Development Guide](../CLAUDE.md)
- [Plugin Configuration](../.claude-plugin/plugin.json)
