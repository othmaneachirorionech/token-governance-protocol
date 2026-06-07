# Token Governance Protocol

**Token Governance Protocol (TGP)** is a draft protocol for declaring, tracking, and enforcing token and cost budgets in AI agent systems.

Modern AI agents can continue consuming tokens, calling models, and generating cost without a clear execution boundary.

TGP defines a minimal control layer:

1. Declare the budget before execution.
2. Track consumption during execution.
3. Emit events when thresholds are reached.
4. Pause or stop execution when limits are exceeded.
5. Produce an auditable budget record.

## Core Rule

```text
No AI agent execution should continue without a declared and monitored token budget.

Why This Exists

AI agent systems can fail financially before they fail technically.

A workflow may keep running while:

token usage grows silently
model calls multiply
retry loops consume budget
tool calls trigger more model calls
agents continue beyond the intended scope
cost becomes visible only after execution

TGP treats token and cost consumption as an execution control surface, not as an invisible side effect.

What TGP Controls
Token budget
Estimated cost
Model usage
Execution limits
Warning thresholds
Kill-switch thresholds
Budget exhaustion events
Pause / stop decisions
Basic Budget Declaration

{
  "budget_id": "budget-demo-001",
  "workflow_id": "lead-review-agent",
  "model": "example-model",
  "max_input_tokens": 50000,
  "max_output_tokens": 10000,
  "max_estimated_cost_usd": 5.0,
  "warning_threshold_percent": 80,
  "kill_threshold_percent": 100,
  "action_on_warning": "emit_warning",
  "action_on_exceed": "pause_for_review"
}

{
  "event_type": "token_budget_warning",
  "budget_id": "budget-demo-001",
  "workflow_id": "lead-review-agent",
  "input_tokens_used": 40000,
  "output_tokens_used": 7000,
  "estimated_cost_usd": 4.1,
  "threshold_percent": 80,
  "timestamp": "2026-06-07T10:00:00Z"
}
What This Is Not

TGP is not:

a billing system
a pricing registry
a full observability platform
a compliance guarantee
a model router
a security sandbox
a replacement for human approval

This repository currently defines the protocol specification only.

A reference implementation may be added later.

Relationship to assumption-gate

assumption-gate pauses or stops execution when material context changes invalidate original assumptions.

TGP pauses or stops execution when token or cost budgets exceed declared limits.

Together, they represent small governance primitives for controlled AI agent execution.

Status

Draft v0.1.0.

Experimental protocol specification.

License

MIT License.
