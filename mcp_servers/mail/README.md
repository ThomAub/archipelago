# Rls Mail MCP Server

A Python-based framework for rapidly developing Model Context Protocol (MCP) servers

## Tools (Default Mode)

These are the individual tools available by default:

### 1. `list_mails`

List emails with pagination support.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `limit` | integer | No | Maximum number of emails to return (1-100). Default: 50 |
| `offset` | integer | No | Number of emails to skip. Default: 0 |

---

### 2. `read_mail`

Read a sent email by its mail ID.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `mail_id` | string | Yes | The Message-ID of the email to read |

---

### 3. `search_mail`

Search emails by sender, recipient, subject, or body.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `from_email` | string | No | Filter by sender email |
| `to_email` | string | No | Filter by recipient email |
| `subject` | string | No | Filter by subject (partial match) |
| `body` | string | No | Filter by body content (partial match) |

---

### 4. `send_mail`

Send a new email.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `from_email` | string | Yes | Sender email address |
| `to_email` | string or array | Yes | Recipient email address(es) |
| `subject` | string | Yes | Email subject line |
| `body` | string | Yes | Email body content |
| `attachments` | array | No | List of file attachments |

---

### 5. `reply_mail`

Reply to an email.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `original_mail_id` | string | Yes | Message-ID of the email to reply to |
| `body` | string | Yes | Reply message body |
| `attachments` | array | No | List of file attachments |

---

### 6. `reply_all_mail`

Reply to all recipients of an email.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `original_mail_id` | string | Yes | Message-ID of the email to reply to |
| `body` | string | Yes | Reply message body |
| `attachments` | array | No | List of file attachments |

---

### 7. `forward_mail`

Forward an email to new recipients.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `original_mail_id` | string | Yes | Message-ID of the email to forward |
| `to_email` | string or array | Yes | Recipient email(s) |
| `body` | string | No | Additional message body |
| `attachments` | array | No | List of additional file attachments |

---

## Consolidated Tools

When using consolidated mode, these meta-tools combine multiple operations:

### 1. `mail`

Mail operations: send, read, list, search, reply, reply_all, and forward emails.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `action` | enum['help', 'send', 'read', 'list', 'search', 'reply', 'reply_all', 'forward'] | Ellipsis | Action to perform |
| `mail_id` | string? | null | Mail ID for read/reply/forward |
| `from_email` | string? | null | Sender email address |
| `to_email` | string | array[string]? | null | Recipient email address(es) |
| `subject` | string? | null | Email subject line |
| `body` | string? | null | Email or message body content. REQUIRED for send. |
| `cc` | string | array[string]? | null | CC recipients, comma-separated emails. |
| `bcc` | string | array[string]? | null | BCC recipients, comma-separated emails. |
| `attachments` | array[string]? | null | File paths to attach |
| `body_format` | enum['plain', 'html']? | null | Body format: 'plain' or 'html' |
| `thread_id` | string? | null | Thread identifier for grouping |
| `in_reply_to` | string? | null | Message-ID being replied to |
| `references` | array[string]? | null | List of referenced Message-IDs |
| `page` | integer? | null | Page number (0-indexed) |
| `limit` | integer? | null | Results per page |
| `offset` | integer? | null | Number of mails to skip |
| `search_from` | string? | null | Filter by sender email |
| `search_to` | string? | null | Filter by recipient email |
| `search_subject` | string? | null | Filter by subject (partial match) |
| `after_date` | string? | null | Filter emails after this date (YYYY-MM-DD or ISO format) |
| `before_date` | string? | null | Filter emails before this date (YYYY-MM-DD or ISO format) |
| `search_thread_id` | string? | null | Filter by thread ID |

---

### 2. `mail_schema`

Get JSON schema for mail input/output models.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `model` | string | Ellipsis | Model name: 'input', 'output', or a result type like 'SendResult', 'MailListResult' |

---