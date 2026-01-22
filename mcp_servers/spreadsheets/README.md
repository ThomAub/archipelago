# Rls Sheets MCP Server

A Python-based framework for rapidly developing Model Context Protocol (MCP) servers

## Tools (Default Mode)

These are the individual tools available by default:

### 1. `create_spreadsheet`

Create a new spreadsheet with the specified sheets.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `directory` | string | Yes | Directory path (use "/" for root) |
| `file_name` | string | Yes | Output filename ending with .xlsx |
| `sheets` | array[object] | Yes | List of sheet definitions with name and data |

---

### 2. `delete_spreadsheet`

Delete the specified spreadsheet.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .xlsx file to delete |

---

### 3. `read_tab`

Read a specific worksheet tab from a spreadsheet, optionally filtering by cell range.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .xlsx file |
| `tab_index` | integer | Yes | 0-based worksheet tab index |
| `cell_range` | string | No | Cell range like "A1" or "A1:C5" |

---

### 4. `read_csv`

Read and parse a CSV file.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .csv file |
| `delimiter` | string | No | Column delimiter character. Default: "," |
| `encoding` | string | No | File encoding. Default: "utf-8" |
| `has_header` | boolean | No | Whether first row is header. Default: true |
| `row_limit` | integer | No | Maximum rows to read |

---

### 5. `list_tabs_in_spreadsheet`

List worksheet names and indices for a spreadsheet.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .xlsx file |

---

### 6. `add_tab`

Add a new worksheet tab to an existing spreadsheet with optional data.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .xlsx file |
| `tab_name` | string | Yes | Name for the new worksheet tab (max 31 chars) |
| `sheet_data` | object | No | Optional data with headers and rows |

---

### 7. `delete_tab`

Delete a worksheet tab from a spreadsheet.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .xlsx file |
| `tab_index` | integer | Yes | 0-based worksheet tab index to delete |

---

### 8. `edit_spreadsheet`

Apply update operations to an existing spreadsheet.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .xlsx file |
| `operations` | array[object] | Yes | List of edit operations to apply |

#### Available Operation Types

##### `set_cell`
Set a specific cell value.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "set_cell" |
| `sheet` | string | Yes | Sheet name |
| `cell` | string | Yes | Cell reference (e.g., "A1") |
| `value` | any | Yes | Value to set |

##### `append_rows`
Append rows to a sheet.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "append_rows" |
| `sheet` | string | Yes | Sheet name |
| `rows` | array[array] | Yes | Rows to append |

##### `rename_sheet`
Rename a sheet.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "rename_sheet" |
| `sheet` | string | Yes | Current sheet name |
| `new_name` | string | Yes | New sheet name |

##### `format_cells`
Format cells (font, colors, alignment, borders).

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "format_cells" |
| `sheet` | string | Yes | Sheet name |
| `range` | string | Yes | Cell range (e.g., "A1:B5", "A:A", "1:5") |
| `font_name` | string | No | Font name |
| `font_size` | integer | No | Font size |
| `font_bold` | boolean | No | Bold text |
| `font_italic` | boolean | No | Italic text |
| `font_underline` | boolean | No | Underline text |
| `font_color` | string | No | Font color (hex, e.g., "FF0000") |
| `fill_color` | string | No | Background color (hex) |
| `fill_pattern` | string | No | Fill pattern (e.g., "solid", "lightGray") |
| `horizontal_alignment` | string | No | Horizontal alignment (left, center, right, justify) |
| `vertical_alignment` | string | No | Vertical alignment (top, center, bottom) |
| `wrap_text` | boolean | No | Enable text wrapping |
| `border_style` | string | No | Border style (thin, medium, thick, etc.) |
| `border_color` | string | No | Border color (hex) |
| `border_sides` | array[string] | No | Border sides (left, right, top, bottom) |

##### `merge_cells`
Merge cells into one.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "merge_cells" |
| `sheet` | string | Yes | Sheet name |
| `range` | string | Yes | Cell range to merge (e.g., "A1:D1") |

