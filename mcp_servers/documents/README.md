# Rls Docs MCP Server

A Python-based framework for rapidly developing Model Context Protocol (MCP) servers

## Tools (Default Mode)

These are the individual tools available by default:

### 1. `create_document`

Create a new .docx document composed of structured content blocks.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `directory` | string | Yes | Directory path |
| `file_name` | string | Yes | Output filename |
| `content` | array[object] | Yes | List of content blocks |
| `metadata` | object | No | Optional document metadata |

---

### 2. `delete_document`

Delete a .docx document from the filesystem.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `file_path` | str | _required_ | - |

---

### 3. `get_document_overview`

Get a structural overview of a .docx document with annotated headings.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |

---

### 4. `read_document_content`

Parse a .docx document into structured content with stable identifiers.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `section_index` | integer | No | Optional section index to read |

---

### 5. `read_image`

Read an image from document using file path and annotation key.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the document file |
| `annotation` | string | Yes | Image annotation key |

---

### 6. `add_content_text`

Insert text at a run, paragraph, or cell identifier.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `identifier` | string | Yes | Target element identifier |
| `text` | string | Yes | Text to insert |
| `position` | string | No | Insert position. Default: "end" |

---

### 7. `edit_content_text`

Replace text content at a specific identifier in a .docx document.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `identifier` | string | Yes | Target element identifier |
| `new_text` | string | Yes | Replacement text |

---

### 8. `delete_content_text`

Delete text or remove elements by identifier.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `identifier` | string | Yes | Target element identifier |
| `scope` | string | No | Deletion scope. Default: "content" |
| `collapse_whitespace` | boolean | No | Collapse whitespace after deletion. Default: false |

---

### 9. `add_image`

Add an image to a document at the specified location.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `image_path` | string | Yes | Path to the image file |
| `identifier` | string | Yes | Target element identifier |
| `position` | string | No | Insert position. Default: "end" |
| `width` | number | No | Image width in inches |
| `height` | number | No | Image height in inches |

---

### 10. `modify_image`

Modify an existing image in a document.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `image_index` | integer | Yes | Index of the image to modify |
| `operation` | string | Yes | Operation type (rotate, flip, brightness, contrast) |
| `rotation` | integer | No | Rotation degrees |
| `flip` | string | No | Flip direction (horizontal, vertical) |
| `brightness` | number | No | Brightness adjustment |
| `contrast` | number | No | Contrast adjustment |

---

### 11. `apply_formatting`

Apply text formatting to a targeted element by identifier.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `identifier` | string | Yes | Target element identifier |
| `bold` | boolean | No | Apply bold formatting |
| `italic` | boolean | No | Apply italic formatting |
| `underline` | boolean | No | Apply underline formatting |
| `strikethrough` | boolean | No | Apply strikethrough formatting |
| `font_size` | number | No | Font size in points |
| `font_color` | string | No | Font color (hex code) |

---

### 12. `page_margins`

Read and modify page margins in Documents documents.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `action` | string | Yes | Action: "read" or "set" |
| `section_index` | integer | No | Optional section index to modify |
| `top` | number | No | Top margin in inches |
| `bottom` | number | No | Bottom margin in inches |
| `left` | number | No | Left margin in inches |
| `right` | number | No | Right margin in inches |

---

### 13. `page_orientation`

Read and modify page orientation in Documents documents.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `action` | string | Yes | Action: "read" or "set" |
| `section_index` | integer | No | Optional section index to modify |
| `orientation` | string | No | Orientation: "portrait" or "landscape" |

---

### 14. `header_footer`

Create, read, and modify headers and footers in Documents documents.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `action` | string | Yes | Action: "read", "set", "clear", or "link" |
| `area` | string | Yes | Area: "header" or "footer" |
| `section_index` | integer | No | Optional section index to modify |
| `content` | array[object] | No | Content blocks for "set" action |
| `link_to_previous` | boolean | No | Link to previous section for "link" action |

---

### 15. `comments`

Read, add, and delete comments in Documents documents.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .docx file |
| `action` | string | Yes | Action: "read", "add", or "delete" |
| `identifier` | string | No | Target element identifier for "add" action |
| `text` | string | No | Comment text for "add" action |
| `author` | string | No | Comment author for "add" action |
| `comment_id` | integer | No | Comment ID for "delete" action |

---

## Consolidated Tools

When using consolidated mode, these meta-tools combine multiple operations:

### 1. `docs`

Document operations: create, read, edit, and manage .docx files.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `action` | enum['help', 'create', 'delete', 'overview', 'read_content', 'read_image', 'add_text', 'edit_text', 'delete_text', 'add_image', 'modify_image', 'format'] | Ellipsis | Action to perform |
| `file_path` | string? | null | Full file path. REQUIRED for file operations. |
| `directory` | string? | null | Directory for 'create' (e.g., '/') |
| `file_name` | string? | null | File name for 'create' (e.g., 'report.docx') |
| `content` | array[object[string, Any]]? | null | Content blocks for 'create': [{type, text, ...}] |
| `metadata` | object[string, Any]? | null | Document metadata for 'create': {title?, author?, ...} |
| `identifier` | string? | null | Stable identifier from read_content (e.g., 'body.p.0') |
| `text` | string? | null | Text content for add_text |
| `new_text` | string? | null | Replacement text for edit_text |
| `position` | string? | null | Position for add_text/add_image: 'start' or 'end' |
| `scope` | string? | null | Scope for delete_text: 'content' or 'element' |
| `collapse_whitespace` | boolean? | null | Collapse whitespace for delete_text in cells |
| `section_index` | integer? | null | Section index for read_content pagination |
| `annotation` | string? | null | Image annotation key for read_image |
| `image_path` | string? | null | Path to image file for add_image |
| `image_index` | integer? | null | 0-based image index for modify_image |
| `operation` | string? | null | Operation for modify_image: rotate, flip, brightness, contrast |
| `rotation` | integer? | null | Rotation angle (0-360) |
| `flip` | string? | null | Flip direction: 'horizontal' or 'vertical' |
| `brightness` | number? | null | Brightness factor (0.0-2.0). 1.0=unchanged. |
| `contrast` | number? | null | Contrast factor (0.0-2.0). 1.0=unchanged. |
| `width` | number? | null | Width in pixels. Optional for export. |
| `height` | number? | null | Height in pixels. Optional for export. |
| `bold` | boolean? | null | Apply bold formatting. |
| `italic` | boolean? | null | Apply italic formatting. |
| `underline` | boolean? | null | Underline formatting |
| `strikethrough` | boolean? | null | Strikethrough formatting |
| `font_size` | number? | null | Font size in points. |
| `font_color` | string? | null | Font color as hex (e.g., 'FF0000') |

---

### 2. `docs_schema`

Get JSON schema for docs input/output models.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `model` | string | Ellipsis | Model name: 'input', 'output', or a result type |

---
