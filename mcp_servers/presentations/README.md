# Rls Slides MCP Server

A Python-based framework for rapidly developing Model Context Protocol (MCP) servers

## Tools (Default Mode)

These are the individual tools available by default:

### 1. `create_deck`

Create a Presentations presentation from structured slide definitions.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `directory` | string | Yes | Directory path |
| `file_name` | string | Yes | Output filename ending with .pptx |
| `slides` | array[object] | Yes | List of slide definitions |
| `metadata` | object | No | Optional presentation metadata |

---

### 2. `delete_deck`

Delete a Presentations presentation.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .pptx file to delete |

---

### 3. `add_slide`

Add a new slide to a presentation at the specified index.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .pptx file |
| `slide_index` | integer | Yes | Index where to insert the new slide |
| `layout` | string | No | Slide layout type |
| `content` | object | No | Slide content definition |

---

### 4. `edit_slides`

Apply structured edit operations to an existing Presentations presentation.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .pptx file |
| `operations` | array[object] | Yes | List of edit operations |
| `metadata` | object | No | Optional metadata updates |

**Available edit operations:**

| Operation Type | Description | Key Parameters |
|----------------|-------------|----------------|
| `update_slide_title` | Update slide title | `index`, `title` |
| `update_slide_subtitle` | Update slide subtitle | `index`, `subtitle` |
| `set_bullets` | Set bullet points | `index`, `placeholder`, `items` |
| `append_bullets` | Append bullet points | `index`, `placeholder`, `items` |
| `clear_placeholder` | Clear placeholder content | `index`, `placeholder` |
| `replace_text` | Find and replace text | `search`, `replace`, `match_case` |
| `append_table` | Append a table | `index`, `placeholder`, `rows`, `header` |
| `update_table_cell` | Update table cell text | `index`, `table_idx`, `row`, `column`, `text` |
| `delete_slide` | Delete a slide | `index` |
| `duplicate_slide` | Duplicate a slide | `index`, `position` |
| `set_notes` | Set speaker notes | `index`, `notes` |
| `apply_text_formatting` | Apply text formatting | `index`, `placeholder`, `bold`, `italic`, `font_size`, `font_color` |
| `add_hyperlink` | Add clickable URL to text | `index`, `placeholder`, `url`, `paragraph_index`, `run_index` |
| `format_table_cell` | Format table cell styling | `index`, `table_idx`, `row`, `column`, `bg_color`, `font_color`, `bold` |

**add_hyperlink operation:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | Yes | Must be "add_hyperlink" |
| `index` | integer | Yes | Slide index (0-based) |
| `placeholder` | string | No | Placeholder: title, body, left, right. Default: "body" |
| `url` | string | Yes | The URL to link to |
| `paragraph_index` | integer | No | Paragraph index to add link to |
| `run_index` | integer | No | Run index within paragraph |

**format_table_cell operation:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | Yes | Must be "format_table_cell" |
| `index` | integer | Yes | Slide index (0-based) |
| `table_idx` | integer | Yes | Table index on the slide (0-based) |
| `row` | integer | Yes | Row index (0-based) |
| `column` | integer | Yes | Column index (0-based) |
| `bold` | boolean | No | Make text bold |
| `italic` | boolean | No | Make text italic |
| `underline` | boolean | No | Underline text |
| `font_size` | number | No | Font size in points |
| `font_color` | string | No | Font color as hex (e.g., "FF0000") |
| `bg_color` | string | No | Background color as hex (e.g., "FFFF00") |

---

### 5. `add_image`

Add an image to a slide at the specified position.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .pptx file |
| `image_path` | string | Yes | Path to the image file |
| `slide_index` | integer | Yes | Slide index |
| `x` | number | No | X position in inches. Default: 1.0 |
| `y` | number | No | Y position in inches. Default: 1.5 |
| `width` | number | No | Image width in inches |
| `height` | number | No | Image height in inches |

---

### 6. `modify_image`

Modify an existing image in a slide (rotate, flip, brightness, contrast, crop).

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .pptx file |
| `slide_index` | integer | Yes | Slide index |
| `image_index` | integer | Yes | Image index on the slide |
| `operation` | string | Yes | Operation type: rotate, flip, brightness, contrast, crop |
| `rotation` | integer | No | Rotation degrees (0-360). Required for rotate operation |
| `flip` | string | No | Flip direction: horizontal, vertical. Required for flip operation |
| `brightness` | number | No | Brightness factor (positive number, 1.0=unchanged). Required for brightness operation |
| `contrast` | number | No | Contrast factor (positive number, 1.0=unchanged). Required for contrast operation |
| `crop_left` | integer | No | Left crop boundary in pixels. Required for crop operation |
| `crop_top` | integer | No | Top crop boundary in pixels. Required for crop operation |
| `crop_right` | integer | No | Right crop boundary in pixels. Required for crop operation |
| `crop_bottom` | integer | No | Bottom crop boundary in pixels. Required for crop operation |

---

### 7. `insert_chart`

Insert a chart into a slide from spreadsheet data.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `presentation_path` | string | Yes | Path to the .pptx file |
| `slide_index` | integer | Yes | Slide index |
| `spreadsheet_path` | string | Yes | Path to source spreadsheet |
| `sheet_name` | string | Yes | Source sheet name |
| `data_range` | string | Yes | Data range (e.g., "A1:D10") |
| `chart_type` | string | No | Chart type: bar, line, pie, area, scatter, doughnut, radar. Default: "bar" |
| `title` | string | No | Chart title |
| `position` | string | No | Position on slide. Default: "body" |
| `include_header` | boolean | No | Include header row. Default: true |

