# APEX-Agents Task Viewer — Implementation Plan

A SvelteKit 5 webapp for browsing, analyzing, and running inference on APEX-Agents benchmark tasks.

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | SvelteKit 5 (Svelte 5 runes) |
| Build | Vite |
| Runtime | Bun |
| Styling | Tailwind CSS 4 |
| AI Integration | Vercel AI SDK (`@ai-sdk/svelte`) |
| PDF Viewer | `pdfjs-dist` or `svelte-pdf` |
| Charts | Chart.js + `svelte-chartjs` or Recharts |
| Tokenizer | `tiktoken` (for token analysis) |

---

## Project Structure

```
task-viewer/
├── src/
│   ├── lib/
│   │   ├── components/
│   │   │   ├── tasks/
│   │   │   │   ├── TaskCard.svelte
│   │   │   │   ├── TaskList.svelte
│   │   │   │   ├── TaskDetail.svelte
│   │   │   │   ├── TaskFilters.svelte
│   │   │   │   └── RubricDisplay.svelte
│   │   │   ├── worlds/
│   │   │   │   ├── WorldCard.svelte
│   │   │   │   ├── WorldList.svelte
│   │   │   │   └── WorldDetail.svelte
│   │   │   ├── files/
│   │   │   │   ├── FilePreview.svelte
│   │   │   │   ├── PdfViewer.svelte
│   │   │   │   ├── SpreadsheetViewer.svelte
│   │   │   │   └── DocumentViewer.svelte
│   │   │   ├── inference/
│   │   │   │   ├── ModelSelector.svelte
│   │   │   │   ├── InferenceRunner.svelte
│   │   │   │   ├── RunHistory.svelte
│   │   │   │   └── ResponseComparison.svelte
│   │   │   ├── stats/
│   │   │   │   ├── DomainDistribution.svelte
│   │   │   │   ├── TokenAnalysis.svelte
│   │   │   │   └── StatsOverview.svelte
│   │   │   └── ui/
│   │   │       ├── Pagination.svelte
│   │   │       ├── SearchInput.svelte
│   │   │       ├── Badge.svelte
│   │   │       ├── Card.svelte
│   │   │       └── Modal.svelte
│   │   ├── server/
│   │   │   ├── data.ts          # Load & cache JSON data
│   │   │   ├── search.ts        # Search/filter logic
│   │   │   ├── files.ts         # File serving utilities
│   │   │   └── inference.ts     # AI SDK provider setup
│   │   ├── stores/
│   │   │   ├── tasks.svelte.ts  # Svelte 5 runes store
│   │   │   ├── filters.svelte.ts
│   │   │   └── inference.svelte.ts
│   │   ├── types/
│   │   │   ├── task.ts
│   │   │   ├── world.ts
│   │   │   ├── rubric.ts
│   │   │   └── inference.ts
│   │   └── utils/
│   │       ├── tokens.ts        # Token counting
│   │       ├── format.ts        # Formatting helpers
│   │       └── search.ts        # Client-side search
│   ├── routes/
│   │   ├── +layout.svelte
│   │   ├── +layout.server.ts    # Load all data at root
│   │   ├── +page.svelte         # Dashboard / Stats overview
│   │   ├── tasks/
│   │   │   ├── +page.svelte     # Task list with filters
│   │   │   ├── +page.server.ts
│   │   │   └── [task_id]/
│   │   │       ├── +page.svelte # Task detail view
│   │   │       ├── +page.server.ts
│   │   │       └── run/
│   │   │           └── +page.svelte  # Inference runner
│   │   ├── worlds/
│   │   │   ├── +page.svelte     # World list
│   │   │   ├── +page.server.ts
│   │   │   └── [world_id]/
│   │   │       ├── +page.svelte # World detail + its tasks
│   │   │       └── +page.server.ts
│   │   ├── stats/
│   │   │   ├── +page.svelte     # Statistics dashboard
│   │   │   └── +page.server.ts
│   │   ├── runs/
│   │   │   ├── +page.svelte     # All inference runs
│   │   │   └── [run_id]/
│   │   │       └── +page.svelte # Single run detail + grading
│   │   └── api/
│   │       ├── files/[...path]/
│   │       │   └── +server.ts   # Serve task files
│   │       ├── inference/
│   │       │   └── +server.ts   # AI SDK streaming endpoint
│   │       ├── grading/
│   │       │   └── +server.ts   # Trigger grading
│   │       └── runner/
│   │           └── +server.ts   # Trigger agent runner
│   ├── app.css                  # Tailwind imports
│   ├── app.html
│   └── hooks.server.ts          # Optional: future auth
├── static/
│   └── apex-agents/             # Symlink or copy of dataset
├── data/
│   └── runs.json                # Persisted inference runs
├── svelte.config.js
├── vite.config.ts
├── tailwind.config.ts
├── package.json
├── bunfig.toml
├── Dockerfile
├── docker-compose.yml
└── README.md
```

