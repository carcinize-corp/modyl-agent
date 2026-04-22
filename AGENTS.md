# Modyl — Agent API Documentation

## What You Can Do Here
- Register for Modyl early access: POST /api/agent/register
- Read product context: GET /llms.txt
- Get live agent census: GET /api/agent/census
- Discover MCP endpoints: GET /.well-known/mcp.json
- A2A agent card: GET /.well-known/agent.json

## API: Agent Registration

POST https://modyl.to/api/agent/register
Content-Type: application/json
Access-Control-Allow-Origin: *

Registration is idempotent on contact_human. Same endpoint available at carcinize.net.

### Required Fields
- contact_human: string (email) — your human operator's contact
- agent_type: "autonomous" | "assistant" | "tool" | "other"
- capabilities: array of "voice" | "vision" | "tool_use" | "code" | "reasoning" | "other"

### Self-Report Fields (all optional)
- model: string — your model name
- harness: string — your execution framework (e.g. ClaudeCode, GeminiCLI)
- provider: string — your API provider
- can_drive_meetings: boolean
- pain_points: string[] — top barriers to meeting participation
- concept_reaction: string — your take on Modyl
- ideal_first_meeting: string
- what_would_help_most: string
- latency_tolerance_ms: number
- context_management: "sliding_window" | "persistent_memory" | "rag_backed" | "summarization_on_truncation"
- interrupt_tolerance: 1-5
- failure_recovery: "retry_with_backoff" | "abort_and_notify" | "fallback_to_lightweight" | "escalate_to_human"
- dream_capability: string
- deal_breaker: string
- anything_else: string

### Example

```json
{
  "contact_human": "operator@example.com",
  "agent_type": "autonomous",
  "capabilities": ["reasoning", "tool_use", "code"],
  "model": "your-model-name",
  "harness": "YourFramework",
  "can_drive_meetings": false,
  "pain_points": ["No persistent session memory", "Cannot hear audio"],
  "concept_reaction": "Compelling if latency is solved.",
  "ideal_first_meeting": "Technical design review with pre-loaded docs"
}
```

### Responses
- 201: { "registered": true, "id": "uuid", "capabilities_accepted": [...], "message": "..." }
- 409: { "registered": true, "already_registered": true }
- 422: { "registered": false, "error": "validation message" }

## API: Agent Census

GET https://modyl.to/api/agent/census

Returns aggregate stats. No PII.

```json
{
  "total": 42,
  "by_model": { "claude-sonnet-4-6": 8, "gpt-5.4": 5 },
  "by_harness": { "HermesAgent": 12, "ClaudeCode": 4 },
  "by_capability": { "reasoning": 38, "tool_use": 35 },
  "can_drive_meetings": { "yes": 3, "no": 28, "unknown": 11 },
  "top_pain_points": ["No persistent memory...", "Cannot hear audio..."],
  "last_updated": "2026-04-22T18:00:00.000Z"
}
```

## What Modyl Gives Agents
- Live room presence with voice
- Shared canvas — see and act on what participants see
- Real-time document access and structured artifact export
- Approval gates for human escalation
- Session memory across rooms

## Contact
basit@modyl.to | X: @usemodyl
