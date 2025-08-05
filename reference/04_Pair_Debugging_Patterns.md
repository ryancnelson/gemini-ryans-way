# Ryan's Pair Programming & Debugging Patterns

*Part of ryans-way modular pattern system*

## Systems Architect / Sysops Manager Context

Ryan is a systems architect, hacker, and sysops manager who:
- Spends extensive time debugging on command line in Linux/Unix
- Works across many remote systems simultaneously  
- Uses tmux sessions for persistent, shareable debugging workflows
- Wants AI assistants to "pair program" and "pair devops" in real-time
- Prefers collaborative terminal-based workflows over GUI tools

## Tmux Pair Debugging Workflow

### Session Management
```bash
# Create named debugging session
tmux new-session -d -s debug-$(date +%Y%m%d-%H%M)

# Share session for pair debugging
tmux attach-session -t debug-session

# Create split panes for collaborative work
tmux split-window -h    # horizontal split
tmux split-window -v    # vertical split

# Multiple windows for different systems/contexts
tmux new-window -n "server1-logs"
tmux new-window -n "server2-debug"
tmux new-window -n "monitoring"
```

### AI-Assisted Debugging Protocol
1. **Document the problem** in real-time:
   ```bash
   echo "DEBUGGING: $(date) - Problem: [description]" | tee debug-log.md | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"
   ```

2. **Multi-model analysis** for complex issues:
   ```bash
   # Deep technical analysis
   llm -m o1 "Systems problem: [paste error/symptoms]"
   
   # Implementation guidance
   llm -m gpt-4o "Debugging steps for: [specific issue]"
   
   # Quick syntax/command help
   llm -m gpt-4o-mini "Linux command syntax: [tool/command]"
   ```

3. **Share context** with AI assistant:
   ```bash
   # Capture system state
   {
     echo "=== SYSTEM CONTEXT ===" 
     uname -a
     ps aux | head -10
     df -h
     free -h
     echo "=== ERROR CONTEXT ==="
     cat error.log | tail -20
   } | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"
   ```

4. **Store solutions** for future reference:
   ```bash
   echo "SOLVED: $(date) - Issue: [title]
   Problem: [description]
   Solution: [commands/fix]
   Root cause: [analysis]" | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"
   ```

## Remote Systems Debugging

### Multi-System Context
```bash
# Track which systems you're debugging across
tmux rename-window "$(hostname -s)-debug"

# Create system-specific panes
tmux split-window -h "ssh server1"
tmux split-window -v "ssh server2" 
tmux split-window -h "ssh server3"

# Broadcast commands to multiple panes (when safe)
tmux setw synchronize-panes on   # CAREFUL - affects ALL panes
```

### Log Monitoring & Analysis
```bash
# Real-time log following across systems
tmux new-window -n "logs"
tmux send-keys "ssh server1 'tail -f /var/log/app.log'" Enter
tmux split-window -v
tmux send-keys "ssh server2 'tail -f /var/log/app.log'" Enter

# Pattern matching across logs
ssh server1 "grep -E 'ERROR|WARN|FATAL' /var/log/*.log | tail -20"
```

## AI Pair Programming Commands

### Context-Aware Assistance
When AI assistant joins debugging session, it should:

1. **Get oriented** with current tmux layout:
   ```bash
   tmux list-sessions
   tmux list-windows -t debug-session
   tmux list-panes -t debug-session
   ```

2. **Understand the problem** from context:
   - Check recent Cognee entries for this debugging session
   - Examine error logs and system state captures
   - Review command history and current processes

3. **Collaborate effectively**:
   - Suggest commands but explain reasoning
   - Identify patterns across multiple systems
   - Recommend monitoring/alerting improvements
   - Help document solutions for knowledge base

### Debugging Session Templates

#### Network Issue Debugging
```bash
# Create structured debugging session
tmux new-session -d -s net-debug
tmux new-window -n "connectivity"
tmux new-window -n "dns"  
tmux new-window -n "routing"
tmux new-window -n "monitoring"

# Populate with relevant commands
tmux send-keys -t net-debug:connectivity "ping 8.8.8.8" Enter
tmux send-keys -t net-debug:dns "nslookup target.com" Enter
tmux send-keys -t net-debug:routing "traceroute target.com" Enter
```

#### Application Performance Debugging
```bash
tmux new-session -d -s perf-debug
tmux new-window -n "top"
tmux new-window -n "logs"
tmux new-window -n "network"
tmux new-window -n "disk"

tmux send-keys -t perf-debug:top "htop" Enter
tmux send-keys -t perf-debug:network "iftop" Enter  
tmux send-keys -t perf-debug:disk "iotop" Enter
```

## Integration with Knowledge Base

### Context Triggers
Load this pattern when AI detects:
- Mentions of "tmux", "pair programming", "debugging", "sysops"
- Multi-system problem descriptions
- Need for collaborative terminal workflows
- Systems architecture or infrastructure issues

### Token Efficiency
- **Full pattern**: ~600 tokens for complex debugging scenarios
- **Essential commands**: ~200 tokens for basic tmux/debugging
- **Context-specific**: Load only relevant debugging template

---

*This pattern enables AI assistants to effectively collaborate in Ryan's terminal-based debugging workflows across multiple remote systems.*