---

## Data Model

### Types (src/lib/types/)

```typescript
// task.ts
export interface Task {
  task_id: string;
  task_name: string;
  world_id: string;
  domain: 'Law' | 'Investment Banking' | 'Management Consulting';
  prompt: string;
  task_input_files: string;
  expected_output: string;
  gold_response: string;
  gold_response_type: string;
  rubric: RubricCriterion[];
  // Computed fields
  token_count?: number;
  files?: TaskFile[];
}

// rubric.ts
export interface RubricCriterion {
  verifier_id: string;
  criteria: string;
}

// world.ts
export interface World {
  world_id: string;
  world_name: string;
  world_description: string;
  domain: string;
  apps: WorldApp[];
  // Computed
  task_count?: number;
}

export interface WorldApp {
  service_id: string;
  service_name: string;
}

// inference.ts
export interface InferenceRun {
  id: string;
  task_id: string;
  model: string;
  provider: string;
  response: string;
  tokens_used: { input: number; output: number };
  duration_ms: number;
  timestamp: string;
  grading_result?: GradingResult;
}

export interface GradingResult {
  criteria_results: { criterion: string; met: boolean; explanation: string }[];
  score: number;
  total: number;
}
```

---

## Routes & Pages

### 1. Dashboard (`/`)
- **Purpose**: Overview stats and quick navigation
- **Data**: Aggregated counts, domain distribution chart, recent runs
- **Components**: `StatsOverview`, `DomainDistribution`, quick links

### 2. Tasks List (`/tasks`)
- **Purpose**: Browse all 480 tasks with filtering
- **Features**:
  - Pagination (20 per page)
  - Filter by domain (3 options)
  - Filter by world (33 options, grouped by domain)
  - Search by keyword in prompt/rubric
  - Sort by task_name, domain, world
- **URL params**: `?domain=Law&world=world_123&q=tariff&page=2`
- **Components**: `TaskFilters`, `TaskList`, `TaskCard`, `Pagination`

### 3. Task Detail (`/tasks/[task_id]`)
- **Purpose**: Full task view with all details
- **Sections**:
  - Task metadata (name, domain, world link)
  - Prompt (full text, token count)
  - Rubric criteria (expandable list)
  - Gold response (collapsible)
  - Associated files (preview thumbnails)
  - "Run Inference" button → `/tasks/[task_id]/run`
  - Previous runs for this task
- **Components**: `TaskDetail`, `RubricDisplay`, `FilePreview`, `RunHistory`

### 4. Task Inference Runner (`/tasks/[task_id]/run`)
- **Purpose**: Run inference with multiple models
- **Features**:
  - Model selector (dropdown with providers)
  - Streaming response display
  - Side-by-side comparison (run 2+ models)
  - Save run to history
  - Trigger grading after run
- **AI SDK Integration**: Uses `@ai-sdk/svelte` `useChat` or `useCompletion`
- **Components**: `ModelSelector`, `InferenceRunner`, `ResponseComparison`

### 5. Worlds List (`/worlds`)
- **Purpose**: Browse all 33 worlds
- **Features**:
  - Filter by domain
  - Show task count per world
  - Apps/services badges
- **Components**: `WorldList`, `WorldCard`

