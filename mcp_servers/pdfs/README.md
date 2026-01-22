# Rls Pdf MCP Server

A Python-based framework for rapidly developing Model Context Protocol (MCP) servers

## Tools (Default Mode)

These are the individual tools available by default:

### 1. `create_pdf`

Create a new PDF document from structured content blocks.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `directory` | string | Yes | Directory path (must start with /) |
| `file_name` | string | Yes | PDF file name (must end with .pdf) |
| `content` | array[object] | Yes | List of content blocks (paragraph, heading, table, etc.) |
| `metadata` | object | No | Optional PDF metadata (title, subject, author) |
| `page_size` | string | No | Page size: "letter" or "a4". Default: "letter" |

---

### 2. `read_pdf_pages`

Extract text, images, and strikethrough annotations from PDF pages.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the PDF file (must start with /) |
| `pages` | array[integer] | No | List of page numbers to extract. Default: all pages |

---

### 3. `read_image`

Read an image extracted from a PDF using its annotation key.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the PDF file |
| `annotation` | string | Yes | Image annotation key from read_pdf_pages output |

---

### 4. `read_page_as_image`

Render a PDF page as an image.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the PDF file |
| `page_number` | integer | Yes | Page number to render |

---

### 5. `search_pdf`

Search for text within a PDF document.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the PDF file |
| `query` | string | Yes | Search query string |
| `case_sensitive` | boolean | No | Case-sensitive search. Default: false |

---

## Consolidated Tools

When using consolidated mode, these meta-tools combine multiple operations:

### 1. `pdf`

PDF operations: create, read, search, and extract images from .pdf files.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `action` | enum['help', 'create', 'read_pages', 'read_image', 'page_as_image', 'search'] | Ellipsis | Action to perform |
| `file_path` | string? | null | Absolute path to PDF file. REQUIRED for read_pages, read_image, page_as_image, search. |
| `directory` | string? | null | Directory for 'create' action. Use '/' for root. REQUIRED for create. |
| `file_name` | string? | null | File name for 'create' action (e.g., 'report.pdf'). REQUIRED for create. |
| `content` | array[object[string, Any]]? | null | Content blocks for 'create': [{type, text, ...}] |
| `metadata` | object[string, Any]? | null | PDF metadata for 'create': {title?, author?, ...} |
| `page_size` | string? | null | Page size for 'create': 'letter' or 'a4' |
| `pages` | array[integer]? | null | Page numbers to read (1-indexed), None for all |
| `page_number` | integer? | null | Page number for 'page_as_image' (1-indexed) |
| `annotation` | string? | null | Image annotation key for 'read_image' |
| `query` | string? | null | Search text. Matches names, descriptions. Case-insensitive. |
| `case_sensitive` | boolean? | null | Case-sensitive search (default: False) |
| `whole_documents` | boolean? | null | Match whole words only (default: False) |
| `max_results` | integer? | null | Maximum results to return (default: 100) |
| `context_chars` | integer? | null | Context characters around match (default: 50) |

---

### 2. `pdf_schema`

Get JSON schema for pdf input/output models.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `model` | string | Ellipsis | Model name: 'input', 'output', or a result type |

---