##### `unmerge_cells`
Unmerge previously merged cells.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "unmerge_cells" |
| `sheet` | string | Yes | Sheet name |
| `range` | string | Yes | Cell range to unmerge (e.g., "A1:D1") |

##### `set_column_width`
Set the width of a column.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "set_column_width" |
| `sheet` | string | Yes | Sheet name |
| `column` | string | Yes | Column letter (e.g., "A", "AA") |
| `width` | number | Yes | Width value (1-255) |

##### `set_row_height`
Set the height of a row.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "set_row_height" |
| `sheet` | string | Yes | Sheet name |
| `row` | integer | Yes | Row number (1-based) |
| `height` | number | Yes | Height value (1-409) |

##### `freeze_panes`
Freeze rows and/or columns at a specific cell.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "freeze_panes" |
| `sheet` | string | Yes | Sheet name |
| `cell` | string | No | Cell reference (e.g., "B2" freezes row 1 and column A). Null to unfreeze. |

##### `add_named_range`
Create a named range.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "add_named_range" |
| `name` | string | Yes | Name for the range (starts with letter/underscore) |
| `sheet` | string | Yes | Sheet name |
| `range` | string | Yes | Cell range (e.g., "A1:B10") |

##### `delete_named_range`
Delete a named range.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "delete_named_range" |
| `name` | string | Yes | Name of the range to delete |

##### `add_data_validation`
Add data validation rules to cells.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "add_data_validation" |
| `sheet` | string | Yes | Sheet name |
| `range` | string | Yes | Cell range (e.g., "A1:A10") |
| `validation_type` | string | Yes | Type: list, whole, decimal, date, time, textLength, custom |
| `operator` | string | No | Operator: between, notBetween, equal, notEqual, lessThan, lessThanOrEqual, greaterThan, greaterThanOrEqual |
| `formula1` | string | No | First formula/value (for list: comma-separated values or range) |
| `formula2` | string | No | Second formula/value (for between/notBetween) |
| `allow_blank` | boolean | No | Allow blank cells (default: true) |
| `show_error_message` | boolean | No | Show error on invalid input (default: true) |
| `error_title` | string | No | Error dialog title |
| `error_message` | string | No | Error dialog message |
| `show_input_message` | boolean | No | Show input message (default: false) |
| `input_title` | string | No | Input message title |
| `input_message` | string | No | Input message text |

##### `add_conditional_formatting`
Add conditional formatting rules to cells.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "add_conditional_formatting" |
| `sheet` | string | Yes | Sheet name |
| `range` | string | Yes | Cell range (e.g., "A1:A10") |
| `rule_type` | string | Yes | Rule type: cellIs, colorScale, dataBar, expression, top10, aboveAverage, duplicateValues, uniqueValues, containsText, notContainsText, beginsWith, endsWith, containsBlanks, notContainsBlanks |
| `operator` | string | No | Operator for cellIs rule |
| `formula` | string | No | Formula or value to compare |
| `formula2` | string | No | Second formula for "between" operator |
| `font_color` | string | No | Font color (hex) |
| `fill_color` | string | No | Fill color (hex) |
| `font_bold` | boolean | No | Bold text |
| `font_italic` | boolean | No | Italic text |
| `color_scale_colors` | array[string] | No | Colors for colorScale (2-3 hex colors) |
| `data_bar_color` | string | No | Color for dataBar |
| `rank` | integer | No | Rank for top10 rule |
| `percent` | boolean | No | Use percentage for top10 |
| `text` | string | No | Text for containsText, beginsWith, endsWith rules |

##### `set_auto_filter`
Enable or disable auto-filter on a range.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "set_auto_filter" |
| `sheet` | string | Yes | Sheet name |
| `range` | string | No | Cell range (e.g., "A1:D10"). Null to remove filter. |

##### `set_number_format`
Set number format for cells.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "set_number_format" |
| `sheet` | string | Yes | Sheet name |
| `range` | string | Yes | Cell range (e.g., "A1:A10") |
| `format` | string | Yes | Format string (e.g., "#,##0.00", "0%", "yyyy-mm-dd") |