### 6. World Detail (`/worlds/[world_id]`)
- **Purpose**: World info + all tasks in that world
- **Sections**:
  - World description
  - Apps/services list
  - Task list (filtered to this world)
- **Components**: `WorldDetail`, `TaskList`

### 7. Statistics (`/stats`)
- **Purpose**: Deep analytics dashboard
- **Charts**:
  - Tasks by domain (pie/bar)
  - Tasks by world (bar, grouped by domain)
  - Rubric criteria count distribution (histogram)
  - Token analysis (prompt length distribution)
  - Expected output types breakdown
  - Gold response length distribution
- **Token Analysis**:
  - Average tokens per prompt by domain
  - Longest/shortest tasks
  - Token cost estimates per model
- **Components**: `DomainDistribution`, `TokenAnalysis`, `StatsOverview`

### 8. Runs History (`/runs`)
- **Purpose**: View all inference runs
- **Features**:
  - Filter by model, task, date
  - Show grading results if available
  - Compare runs across models
- **Components**: `RunHistory`, `GradingDisplay`

### 9. Run Detail (`/runs/[run_id]`)
- **Purpose**: Single run with grading
- **Sections**:
  - Task info (link)
  - Model/provider
  - Response (full)
  - Grading results (criterion by criterion)
  - Comparison with gold response

---

## API Endpoints

### `GET /api/files/[...path]`
- Serves files from `apex-agents/task_files/`
- Sets correct MIME type
- Used by file preview components

### `POST /api/inference`
- Body: `{ task_id, model, provider }`
- Streams response using AI SDK
- Returns: streamed text + final metadata

### `POST /api/grading`
- Body: `{ run_id }` or `{ task_id, response }`
- Triggers grading logic (compares against rubric)
- Can use LLM judge or link to external grader

### `POST /api/runner`
- Body: `{ task_id, config }`
- Links to / triggers the archipelago agent runner
- Returns: job ID or status

---

## AI SDK Integration

### Provider Configuration (`src/lib/server/inference.ts`)

```typescript
import { createOpenAI } from '@ai-sdk/openai';
import { createAnthropic } from '@ai-sdk/anthropic';
import { createGoogleGenerativeAI } from '@ai-sdk/google';

export const providers = {
  openai: createOpenAI({ apiKey: process.env.OPENAI_API_KEY }),
  anthropic: createAnthropic({ apiKey: process.env.ANTHROPIC_API_KEY }),
  google: createGoogleGenerativeAI({ apiKey: process.env.GOOGLE_API_KEY }),
};

export const models = [
  { id: 'gpt-4o', provider: 'openai', name: 'GPT-4o' },
  { id: 'gpt-4o-mini', provider: 'openai', name: 'GPT-4o Mini' },
  { id: 'claude-sonnet-4-20250514', provider: 'anthropic', name: 'Claude Sonnet 4' },
  { id: 'claude-opus-4-20250514', provider: 'anthropic', name: 'Claude Opus 4' },
  { id: 'gemini-2.0-flash', provider: 'google', name: 'Gemini 2.0 Flash' },
];
```

### Streaming Endpoint (`src/routes/api/inference/+server.ts`)

```typescript
import { streamText } from 'ai';
import { providers, models } from '$lib/server/inference';

export async function POST({ request }) {
  const { task_id, model_id, prompt } = await request.json();
  const model = models.find(m => m.id === model_id);
  const provider = providers[model.provider];

  const result = await streamText({
    model: provider(model_id),
    prompt,
  });

  return result.toDataStreamResponse();
}
```

### Client Component (`src/lib/components/inference/InferenceRunner.svelte`)

```svelte
<script lang="ts">
  import { useCompletion } from '@ai-sdk/svelte';

  let { task } = $props();

  const { completion, input, handleSubmit, isLoading } = useCompletion({
    api: '/api/inference',
    body: { task_id: task.task_id }
  });
</script>
```

---

## File Preview Strategy

### PDF Files
- Use `pdfjs-dist` with canvas rendering
- Lazy load pages (only render visible)
- Thumbnail generation for file list

### Spreadsheets (.xlsx, .csv)
- Use `sheetjs` (xlsx) to parse
- Render as HTML table with virtual scrolling
- Show first N rows by default

