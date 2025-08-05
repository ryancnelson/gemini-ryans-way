# Ryan's Way - Storage Architecture

## Problem Statement
Current approach loads 4,000 tokens of patterns every session (40% overhead). Need sub-100ms pattern retrieval with 85% token reduction.

## Solution: SQLite3 Cache with Markdown Sources

### Storage Layers

#### Layer 1: Human-Editable Sources (Git-tracked)
```
patterns/
├── filesearch.md      # File search patterns (250 tokens)
├── network.md         # Tailscale, SSH patterns (400 tokens)  
├── git.md             # Git and Keybase patterns (300 tokens)
├── tools.md           # Modern tool alternatives (200 tokens)
├── transfer.md        # Magic wormhole, secure transfer (250 tokens)
├── infrastructure.md  # Host topology, services (350 tokens)
├── dev.md            # Development workflows (300 tokens)
└── ...               # Additional patterns as needed
```

#### Layer 2: Fast Retrieval Cache (Git-tracked)
```
patterns.db           # SQLite3 database (~50KB)
```

**Schema**:
```sql
CREATE TABLE patterns (
    id TEXT PRIMARY KEY,           -- 'filesearch', 'network', etc.
    content TEXT,                  -- Full pattern markdown
    token_count INTEGER,           -- Estimated tokens
    tags TEXT,                     -- 'search,find,grep,rg,mdfind'
    category TEXT,                 -- 'file-ops', 'network', etc.
    last_updated TIMESTAMP,        -- Source file modification time
    usage_count INTEGER DEFAULT 0, -- Usage tracking
    priority INTEGER DEFAULT 1     -- Loading priority
);

CREATE INDEX idx_tags ON patterns(tags);
CREATE INDEX idx_category ON patterns(category);
```

#### Layer 3: Sync & Management
```
sync.py               # Rebuild cache from markdown sources
query.py              # Fast pattern retrieval interface
stats.py              # Usage analytics and optimization
```

### Query Interface

#### Primary Queries (Sub-20ms)
```python
# Direct pattern lookup
get_pattern('filesearch') → returns markdown content (250 tokens)

# Tag-based search  
find_patterns('git,keybase') → returns [git.md content]

# Multi-pattern loading
get_patterns(['network', 'infrastructure']) → returns combined content
```

#### Usage Examples
```python
# For AI Assistant
import ryans_way
patterns = ryans_way.get_patterns_for_task("file searching")
# Returns: filesearch.md content (250 tokens)

# Context-aware loading  
patterns = ryans_way.get_patterns_for_directory("/git/myproject")  
# Returns: git.md + dev.md content (600 tokens)

# Fallback query
patterns = ryans_way.get_all_patterns()  
# Returns: Full ryans-way.md content (4000 tokens) - emergency only
```

### Performance Characteristics

#### Speed Benchmarks (Target)
- Pattern lookup: <10ms (SQLite3 indexed query)
- Multi-pattern: <20ms (batch query + concatenation)  
- Cache rebuild: <100ms (parse all markdown + insert)
- Database size: <100KB (all patterns + metadata)

#### Token Economics
```
Current approach:     4,000 tokens/session (100% overhead)
Single pattern:        200-400 tokens/session (5-10% overhead)  
Multi-pattern:         600-800 tokens/session (15-20% overhead)
Emergency fallback:   4,000 tokens/session (rare, <1% of sessions)

Average savings: 85% token reduction
```

### Maintenance Workflow

#### Daily Usage
```bash
# AI Assistant workflow  
1. Load ryans-way.init.md (200 tokens)
2. Query pattern database based on task
3. Load only relevant patterns (200-500 tokens)
4. Proceed with task using loaded context
```

#### Pattern Updates  
```bash
# Human maintenance workflow
1. Edit patterns/filesearch.md (human-readable markdown)
2. Run: python sync.py (rebuilds cache in <100ms)
3. Git commit patterns/ + patterns.db  
4. Changes available immediately to all AI sessions
```

#### Cache Management
```bash
# Periodic maintenance
python stats.py              # Show usage analytics  
python sync.py --verify      # Ensure cache matches sources
python sync.py --rebuild     # Full cache rebuild
```

### Directory Structure
```
ryans-way/
├── README.md                 # Project overview
├── ARCHITECTURE.md          # This file
├── ryans-way.md            # Master reference (comprehensive)
├── ryans-way.init.md       # Pattern dispatcher
├── patterns/               # Human-editable sources
│   ├── filesearch.md
│   ├── network.md
│   ├── git.md
│   ├── tools.md
│   ├── transfer.md
│   ├── infrastructure.md
│   └── dev.md
├── patterns.db            # SQLite3 cache (fast queries)
├── sync.py                # Cache management
├── query.py               # Pattern retrieval interface  
├── stats.py               # Usage analytics
└── tests/                 # Test suite
    ├── test_query.py
    ├── test_sync.py
    └── benchmark.py
```

### Benefits

#### For AI Assistants
- **Fast startup**: Load only needed patterns (200-500 tokens vs 4,000)
- **Relevant context**: No irrelevant information cluttering working memory
- **Reliable fallback**: System never fails, graceful degradation
- **Consistent behavior**: Same patterns across all AI clients

#### For Human Maintenance  
- **Easy editing**: Patterns in human-readable markdown
- **Version control**: Everything tracked in git
- **Analytics**: See which patterns are used most
- **Extensible**: Add new patterns by creating markdown files

#### For System Performance
- **85% token reduction**: Dramatically lower context overhead
- **Sub-100ms queries**: Feels instant to AI and human
- **Portable**: Single SQLite3 file, works anywhere
- **Scalable**: Can handle hundreds of patterns efficiently

### Implementation Priority

#### Phase 1: Core Infrastructure
1. Create patterns/ directory structure  
2. Extract first few patterns from ryans-way.md
3. Build sync.py and query.py
4. Test basic retrieval performance

#### Phase 2: Pattern Migration
1. Split ryans-way.md into focused pattern files
2. Populate SQLite3 cache
3. Add usage tracking and analytics
4. Validate token count estimates

#### Phase 3: AI Integration  
1. Update AI workflow to use new system
2. Add context-aware pattern loading
3. Performance testing and optimization
4. Documentation for AI assistant usage

This architecture provides the fast, efficient, maintainable pattern system you need while preserving the comprehensive knowledge in a token-efficient way.