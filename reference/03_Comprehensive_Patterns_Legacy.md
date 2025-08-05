# Ryan's Way - Preferred Tools & Patterns

## Core Philosophy
Use the fastest, most efficient tools available on each platform. Prefer modern alternatives to traditional Unix tools when they exist.

## macOS-Specific Optimizations

### File Search - Use Spotlight (mdfind) First
```bash
# FAST: Use Spotlight index for file searches (if indexed)
mdfind -name "filename.ext"              # Find by filename
mdfind "text content"                    # Find by file content
mdfind -onlyin ~/devel "search term"     # Limit to specific directory

# When mdfind fails or for unindexed locations, fall back to find
find ~/devel -name "*.py" -type f
```

### Text Search - Use ripgrep (rg)
```bash
# PREFERRED: ripgrep is much faster than grep
rg "pattern" ~/devel/                    # Search in directory
rg -i "case insensitive"                 # Case insensitive
rg --type js "function"                  # Filter by file type
rg -A 5 -B 5 "context pattern"          # Show context lines

# AVOID: Traditional grep unless necessary
# grep -r "pattern" ~/devel/
```

### Directory Navigation
```bash
# PREFERRED: Modern tools
eza -la                                  # Better ls (if installed)
ls -la                                   # Fallback

# PREFERRED: Use pushd/popd for directory stack
pushd ~/devel/project                    # Go and remember where you were
popd                                     # Return to previous location
```

## Universal Tool Preferences

### JSON Processing
```bash
# PREFERRED: jq for JSON
curl -s api.endpoint.com | jq '.data[]'

# AVOID: Manual parsing with grep/sed/awk for JSON
```

### File Viewing
```bash
# PREFERRED: bat (syntax highlighting)
bat filename.py                         # If installed
# FALLBACK: cat
cat filename.py
```

### Process Management
```bash
# PREFERRED: Modern process viewers
htop                                     # Better top (if installed)
ps aux | grep process_name               # Fallback
```

### File Transfer - Magic Wormhole
```bash
# PREFERRED: magic-wormhole for secure file transfer
# Send files (generates shareable code)
wormhole send filename.tar.gz            # Creates code like "94-councilman-obtuse"

# Receive files (AI assistant pattern - always auto-accept)
echo "Y" | wormhole receive CODE         # Auto-accepts transfer prompt

# Why this is better:
# - No SSH keys or complex networking required
# - Works across firewalls and NAT
# - End-to-end encrypted
# - Simple shareable codes
# - Perfect for one-time transfers between systems

# CRITICAL: Always use "echo Y |" pattern in automation
# - wormhole receive prompts for permission
# - Without auto-accept, AI assistants get stuck on interactive prompt
# - Codes are single-use only - consumed after first attempt
```

### Binary Execution - dbin (Run Any Tool Without Installation)
```bash
# STEP 1: Install dbin itself (one-time setup)
wget -qO- "https://raw.githubusercontent.com/xplshn/dbin/master/stubdl" | sh -s -- install dbin
# Creates ~/.local/bin/dbin for permanent use

# STEP 2: Use installed dbin (much cleaner)
DBIN="~/.local/bin/dbin"

# Search for available tools
$DBIN search cowsay                      # Find tools matching "cowsay"

# List all available tools  
$DBIN list | head -20                    # Show first 20 available tools

# Run tools temporarily (with caching)
$DBIN run cowsay/cowsay "Hello!"         # Downloads, caches, and runs
$DBIN run cowsay/cowsay "Again!"         # Runs from cache (faster)

# Install tools permanently to ~/.local/bin (user-only)
$DBIN install cowsay/cowsay              # Installs to ~/.local/bin/cowsay
~/.local/bin/cowsay "Now permanent!"     # Use directly

# FALLBACK: One-time usage without installing dbin
# wget -qO- "https://raw.githubusercontent.com/xplshn/dbin/master/stubdl" | sh -s -- run TOOL

# Why this is better:
# - Test tools without system installation
# - No sudo/admin rights needed  
# - User-only installation (~/.local/bin)
# - Thousands of tools available
# - Perfect for servers where you can't install packages
# - Caching for repeated temporary use
# - Install dbin once, use everywhere

# CLEANUP: Remove temporary tools after session (optional)
rm ~/.local/bin/cowsay                   # Remove specific installed tool
rm ~/.local/bin/dbin                     # Remove dbin itself if desired

# PATTERN: Always use full tool names from search results
# - Use "cowsay/cowsay" not just "cowsay"
# - Check exact names with search command first

# REQUIREMENTS: Only needs writable home directory
# - No admin/sudo privileges required
# - Perfect for restricted environments
# - Clean up after sessions to avoid clutter

# PLATFORM SUPPORT: Works on most Linux systems
# - Supports aarch64 and x86_64 architectures
# - Tested on various Linux distributions
# - More info: https://github.com/xplshn/dbin
```

## Search Strategy Hierarchy

### 1. File Location (macOS)
```bash
# Try in order:
mdfind -name "target-file"               # Spotlight first (fastest)
find ~/likely-location -name "*target*"  # Targeted find second
find / -name "*target*" 2>/dev/null      # Brute force last resort
```

### 2. Content Search
```bash
# Try in order:
rg "search-term" ~/project-dir/          # ripgrep first (fastest)
grep -r "search-term" ~/project-dir/     # grep fallback
mdfind "search-term"                     # Spotlight for indexed content
```

### 3. Command Discovery
```bash
# Find commands and binaries:
which command-name                       # Check if command exists
type command-name                        # Show command type/location
brew list | grep pattern                 # Find installed packages (macOS)
```

## Efficiency Patterns