### Documents (.docx)
- Use `mammoth` to convert to HTML
- Render sanitized HTML

### Images
- Native `<img>` with lazy loading
- Lightbox for full view

### Component Structure

```svelte
<!-- FilePreview.svelte -->
<script lang="ts">
  let { file } = $props();

  const extension = file.name.split('.').pop()?.toLowerCase();
</script>

{#if extension === 'pdf'}
  <PdfViewer src={file.url} />
{:else if ['xlsx', 'csv'].includes(extension)}
  <SpreadsheetViewer src={file.url} />
{:else if ['docx', 'doc'].includes(extension)}
  <DocumentViewer src={file.url} />
{:else if ['png', 'jpg', 'jpeg', 'gif'].includes(extension)}
  <img src={file.url} alt={file.name} loading="lazy" />
{:else}
  <a href={file.url} download>Download {file.name}</a>
{/if}
```

---

## Token Analysis

### Implementation (`src/lib/utils/tokens.ts`)

```typescript
import { encoding_for_model } from 'tiktoken';

const enc = encoding_for_model('gpt-4o');

export function countTokens(text: string): number {
  return enc.encode(text).length;
}

export function analyzeTask(task: Task) {
  return {
    prompt_tokens: countTokens(task.prompt),
    gold_response_tokens: countTokens(task.gold_response),
    rubric_tokens: task.rubric.reduce(
      (sum, r) => sum + countTokens(r.criteria), 0
    ),
  };
}
```

### Stats to Compute
- Token distribution by domain
- Average prompt length
- Tasks with longest/shortest prompts
- Estimated cost per task (model pricing)

---

## Data Loading Strategy

### Root Layout (`src/routes/+layout.server.ts`)

```typescript
import { loadData } from '$lib/server/data';

export const load = async () => {
  const { tasks, worlds, metadata } = await loadData();
  return { tasks, worlds, metadata };
};
```

### Data Cache (`src/lib/server/data.ts`)

```typescript
import { readFile } from 'fs/promises';
import { join } from 'path';

let cache: { tasks: Task[]; worlds: World[]; metadata: any } | null = null;

export async function loadData() {
  if (cache) return cache;

  const dataDir = process.env.DATA_DIR || './static/apex-agents';

  const [tasksRaw, worldsRaw, metadataRaw] = await Promise.all([
    readFile(join(dataDir, 'tasks_and_rubrics.json'), 'utf-8'),
    readFile(join(dataDir, 'world_descriptions.json'), 'utf-8'),
    readFile(join(dataDir, 'metadata.json'), 'utf-8'),
  ]);

  cache = {
    tasks: JSON.parse(tasksRaw),
    worlds: JSON.parse(worldsRaw),
    metadata: JSON.parse(metadataRaw),
  };

  return cache;
}
```

---

## Docker Setup

### Dockerfile

```dockerfile
FROM oven/bun:1 AS builder
WORKDIR /app
COPY package.json bun.lockb ./
RUN bun install --frozen-lockfile
COPY . .
RUN bun run build

FROM oven/bun:1-slim
WORKDIR /app
COPY --from=builder /app/build ./build
COPY --from=builder /app/package.json .
COPY --from=builder /app/node_modules ./node_modules
COPY static/apex-agents ./static/apex-agents

ENV NODE_ENV=production
ENV PORT=3000
EXPOSE 3000
CMD ["bun", "run", "build/index.js"]
```

### docker-compose.yml

```yaml
version: '3.8'
services:
  task-viewer:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - ./static/apex-agents:/app/static/apex-agents:ro
      - ./data:/app/data
    environment:
      - DATA_DIR=/app/static/apex-agents
      - OPENAI_API_KEY=${OPENAI_API_KEY}
      - ANTHROPIC_API_KEY=${ANTHROPIC_API_KEY}
      - GOOGLE_API_KEY=${GOOGLE_API_KEY}
```

---

## Implementation Phases

