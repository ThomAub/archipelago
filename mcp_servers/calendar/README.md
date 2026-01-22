# Rls Calendar MCP Server

A Python-based framework for rapidly developing Model Context Protocol (MCP) servers

## Tools (Default Mode)

These are the individual tools available by default:

### 1. `list_events`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `limit` | int | DEFAULT_LIST_LIMIT | Maximum number of events to return |

---

### 2. `read_event`

Read a calendar event by its event ID.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `event_id` | string | Yes | Event ID to read |

---

### 3. `create_event`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `summary` | str | _required_ | Event summary/title |
| `description` | str? | null | Event description |
| `start` | CalendarEventDateTime | _required_ | Event start time |
| `end` | CalendarEventDateTime | _required_ | Event end time |
| `location` | str? | null | Event location |
| `attendees` | list[CalendarEventAttendee]? | null | Event attendees |
| `colorId` | str? | null | Event color ID |
| `reminders` | CalendarEventReminders? | null | Event reminders |

---

### 4. `update_event`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `event_id` | str | _required_ | Event ID to update |
| `summary` | str? | null | Event summary/title |
| `description` | str? | null | Event description |
| `start` | CalendarEventDateTime? | null | Event start time |
| `end` | CalendarEventDateTime? | null | Event end time |
| `location` | str? | null | Event location |
| `attendees` | list[CalendarEventAttendee]? | null | Event attendees |
| `colorId` | str? | null | Event color ID |
| `reminders` | CalendarEventReminders? | null | Event reminders |

---

### 5. `delete_event`

Delete a calendar event by its event ID.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `event_id` | string | Yes | Event ID to delete |

---

## Consolidated Tools

When using consolidated mode, these meta-tools combine multiple operations:

### 1. `calendar`

Calendar operations: create, read, update, delete, and list events.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `action` | enum['help', 'create', 'read', 'update', 'delete', 'list'] | Ellipsis | Action to perform. REQUIRED. Use help to see available actions. |
| `event_id` | string? | null | Event ID. REQUIRED for read/update/delete actions. |
| `summary` | string? | null | Event title/summary. REQUIRED for create action. |
| `description` | string? | null | Event description |
| `location` | string? | null | Event location |
| `start_date` | string? | null | Start date for all-day events (YYYY-MM-DD) |
| `start_datetime` | string? | null | Start datetime (ISO format with timezone) |
| `end_date` | string? | null | End date for all-day events (YYYY-MM-DD) |
| `end_datetime` | string? | null | End datetime (ISO format with timezone) |
| `timezone` | string? | null | Timezone (e.g., 'America/New_York') |
| `attendees` | array[string]? | null | List of attendee emails |
| `page` | integer? | null | Page number (0-indexed). Use with limit for pagination. |
| `limit` | integer? | null | Results per page. Use with page for pagination. |

---

### 2. `calendar_schema`

Get JSON schema for calendar input/output models.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `model` | string | Ellipsis | Model name: 'input', 'output', or a result type |

---