---

### 8. `insert_table`

Insert a table into a slide.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .pptx file |
| `slide_index` | integer | Yes | Slide index |
| `rows` | array[array] | Yes | Table data as 2D array |
| `header` | boolean | No | First row is header. Default: true |
| `x` | number | No | X position in inches. Default: 0.5 |
| `y` | number | No | Y position in inches. Default: 1.5 |
| `width` | number | No | Table width in inches. Default: 9.0 |
| `height` | number | No | Table height in inches. Default: 5.0 |

---

### 9. `add_shape`

Add a shape to a slide with optional fill, line, and text styling.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .pptx file |
| `slide_index` | integer | Yes | Slide index |
| `shape_type` | string | Yes | Shape type (see below) |
| `x` | number | No | X position in inches. Default: 1.0 |
| `y` | number | No | Y position in inches. Default: 1.0 |
| `width` | number | No | Shape width in inches. Default: 2.0 |
| `height` | number | No | Shape height in inches. Default: 2.0 |
| `fill_color` | string | No | Fill color as hex (e.g., "FF0000") |
| `line_color` | string | No | Line color as hex (e.g., "000000") |
| `line_width` | number | No | Line width in points |
| `text` | string | No | Text to add inside the shape |
| `text_color` | string | No | Text color as hex (e.g., "000000") |
| `font_size` | number | No | Font size in points |

**Available shape types:**
- `rectangle`, `rounded_rectangle`, `oval`, `triangle`
- `right_arrow`, `left_arrow`, `up_arrow`, `down_arrow`
- `pentagon`, `hexagon`, `star`, `heart`
- `lightning_bolt`, `cloud`

---

### 10. `read_slides`

Read a character range from a Presentations presentation's text content.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .pptx file |
| `start` | integer | No | Start character index |
| `end` | integer | No | End character index |

---

### 11. `read_completedeck`

Read all slides from a presentation and return overview with indices.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .pptx file |

---

### 12. `read_individualslide`

Read detailed information about a single slide including components and images.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .pptx file |
| `slide_index` | integer | Yes | Slide index to read |

---

### 13. `read_image`

Retrieve a cached image extracted by read_slide using its annotation key.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the presentation file |
| `annotation` | string | Yes | Image annotation key |

---

## Consolidated Tools

When using consolidated mode, these meta-tools combine multiple operations:

### 1. `slides_schema`

Get JSON schemas for slides tool input/output models.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `schema_name` | string? | null | Name of specific schema to retrieve. If not provided, returns all schema names. |

---

### 2. `slides`

Unified interface for all Presentations presentation operations.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `action` | enum['create', 'delete', 'add_slide', 'edit', 'add_image', 'modify_image', 'insert_chart', 'insert_table', 'add_shape', 'read_range', 'read_deck', 'read_slide', 'read_image'] | Ellipsis | The action to perform |
| `file_path` | string? | null | Path to the .pptx file (required for most actions) |
| `directory` | string? | null | Directory path. REQUIRED for list/create operations. |
| `file_name` | string? | null | Filename with extension. REQUIRED for create/save. |
| `slides` | array[object[string, Any]]? | null | Slide definitions for create |
| `metadata` | object[string, Any]? | null | Presentation metadata (title, subject, author, comments) |
| `input_data` | object[string, Any]? | null | Input data for add_slide action |
| `operations` | array[object[string, Any]]? | null | Edit operations to apply |
| `image_path` | string? | null | Path to image file |
| `slide_index` | integer? | null | Slide index (0-based) |
| `x` | number? | null | X position in inches |
| `y` | number? | null | Y position in inches |
| `width` | number? | null | Width in pixels. Optional for export. |
| `height` | number? | null | Height in pixels. Optional for export. |
| `image_index` | integer? | null | Image index on slide (0-based) |
| `operation` | string? | null | Operation: rotate, flip, brightness, contrast, crop |
| `rotation` | integer? | null | Rotation angle (0-360) |
| `flip` | string? | null | Flip direction: horizontal, vertical |
| `brightness` | number? | null | Brightness factor (0.0-2.0). 1.0=unchanged. |
| `contrast` | number? | null | Contrast factor (0.0-2.0). 1.0=unchanged. |
| `crop_left` | integer? | null | Left crop boundary in pixels |
| `crop_top` | integer? | null | Top crop boundary in pixels |
| `crop_right` | integer? | null | Right crop boundary in pixels |
| `crop_bottom` | integer? | null | Bottom crop boundary in pixels |
| `spreadsheet_path` | string? | null | Path to source spreadsheet |
| `sheet_name` | string? | null | Sheet name in spreadsheet |
| `data_range` | string? | null | Cell range (e.g., 'A1:D5') |
| `chart_type` | string? | null | Chart type filter. Optional. |
| `title` | string? | null | Title for the entity. REQUIRED for create. |
| `position` | string? | null | Position: body, left, right |
| `include_header` | boolean? | null | Whether first row is header |
| `rows` | array[array[Any]]? | null | Table rows data |
| `header` | boolean? | null | Bold first row as header |
| `start` | integer? | null | Start character position |
| `end` | integer? | null | End character position |
| `annotation` | string? | null | Image annotation key from cache |
| `shape_type` | string? | null | Shape type for add_shape action |
| `fill_color` | string? | null | Fill color as hex (e.g., "FF0000") |
| `line_color` | string? | null | Line color as hex (e.g., "000000") |
| `line_width` | number? | null | Line width in points |
| `text` | string? | null | Text to add inside the shape |
| `text_color` | string? | null | Text color as hex (e.g., "000000") |
| `font_size` | number? | null | Font size in points |

---