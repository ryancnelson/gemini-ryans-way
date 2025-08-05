# My AI Tools & Debugging Hacks

## Cognee Knowledge Store (SSH Access)

### Store Information in Cognee
```bash
# Method 1: Pipe content directly
echo "Your information here" | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"

# Method 2: Send file content  
cat your-file.txt | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"

# Method 3: Multiline content with heredoc
cat << 'EOF' | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"
# Your Title
Multiple lines of content
- Bullet points
- Technical details
- Code examples
EOF
```

### Search Cognee Knowledge
```bash
# Basic search
ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-search 'your search query'"

# Examples
ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-search 'MIDI controller debugging'"
ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-search 'AWS credentials configuration'"
ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-search 'how to use llm CLI tool'"
```

### Cognee Processing
```bash
# After adding content, run this to update the knowledge graph
ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-cognify"
```

## LLM CLI Tool - Multiple AI Models

### Available Models
```bash
# List all available models
llm models

# Key models for different tasks:
# - gpt-4o: Latest GPT-4 for general tasks
# - o1: Advanced reasoning and technical problems  
# - gpt-4o-mini: Fast, cost-effective for simple tasks
# - nova-pro: Amazon's Bedrock model
```

### Basic Usage
```bash
# Default model (gpt-4o-mini)
llm "Your question here"

# Specify model
llm -m o1 "Complex technical debugging question"
llm -m gpt-4o "General coding help"
llm -m nova-pro "Alternative perspective on problem"
```

### Specialized Debugging Queries
```bash
# Hardware/MIDI debugging with o1 (excellent for technical reasoning)
llm -m o1 "AKAI APC MINI MIDI controller issue: LEDs stopped working after USB reset. MIDI I/O works. Need SysEx initialization sequence."

# Code review with GPT-4o
llm -m gpt-4o "Review this Python MIDI code for potential issues: [paste code]"

# Quick syntax help with mini
llm -m gpt-4o-mini "Python rtmidi send SysEx message syntax"
```

### Multi-Model Consultation Strategy
```bash
# 1. Get initial analysis from o1 (best reasoning)
llm -m o1 "Detailed technical problem description..."

# 2. Get implementation details from gpt-4o  
llm -m gpt-4o "Based on this solution approach, show me the exact code implementation..."

# 3. Get alternative perspective from Bedrock
llm -m nova-pro "Different angle on this same technical problem..."
```

## MCP CLI Tool - Extended Capabilities

### Basic MCP Usage
```bash
# Use MCP with various servers (Wolfram, Filesystem, Slack, etc.)
mcp [command] [options]

# Integration with LLM
llm -m gpt-4o "Question that might need web search or file access" # (uses MCP automatically in some setups)
```

### MCP + LLM Integration
According to Cognee knowledge:
- `mcptools` works with MCP servers like Wolfram, Filesystem, Slack, OPNsense
- Use the `mcp_llm_integration.sh` script for shell-based integration
- Enables LLM access to external tools and data sources

## AWS Credentials Integration

### Setup for LLM/MCP Tools
```bash
# Ensure AWS credentials are configured
cat ~/.aws/credentials
cat ~/.aws/config

# LLM tools should automatically use these for AWS-based models (like Bedrock Nova)
llm -m nova-pro "Question using AWS Bedrock model"
```

## Practical Debugging Workflow

### Step 1: Document the Problem
```bash
# Store current issue in Cognee for context
echo "Current debugging session: [problem description]
- Symptoms: [list symptoms]  
- Attempted solutions: [list attempts]
- Current hypothesis: [your theory]" | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"
```

### Step 2: Multi-Model Analysis
```bash
# Get reasoning-heavy analysis
llm -m o1 "Technical problem requiring deep analysis: [detailed description]"

# Get practical implementation  
llm -m gpt-4o "Based on this diagnosis, show me step-by-step implementation"

# Get alternative perspective
llm -m nova-pro "Different approach to this same problem"
```

### Step 3: Search Existing Knowledge
```bash
# Check if you've solved similar problems before
ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-search 'similar keywords from current problem'"
```

### Step 4: Document Solution
```bash
# Store successful solution for future reference
cat << 'EOF' | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"
# SOLVED: [Problem Title]

## Issue: 
[Brief description]

## Root Cause:
[What was actually wrong]  

## Solution:
[Exact steps/code that worked]

## Key Learning:
[What to remember for next time]
EOF
```

## Advanced Tips

### Model Selection Guide
- **o1**: Complex reasoning, debugging, system analysis
- **gpt-4o**: Code implementation, detailed explanations  
- **gpt-4o-mini**: Quick answers, syntax help, simple tasks
- **nova-pro**: Alternative perspectives, AWS-specific questions
- **gemini-2.5-pro**: Google's most capable model, excellent for analysis
- **gemini-2.0-flash**: Fast Google model, good balance of speed/capability
- **gemini-1.5-pro-latest**: Large context window (1M tokens), multimodal

### Google Gemini Models
You have access to Google's Gemini models via API key in `gemini.api.key.txt`:

```bash
# Method 1: Direct llm command
export LLM_GEMINI_KEY=$(cat gemini.api.key.txt)
llm -m gemini-2.5-pro "Your question here"

# Method 2: Using helper script (recommended)
./gemini_helper.sh "Your question here"
./gemini_helper.sh gemini-2.5-pro "Complex analysis task"
```

**Key Gemini Models Available:**
- `gemini-2.5-pro` - Latest, most capable model
- `gemini-2.5-flash` - Fast, high-performance version
- `gemini-2.0-flash` - Good balance (default in helper script)
- `gemini-1.5-pro-latest` - 1 million token context window
- `gemini-2.0-flash-thinking-exp-*` - Experimental reasoning models

**Gemini Strengths:**
- **Massive context window** (1M tokens in 1.5 Pro) - can process entire codebases
- **Native multimodality** - handles text, images, video, audio seamlessly
- **Fast processing** with Mixture-of-Experts architecture
- **Different perspective** from OpenAI models for cross-validation

### Cognee Search Optimization
- Use specific technical terms
- Include error messages or symptoms
- Search for patterns, not exact matches
- Store solutions immediately when found

### Chaining Tools for Complex Problems
```bash
# 1. Search existing knowledge
ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-search 'MIDI LED control'"

# 2. Get expert analysis if no existing solution
llm -m o1 "Complex technical analysis needed..."

# 3. Get implementation details
llm -m gpt-4o "Show me exact code for solution..."

# 4. Store complete solution
echo "[Complete solution]" | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"
```

---

These tools provide a powerful debugging and development workflow by combining:
- **Personal knowledge store** (Cognee) for learning from past solutions
- **Multiple AI models** (LLM CLI) for different types of analysis  
- **Extended capabilities** (MCP) for accessing external resources

**Key insight**: Different AI models excel at different tasks - use the right tool for each step of the problem-solving process.