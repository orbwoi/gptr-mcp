# Changelog - orbwoi/gptr-mcp

This is a fork of [assafelovic/gptr-mcp](https://github.com/assafelovic/gptr-mcp) with modifications for clean Markdown output.

## Modifications

### 2026-02-15: HTML Stripping for Clean Markdown Output

**Problem:** The original MCP server returned raw HTML entities and tags in search results and research context, making output harder to read and wasting tokens.

**Solution:** Added HTML stripping to all tool outputs.

**Files Modified:**

| File | Changes |
|------|---------|
| `utils.py` | Added `strip_html()`, `clean_search_results()`, `clean_context()` functions |
| `server.py` | Applied HTML stripping to `quick_search`, `deep_research`, `get_research_context`, and resource outputs |

**New Functions:**

```python
def strip_html(text: str) -> str:
    """Decode HTML entities and remove HTML tags."""
    
def clean_search_results(results: List[Dict]) -> List[Dict]:
    """Clean HTML from DuckDuckGo search snippets."""
    
def clean_context(context: Any) -> str:
    """Clean HTML from research context (list or string)."""
```

**Behavior Changes:**

| Tool | Before | After |
|------|--------|-------|
| `quick_search` | Raw HTML entities (`\u00b7`, `&amp;`) | Clean text (`·`, `&`) |
| `deep_research` | May contain HTML tags | Clean Markdown-ready text |
| `get_research_context` | Raw scraped content | HTML-stripped content |
| `research://` resource | Raw context | Clean context |

**Benefits:**

- Clean Markdown output suitable for LLM consumption
- No HTML noise in responses
- Better token efficiency
- No configuration needed - automatic

## Configuration

This fork includes Ollama support. Set environment variables:

```env
LLM_PROVIDER=ollama
FAST_LLM=ollama:<model>
SMART_LLM=ollama:<model>
OLLAMA_BASE_URL=http://localhost:11434
EMBEDDING=ollama:nomic-embed-text
OLLAMA_EMBEDDING_MODEL=nomic-embed-text
RETRIEVER=duckduckgo
```

## Syncing with Upstream

```bash
git fetch upstream
git merge upstream/master
```

## License

MIT (same as upstream)