### Before Using Slow Tools
```bash
# ALWAYS check if faster alternative exists:
# Instead of find, try mdfind first
# Instead of grep, try rg first  
# Instead of cat, try bat first
# Instead of ls, try eza first
```

### File Operations
```bash
# PREFERRED: Use absolute paths in scripts
/usr/bin/python3 script.py              # Avoid PATH ambiguity

# PREFERRED: Test existence before operations
[ -f "filename" ] && cat filename       # Check before reading
[ -d "dirname" ] && cd dirname          # Check before changing
```

### Shell Scripting
```bash
# PREFERRED: Fail fast
set -euo pipefail                        # Exit on errors, undefined vars, pipe failures

# PREFERRED: Use modern syntax
[[ condition ]] instead of [ condition ] # More robust conditionals
```

## Platform Detection
```bash
# Detect platform and adjust behavior
if [[ "$OSTYPE" == "darwin"* ]]; then
    # macOS - use mdfind, brew, etc.
    search_cmd="mdfind"
elif [[ "$OSTYPE" == "linux-gnu"* ]]; then
    # Linux - use locate, apt/yum, etc.
    search_cmd="find"
fi
```

## Integration with AI Assistants

### Teach AI These Preferences
When working with an AI assistant, establish these patterns:

1. **File searches**: "Use mdfind first on macOS, then find"
2. **Text searches**: "Use rg (ripgrep) instead of grep"
3. **Modern alternatives**: "Prefer bat over cat, eza over ls"
4. **Fail-fast scripting**: "Use set -euo pipefail in scripts"

### Quick AI Bootstrap Commands
```bash
# Show AI assistant Ryan's preferred tools
cat ryans-way.md

# Show what tools are available
which rg && echo "✅ ripgrep available" || echo "❌ install ripgrep"
which mdfind && echo "✅ mdfind available" || echo "❌ not on macOS"
which bat && echo "✅ bat available" || echo "❌ install bat"
```

### Environment Caching
```bash
# PREFERRED: Cache workstation environment for AI assistants
# Store complete environment inventory in ~/.config/ for quick reference

# Create cache directory
mkdir -p ~/.config

# Cache current environment (manual or automated)
cat > ~/.config/ryans-way.cache.json << 'EOF'
{
  "last_updated": "$(date -u +%Y-%m-%dT%H:%M:%SZ)",
  "hostname": "$(hostname)",
  "tools": { ... },
  "infrastructure": { ... },
  "network": { ... }
}
EOF

# Quick environment check from cache
jq '.tools.preferred_available' ~/.config/ryans-way.cache.json

# Benefits:
# - Avoid re-scanning environment on every AI session
# - Persistent knowledge of available tools and configurations  
# - Quick reference for infrastructure topology (Tailscale IPs, etc.)
# - Helps AI assistants make smarter tool choices immediately
# - Works across platforms where ~/.config/ is available
```

## Ryan's Infrastructure - Git & Remote Access

### Keybase Git Storage
```bash
# PREFERRED: All personal projects use Keybase git for secure, private storage
git remote add origin keybase://private/ryancnelson/project-name
git push -u origin main

# Projects are stored in Keybase encrypted filesystem
# Benefits: Always private, encrypted at rest, accessible across devices
```

### Tailscale Remote Access
```bash
# PREFERRED: SSH to tailscale hosts for keybase operations
ssh fjord                                # Primary development host
ssh minnie-2                            # Secondary development host  

# Both hosts have keybase CLI and can execute keybase commands
# Use for: keybase git operations, file sync, encrypted storage access
```

### Remote Git Operations
```bash
# Execute keybase git operations from any tailscale-connected workstation
ssh fjord "keybase git list"            # List keybase repositories
ssh fjord "cd /path && git push"        # Push to keybase from remote
ssh minnie-2 "keybase fs ls /private"   # Browse keybase filesystem

# Sync patterns (based on Cognee knowledge):
# - sync-to-keybase.sh: Push local changes to keybase
# - pull-from-keybase.sh: Pull updates from keybase repository
```

### Infrastructure Benefits
- **Security**: All code stored encrypted in Keybase
- **Accessibility**: Available from any tailscale-connected device
- **Redundancy**: Multiple hosts can access and sync repositories
- **Privacy**: No reliance on public git hosting for personal projects

## Ryan's Network Infrastructure

### TP-Link Deco Mesh Network
```bash
# Network topology (from Cognee knowledge):
# - TP-Link Deco routers provide mesh Wi-Fi coverage
# - Integrated with OPNsense firewall for advanced traffic management
# - Network segmentation: main LAN, guest network, IoT devices
# - Proper traffic management and network design

# Access patterns:
# - Seamless Wi-Fi coverage throughout area
# - Connected to OPNsense-managed networks
# - Supports network categories and traffic isolation
```

### Network Architecture Benefits
- **Coverage**: Mesh network eliminates dead zones
- **Segmentation**: Separate networks for main, guest, and IoT traffic
- **Management**: OPNsense integration for advanced firewall controls
- **Security**: Traffic isolation between network segments

## Adding New Patterns

To extend this knowledge base:

1. **Add new tools**: Document preferred vs fallback commands
2. **Add platform-specific**: Use platform detection patterns
3. **Add efficiency rules**: Always include "why this way is better"
4. **Add integration**: Show how AI assistants should use the pattern
5. **Add infrastructure**: Document remote access patterns and benefits

## Example: Adding a New Tool

```markdown
### New Category - [Tool Purpose]
```bash
# PREFERRED: [modern tool]
new-tool --options input                 # Reason why this is better

# FALLBACK: [traditional tool]  
old-tool options input                   # When to use this instead
```

This knowledge base becomes part of your AI assistant bootstrap - any assistant can quickly learn your preferred ways of doing things by ingesting this file.