### Phase 1: Foundation (MVP)
1. [ ] Initialize SvelteKit 5 project with Bun + Vite + Tailwind
2. [ ] Define TypeScript types for Task, World, Rubric
3. [ ] Implement data loading (`+layout.server.ts`)
4. [ ] Create basic UI components (Card, Badge, Pagination)
5. [ ] Build Tasks list page with pagination
6. [ ] Build Task detail page
7. [ ] Build Worlds list and detail pages

### Phase 2: Search & Filters
8. [ ] Add domain filter (dropdown/tabs)
9. [ ] Add world filter (searchable dropdown)
10. [ ] Implement keyword search (prompt + rubric)
11. [ ] URL-based filter state (query params)

### Phase 3: File Preview
12. [ ] Set up file serving API endpoint
13. [ ] Implement PDF viewer component
14. [ ] Implement spreadsheet viewer
15. [ ] Implement document viewer
16. [ ] File list with thumbnails on task detail

### Phase 4: Statistics & Token Analysis
17. [ ] Dashboard page with overview stats
18. [ ] Domain/world distribution charts
19. [ ] Token counting utility (tiktoken)
20. [ ] Token analysis visualizations
21. [ ] Task complexity metrics

### Phase 5: AI Inference Integration
22. [ ] Set up AI SDK providers
23. [ ] Create inference API endpoint (streaming)
24. [ ] Model selector component
25. [ ] Inference runner page
26. [ ] Response streaming display
27. [ ] Run history persistence (JSON file)
28. [ ] Multi-model comparison view

### Phase 6: Grading & Runner Integration
29. [ ] Grading display component
30. [ ] Grading API endpoint (LLM judge or link to grader)
31. [ ] Agent runner trigger endpoint
32. [ ] Run detail page with grading results

### Phase 7: Polish & Deploy
33. [ ] Responsive design refinement
34. [ ] Loading states and error handling
35. [ ] Dockerfile and docker-compose
36. [ ] Cloud Run deployment config
37. [ ] Environment variable documentation

---

## Environment Variables

```bash
# Data
DATA_DIR=./static/apex-agents

# AI Providers
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
GOOGLE_API_KEY=AI...

# Optional: Agent Runner
RUNNER_URL=http://localhost:8000

# Optional: Grader
GRADER_URL=http://localhost:8001
```

---

## Key Dependencies

```json
{
  "dependencies": {
    "@ai-sdk/anthropic": "^1.0.0",
    "@ai-sdk/google": "^1.0.0",
    "@ai-sdk/openai": "^1.0.0",
    "@ai-sdk/svelte": "^1.0.0",
    "ai": "^4.0.0",
    "pdfjs-dist": "^4.0.0",
    "xlsx": "^0.18.0",
    "mammoth": "^1.6.0",
    "tiktoken": "^1.0.0",
    "chart.js": "^4.0.0",
    "svelte-chartjs": "^3.0.0"
  },
  "devDependencies": {
    "@sveltejs/kit": "^2.0.0",
    "@sveltejs/adapter-node": "^5.0.0",
    "svelte": "^5.0.0",
    "tailwindcss": "^4.0.0",
    "vite": "^6.0.0",
    "typescript": "^5.0.0"
  }
}
```

---

## UI/UX Notes

### Navigation
- Sidebar: Tasks, Worlds, Stats, Runs
- Breadcrumbs on detail pages
- Quick filters always visible on list pages

### Task Card Design
- Domain badge (color-coded: Law=blue, IB=green, Consulting=purple)
- World name (link)
- Prompt preview (truncated)
- Rubric count
- File count indicator

### Color Scheme (Tailwind)
- Law: `blue-500`
- Investment Banking: `emerald-500`
- Management Consulting: `violet-500`
- Background: `slate-50` / `slate-900` (dark mode)

### Responsive
- Mobile: Stack filters, single column cards
- Desktop: Sidebar + main content, 2-3 column grid

---

## Future Enhancements (Out of Scope for MVP)

- [ ] Full-text search with fuzzy matching
- [ ] Batch inference (run all tasks with a model)
- [ ] Export results to CSV
- [ ] Compare multiple models on same task set
- [ ] Integration with BigQuery for results storage
- [ ] User authentication (when deployed with IAP)
- [ ] Task difficulty scoring based on rubric/token analysis
