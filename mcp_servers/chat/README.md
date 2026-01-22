# Rls Chat MCP Server

A Python-based framework for rapidly developing Model Context Protocol (MCP) servers

## Tools (Default Mode)

These are the individual tools available by default:

### 1. `list_channels`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `limit` | int | 100 | Maximum number of channels to return |

---

### 2. `get_channel_history`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `channel_id` | str | _required_ | Channel ID |
| `limit` | int | 30 | Maximum number of messages to return |

---

### 3. `get_thread_replies`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `channel_id` | str | _required_ | Channel ID |

---

### 4. `get_user_profile`

Get detailed profile information for a specific user.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `user_id` | string | Yes | User ID |

---

### 5. `get_users`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `limit` | int | 100 | Maximum number of users to return |
| `page` | int | 0 | Page number (0-indexed) |

---

### 6. `post_message`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `channel_id` | str | _required_ | Channel ID |

---

### 7. `reply_to_thread`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `channel_id` | str | _required_ | Channel ID |
| `post_id` | str | _required_ | Post ID to reply to |

---

### 8. `add_reaction`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `channel_id` | str | _required_ | Channel ID |
| `post_id` | str | _required_ | Post ID to react to |

---

### 9. `delete_post`

No description available.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `channel_id` | str | _required_ | Channel ID |

---

## Consolidated Tools

When using consolidated mode, these meta-tools combine multiple operations:

### 1. `chat`

Chat operations: channels, messages, reactions, and users.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `action` | enum['help', 'list_channels', 'get_history', 'post', 'reply', 'react', 'get_replies', 'list_users', 'get_profile', 'delete'] | Ellipsis | Action to perform. REQUIRED. Use help to see available actions. |
| `channel_id` | string? | null | Channel/group ID |
| `post_id` | string? | null | Message/post ID |
| `message` | string? | null | Message content for post/reply |
| `emoji` | string? | null | Emoji for reaction (e.g., '👍') |
| `user_id` | string? | null | User ID for get_profile |
| `page` | integer? | null | Page number (0-indexed) |
| `limit` | integer? | null | Results per page. Use with page for pagination. |

---

### 2. `chat_schema`

Get JSON schema for chat input/output models.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `model` | string | Ellipsis | Model name: 'input', 'output', or a result type |

---