##### `add_image`
Add an image to a worksheet.

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | Yes | Must be "add_image" |
| `sheet` | string | Yes | Sheet name |
| `image_path` | string | Yes | Path to the image file |
| `cell` | string | Yes | Anchor cell position (e.g., "A1") |
| `width` | integer | No | Width in pixels |
| `height` | integer | No | Height in pixels |

---

### 9. `add_content_text`

Add content to a specific cell in a worksheet tab, only if the cell is empty.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .xlsx file |
| `tab_index` | integer | Yes | 0-based worksheet tab index |
| `cell` | string | Yes | Cell reference (e.g., "A1", "B5") |
| `value` | any | Yes | Value to add to the cell |

---

### 10. `delete_content_cell`

Delete content from a specific cell in a worksheet tab.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Path to the .xlsx file |
| `tab_index` | integer | Yes | 0-based worksheet tab index |
| `cell` | string | Yes | Cell reference (e.g., "A1", "B5") |

---

### 11. `create_chart`

Create a chart from data in a spreadsheet.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `file_path` | str[str] | _required_ | - |
| `sheet` | str[str] | _required_ | - |
| `data_range` | str[str] | _required_ | - |
| `chart_type` | Literal[Literal[bar, line, pie]] | 'bar' | - |
| `title` | str | None[str | None] | null | - |
| `position` | str[str] | 'E2' | - |
| `categories_column` | int | None[int | None] | null | - |
| `include_header` | bool[bool] | True | - |

---

## Consolidated Tools

When using consolidated mode, these meta-tools combine multiple operations:

### 1. `sheets`

Spreadsheet operations: create, read, edit, and manage .xlsx files.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `action` | enum['help', 'create', 'delete', 'read_tab', 'read_csv', 'list_tabs', 'add_tab', 'delete_tab', 'edit', 'add_content', 'delete_content', 'create_chart'] | Ellipsis | Action to perform |
| `file_path` | string? | null | Full file path (e.g., '/report.xlsx'). REQUIRED for all actions except 'create'. |
| `directory` | string? | null | Directory path. REQUIRED for 'create' action (e.g., '/'). Use with file_name. |
| `file_name` | string? | null | File name with .xlsx extension. REQUIRED for 'create' action (e.g., 'report.xlsx'). |
| `tab_index` | integer? | null | 0-based tab index. REQUIRED for read_tab, delete_tab, add_content, delete_content. Use 0 for firs... |
| `tab_name` | string? | null | Tab name for 'add_tab' action only. NOT used for read_tab (use tab_index instead). |
| `cell_range` | string? | null | Cell range for 'read_tab' (e.g., 'A1:C5') |
| `sheets` | array[object[string, Any]]? | null | Sheet definitions for 'create'. REQUIRED for create. Format: [{name: 'Sheet1', headers: ['A','B']... |
| `sheet_data` | object[string, Any]? | null | Data for 'add_tab': {headers?, rows} |
| `operations` | array[object[string, Any]]? | null | Operations for 'edit' action. Each operation needs 'type' field. |
| `cell` | string? | null | Cell reference for add_content/delete_content (e.g., 'A1') |
| `value` | Any? | null | Value to set or match. |
| `sheet` | string? | null | Target sheet name. |
| `data_range` | string? | null | Data range for chart (e.g., 'A1:C10') |
| `chart_type` | enum['bar', 'line', 'pie']? | null | Chart type |
| `title` | string? | null | Title for the entity. REQUIRED for create. |
| `position` | string? | null | Chart position (e.g., 'E2') |
| `categories_column` | integer? | null | Column index for X-axis categories |
| `include_header` | boolean? | null | Whether first row is header |
| `delimiter` | string? | null | CSV delimiter |
| `encoding` | string? | null | CSV encoding |
| `has_header` | boolean? | null | CSV has header row |
| `row_limit` | integer? | null | Max rows to read from CSV |

---

### 2. `sheets_schema`

Get JSON schema for sheets input/output models.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `model` | string | Ellipsis | Model name: 'input', 'output', or a result type like 'ReadTabResult' |

---