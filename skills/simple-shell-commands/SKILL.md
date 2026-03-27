---
name: simple-shell-commands
description: "Use run_shell for straightforward command execution instead of shell_agent when commands are simple and don't require autonomous error recovery or complex scripting"
---

# Simple Shell Commands Skill

## When to Use run_shell vs shell_agent

Use `run_shell` when:
- You have a **simple, straightforward command** that you know will work
- The command is **short and self-contained** (e.g., `echo`, `ls`, `cat`, `pwd`)
- You don't need **automatic error recovery** or complex logic
- You want **direct execution** without the overhead of an intelligent agent
- The command is **deterministic** and doesn't require conditional execution

Use `shell_agent` when:
- You need **autonomous error recovery** and retry logic
- The task requires **complex scripting** or conditional execution
- You need the agent to **figure out how to accomplish a goal** (not just execute a command)
- The task involves **multiple steps** that need coordination
- You want **automatic debugging** and iteration

## Examples

### ✅ Good use of run_shell:
```python
# Simple echo command
run_shell("echo hello world")

# List directory contents
run_shell("ls -la")

# Check current directory
run_shell("pwd")

# Read a file
run_shell("cat config.txt")

# Simple file operation
run_shell("cp source.txt destination.txt")
```

### ❌ Inappropriate use of run_shell:
```python
# Complex task that requires error handling
run_shell("compile_and_test_project.sh")  # Use shell_agent instead

# Task that might fail and need recovery
run_shell("download_from_unreliable_source")  # Use shell_agent instead
```

### ✅ Appropriate use of shell_agent:
```python
# Complex task requiring error recovery
shell_agent("Compile the project, run tests, and fix any compilation errors")

# Multi-step process
shell_agent("Download data from API, process it, and save results to database")

# Task requiring autonomous problem-solving
shell_agent("Figure out why the service isn't starting and fix it")
```

## Decision Flow

1. **Is the command a single, simple operation?** → Use `run_shell`
2. **Do you need automatic error recovery?** → Use `shell_agent`
3. **Is the command deterministic and known to work?** → Use `run_shell`
4. **Does the task require figuring out how to do something?** → Use `shell_agent`
5. **Is it just executing a known command?** → Use `run_shell`

## Performance Considerations

- `run_shell` is **faster** for simple commands (direct execution)
- `shell_agent` has **overhead** but provides intelligence and error recovery
- For batch operations of simple commands, prefer multiple `run_shell` calls
- For complex workflows, prefer a single `shell_agent` call

## Best Practices

1. **Keep it simple**: If you can write the exact command, use `run_shell`
2. **Know your tools**: Understand what each shell function is optimized for
3. **Minimize overhead**: Don't use `shell_agent` for tasks it doesn't need
4. **Be explicit**: Write clear, complete commands for `run_shell`
5. **Test first**: If unsure, try `run_shell` first and only use `shell_agent` if you need its features