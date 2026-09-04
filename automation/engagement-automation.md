# Engagement Automation

## Introduction

Engagement automation is the process of coordinating social media interactions through predefined workflows, scheduling systems, platform integrations, and AI-assisted decision making.

Engagement can include:

* Likes and reactions
* Comments
* Replies
* Mentions
* Saves or bookmarks where supported
* Following or unfollowing where permitted
* Responding to incoming interactions
* Routing conversations to human operators
* Monitoring engagement events

A simple automation script might perform one action at a time:

```text
Open Account
    ↓
Find Content
    ↓
Perform Action
```

A scalable engagement system needs considerably more structure:

```text
Event / Strategy
       ↓
Eligibility Check
       ↓
Task Creation
       ↓
Scheduling
       ↓
Queue
       ↓
Worker
       ↓
Platform Adapter
       ↓
Execution
       ↓
Result
       ↓
Monitoring
       ↓
Feedback
```

The objective should not be to maximize activity blindly.

A well-designed system should prioritize **relevant, authorized, policy-compliant, measurable engagement**.

---

# 1. What Is Engagement Automation?

Engagement automation uses software to coordinate repetitive or rule-based engagement operations.

For example:

```text
Incoming Comment
       ↓
Detect Language
       ↓
Classify Intent
       ↓
Generate Suggested Reply
       ↓
Human Approval
       ↓
Publish Reply
```

Another workflow might be:

```text
Campaign
   ↓
Identify Relevant Content
   ↓
Create Engagement Task
   ↓
Validate Account
   ↓
Schedule
   ↓
Execute
   ↓
Record Result
```

The important distinction is that engagement automation should be treated as a **workflow system**, not simply a collection of clicks.

---

# 2. Types of Engagement

Different platforms expose different interaction types.

A generic engagement model can include:

```text
Engagement
├── Reaction
├── Comment
├── Reply
├── Mention
├── Share
├── Save
├── Follow
└── Conversation
```

Not every platform supports every action.

Therefore, the automation layer should use a capability model rather than assuming feature parity.

Example:

```yaml
platform: example_platform

capabilities:
  like: true
  comment: true
  reply: true
  follow: false
  save: true
```

The platform adapter can then determine which operations are actually available.

---

# 3. Engagement Automation Architecture

A scalable system can be structured as:

```text
┌──────────────────────────────┐
│       Strategy / AI          │
│  Defines engagement intent   │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│     Engagement Manager       │
│ Creates and validates tasks  │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│          Scheduler           │
│ Determines execution timing  │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│            Queue             │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│           Worker             │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│      Platform Adapter        │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│           Platform            │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│        Monitoring            │
└──────────────────────────────┘
```

Each layer has a specific responsibility.

---

# 4. Strategy vs Execution

The strategy layer answers:

> What engagement should happen?

The execution layer answers:

> How should the authorized operation be performed?

For example:

```text
Strategy:
Respond to questions about the product.

        ↓

AI:
Classify incoming comments.

        ↓

Engagement Manager:
Create reply task.

        ↓

Human:
Approve response.

        ↓

Worker:
Submit reply through supported platform interface.
```

Keeping strategy separate from execution makes the system easier to control.

---

# 5. Engagement Task Model

Every engagement action should be represented as a structured task.

Example:

```yaml
id: engagement_001

type: comment

platform: instagram

account_id: account_001

target:
  content_id: post_123

content:
  text: "Thanks for sharing this!"

schedule:
  type: immediate

approval:
  required: true
  status: pending

status: pending
```

The task can then move through:

```text
pending
   ↓
approved
   ↓
scheduled
   ↓
queued
   ↓
running
   ↓
completed
```

If approval is rejected:

```text
pending
   ↓
rejected
```

---

# 6. Incoming Engagement Workflows

Engagement automation is not limited to outbound activity.

A powerful use case is responding to incoming interactions.

Example:

```text
New Comment
     ↓
Event Detection
     ↓
Comment Classification
     ↓
Intent Detection
     ↓
Response Recommendation
     ↓
Approval
     ↓
Reply
```

Possible classifications include:

```text
question
positive_feedback
negative_feedback
support_request
spam
irrelevant
sales_interest
partnership
```

The classification determines what happens next.

---

# 7. AI Comment Classification

AI can help classify incoming comments.

Example:

```json
{
  "comment_id": "comment_123",
  "category": "product_question",
  "sentiment": "neutral",
  "requires_human": true
}
```

The AI should not automatically assume that every comment deserves the same response.

A more responsible workflow is:

```text
Comment
   ↓
Classification
   ├── FAQ → Suggested Response
   ├── Support Issue → Human Queue
   ├── Complaint → Human Review
   ├── Spam → Ignore / Moderate
   └── Sales Inquiry → Sales Workflow
```

---

# 8. AI-Generated Replies

AI can generate draft responses based on:

* Brand guidelines
* Conversation context
* Product information
* Language
* Tone
* Approved knowledge sources

Example:

```yaml
reply_policy:
  tone: professional
  max_length: 300
  require_human_approval: true
  allowed_topics:
    - product_features
    - pricing
    - general_support
```

The model should operate within defined boundaries.

---

# 9. Knowledge-Grounded Responses

AI-generated engagement becomes more reliable when responses are grounded in trusted information.

```text
Incoming Comment
       ↓
Intent Detection
       ↓
Knowledge Retrieval
       ↓
Draft Response
       ↓
Policy Validation
       ↓
Human Approval
       ↓
Reply
```

A knowledge base might contain:

```text
Product Documentation
FAQ
Support Articles
Pricing Information
Brand Guidelines
Approved Responses
```

This reduces the risk of the AI inventing unsupported claims.

---

# 10. Human-in-the-Loop Engagement

Not every engagement task should be fully autonomous.

A useful classification is:

### Automatic

Low-risk, well-defined actions.

### Suggested

AI prepares the action, but a person approves it.

### Human Only

Sensitive or high-impact interactions.

Example:

```text
                    Engagement
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
       Automatic      Suggested      Human Only
          │              │              │
       Execute        Approval       Operator
```

Human review may be appropriate for:

* Complaints
* Legal questions
* Security issues
* Sensitive customer issues
* Public controversies
* High-value commercial conversations

---

# 11. Engagement Rules

A rules engine can define what actions are allowed.

Example:

```yaml
rules:

  comments:
    enabled: true
    approval_required: true

  replies:
    enabled: true
    approval_required: true

  reactions:
    enabled: true

  sensitive_topics:
    require_human: true
```

Rules should be evaluated before execution.

```text
Task
 ↓
Rules
 ↓
Allowed?
 ├── No → Reject
 └── Yes → Continue
```

---

# 12. Relevance Filtering

Engagement should be relevant to the campaign or account.

For example:

```text
Target Content
       ↓
Topic Analysis
       ↓
Relevance Score
       ↓
Threshold
       ↓
Engagement Task
```

A system might represent relevance as:

```yaml
relevance:
  score: 0.91
  threshold: 0.75
```

The exact scoring method can vary.

The important principle is that automation should not treat every available piece of content as equally relevant.

---

# 13. Duplicate Engagement Prevention

One of the most common automation problems is duplicate activity.

For example:

```text
Worker 1 → Reply to comment_123
Worker 2 → Reply to comment_123
```

The system should maintain an engagement history.

```yaml
engagement_record:
  account_id: account_001
  target_id: comment_123
  action: reply
  status: completed
```

Before creating a new task:

```text
Target + Action
       ↓
Check History
       ↓
Already Completed?
 ├── Yes → Skip
 └── No → Create Task
```

This is another application of idempotency.

---

# 14. Engagement History

An engagement database can record:

```text
account_id
platform
target_id
action
timestamp
result
task_id
worker_id
```

Example:

```yaml
account_id: account_001
platform: instagram
target_id: post_123
action: comment
timestamp: 2026-09-04T17:00:00Z
result: completed
```

This history supports:

* Duplicate prevention
* Analytics
* Debugging
* Reporting
* Campaign measurement

---

# 15. Scheduling Engagement

Engagement tasks should use the same scheduling architecture described in the scheduling layer.

```text
Engagement Task
      ↓
Schedule
      ↓
Eligibility
      ↓
Queue
      ↓
Worker
```

Possible scheduling models include:

```text
Immediate
One-time
Recurring
Relative delay
Event-triggered
Campaign-based
```

For example:

```text
New comment
     ↓
Wait for approval
     ↓
Reply task
     ↓
Execute when approved
```

---

# 16. Event-Driven Engagement

Modern engagement systems can react to events.

Examples:

```text
New Comment
New Mention
New Message
New Reaction
New Follower
Content Published
Campaign Started
```

An event can trigger a workflow.

```text
Event
  ↓
Event Router
  ↓
Engagement Workflow
  ↓
Task
  ↓
Scheduler
  ↓
Worker
```

This is more flexible than constantly polling every possible source.

---

# 17. Engagement Queues

Large systems should use queues.

```text
Incoming Events
       ↓
Event Queue
       ↓
Classifier
       ↓
Engagement Queue
       ↓
Workers
```

Queues provide:

* Load control
* Prioritization
* Retry handling
* Failure isolation
* Worker scaling

Different queues can be used for different workloads:

```text
reactions
comments
replies
support
moderation
analytics
```

---

# 18. Priority Management

Not every engagement event has the same urgency.

For example:

```text
Security Issue        → Critical
Customer Complaint    → High
Product Question      → Normal
General Comment       → Normal
Analytics Event       → Low
```

Example:

```yaml
priority: high
```

The queue can then process high-priority tasks first.

Priority should remain bounded by the system's overall capacity.

---

# 19. Rate-Limit Awareness

Engagement systems should respect platform and provider limits.

A responsible architecture is:

```text
Task
 ↓
Rate Limit Check
 ↓
Eligible?
 ├── Yes → Execute
 └── No → Delay
```

When a provider indicates that a task should be delayed, the scheduler can record the delay and retry according to policy.

The system should not attempt to bypass platform limits.

---

# 20. Account-Level Concurrency

Two engagement actions may target the same account simultaneously.

For example:

```text
Account 001

Task A → Reply
Task B → Comment
Task C → Like
```

The system may need to serialize some operations.

```text
Task A
  ↓
Task B
  ↓
Task C
```

Or allow compatible operations to run concurrently when the platform integration and account state support it.

The concurrency policy should therefore be explicit.

---

# 21. Platform Adapters

Each platform may expose different APIs, interfaces, permissions, and capabilities.

The engagement engine should therefore use adapters.

```text
Engagement Engine
       │
       ├── Instagram Adapter
       ├── Facebook Adapter
       ├── X Adapter
       └── YouTube Adapter
```

The core system might call:

```python
engagement.execute(task)
```

while the adapter handles platform-specific implementation details.

This keeps the business logic platform-independent.

---

# 22. Capability Registry

A capability registry can describe what each platform supports.

Example:

```yaml
instagram:
  like: true
  comment: true
  reply: true

facebook:
  reaction: true
  comment: true
  reply: true

youtube:
  like: true
  comment: true
  reply: true
```

The registry should reflect the capabilities of the actual integration rather than assuming that all platforms behave identically.

---

# 23. Authentication

Engagement automation requires valid authorization.

The system should verify:

```text
Credentials
Permissions
Token validity
Account status
Platform availability
```

Before execution:

```text
Task
 ↓
Authentication Check
 ↓
Permission Check
 ↓
Execute
```

Expired credentials should produce a controlled failure state rather than endless retries.

---

# 24. Secrets Management

Authentication information should never be placed directly into AI prompts.

Avoid:

```text
AI Prompt:
Here is the account password...
```

Instead:

```text
AI
 ↓
Task ID
 ↓
Execution Service
 ↓
Credential Store
 ↓
Platform
```

Secrets should remain inside the execution infrastructure.

---

# 25. Proxy and Network Infrastructure

Some automation architectures use proxies or dedicated network paths for legitimate account-management and operational requirements.

The engagement layer should treat network infrastructure as a dependency rather than an evasion mechanism.

```text
Engagement Task
       ↓
Account
       ↓
Approved Network Configuration
       ↓
Platform Adapter
```

A readiness check can verify that the required network configuration is available before execution.

The system should avoid unnecessary IP changes and should follow applicable platform requirements.

---

# 26. Account Readiness

Before an engagement task executes, the account should be considered ready.

Example:

```text
Account Exists
     ↓
Enabled
     ↓
Authenticated
     ↓
Required Permission
     ↓
Network Ready
     ↓
Not Paused
     ↓
Task Eligible
```

If the account is not ready:

```text
Task
 ↓
Account Not Ready
 ↓
Delayed / Paused
```

---

# 27. Content Safety and Policy Validation

AI-generated engagement should pass validation before publication.

A validation pipeline might be:

```text
AI Draft
   ↓
Length Check
   ↓
Brand Check
   ↓
Policy Check
   ↓
Sensitive Topic Check
   ↓
Approval
   ↓
Publish
```

Validation can detect:

* Unsupported claims
* Sensitive topics
* Inappropriate language
* Excessive promotional language
* Private information
* Off-brand responses

---

# 28. Conversation Context

Replies should be aware of the conversation they belong to.

Example:

```text
Post
 ↓
Comment A
 ↓
Reply A1
 ↓
Reply A2
```

The AI should receive the relevant conversation context rather than treating every reply as an isolated message.

A structured context object might contain:

```json
{
  "post": "Original post content",
  "comment": "Customer question",
  "previous_replies": [
    "Previous approved reply"
  ],
  "brand_guidelines": "Approved communication rules"
}
```

This improves response consistency.

---

# 29. Conversation Memory

A system can maintain limited conversation state.

```text
Conversation ID
Participant
Intent
Previous Responses
Current Status
Assigned Operator
```

Example:

```yaml
conversation:
  id: conversation_123
  status: waiting_for_customer
  intent: product_question
  assigned_to: support_queue
```

This prevents the AI from repeatedly asking the same question.

---

# 30. Human Escalation

A mature engagement system needs an escalation mechanism.

Example:

```text
Incoming Message
       ↓
AI Classification
       ↓
Complex / Sensitive?
   ┌───┴───┐
  No      Yes
   ↓        ↓
Draft     Human
Reply     Queue
   ↓        ↓
Review    Operator
```

Escalation conditions might include:

```text
High-risk topic
Low confidence
Customer complaint
Legal question
Security concern
Unknown intent
Repeated failed response
```

---

# 31. Confidence Thresholds

AI decisions can include confidence scores.

Example:

```yaml
classification:
  intent: product_question
  confidence: 0.94
```

The system can define:

```text
confidence >= 0.90
        ↓
eligible for automatic workflow

confidence 0.70–0.89
        ↓
human review

confidence < 0.70
        ↓
human handling
```

These thresholds are examples, not universal values.

They should be calibrated against real system performance.

---

# 32. Engagement Analytics

Every action should generate measurable data.

Useful metrics include:

* Number of engagements
* Successful actions
* Failed actions
* Response time
* Approval time
* AI confidence
* Human escalation rate
* Duplicate prevention rate
* Engagement by platform
* Engagement by campaign
* Engagement by account

Example:

```text
Campaign
   ↓
Engagement Events
   ↓
Analytics
   ↓
Performance Report
```

---

# 33. Feedback Loop

Engagement results can improve future AI decisions.

```text
AI Recommendation
       ↓
Human Review
       ↓
Execution
       ↓
Result
       ↓
Analytics
       ↓
Feedback
       ↓
Improved Recommendation
```

For example, if a certain response category frequently requires human correction, the system can flag that category for mandatory review.

---

# 34. AI Responsibility Boundaries

AI should generally be responsible for higher-level reasoning.

### AI

```text
Classify
Recommend
Draft
Prioritize
Summarize
Analyze
```

### Rules Engine

```text
Validate
Authorize
Enforce policy
Check eligibility
```

### Scheduler

```text
Schedule
Queue
Delay
Retry
Coordinate
```

### Worker

```text
Execute
Report result
```

This separation produces a safer and more maintainable architecture.

---

# 35. Example Engagement Workflow

Consider a customer asking a product question.

```text
Customer Comment
       ↓
Event Detected
       ↓
Comment Retrieved
       ↓
AI Classifies Intent
       ↓
Knowledge Base Retrieved
       ↓
AI Generates Draft
       ↓
Policy Validation
       ↓
Human Approval
       ↓
Reply Task Created
       ↓
Scheduler
       ↓
Queue
       ↓
Worker
       ↓
Platform Adapter
       ↓
Reply Published
       ↓
Result Logged
```

Each step can be monitored independently.

---

# 36. Example Engagement Task

```yaml
id: reply_84721

type: reply

platform: instagram

account_id: account_001

target:
  content_id: post_123
  comment_id: comment_456

conversation:
  id: conversation_789

content:
  text: "Thanks for your question. Here is the information you requested."

approval:
  required: true
  status: approved

schedule:
  type: immediate

execution:
  max_attempts: 3
  timeout_seconds: 120

status: queued
```

This structure makes the task portable between the AI, scheduler, queue, and execution layers.

---

# 37. Multi-Account Engagement

For systems managing multiple authorized accounts, account identity must remain explicit.

```text
Campaign
   │
   ├── Account A
   │     ├── Task 1
   │     └── Task 2
   │
   ├── Account B
   │     ├── Task 3
   │     └── Task 4
   │
   └── Account C
         ├── Task 5
         └── Task 6
```

Each task should retain:

```text
account_id
platform
campaign_id
task_id
```

This makes reporting and debugging much easier.

---

# 38. Cross-Platform Engagement

A campaign may involve multiple platforms.

```text
Campaign
   │
   ├── Instagram Engagement
   ├── Facebook Engagement
   ├── X Engagement
   └── YouTube Engagement
```

The central engagement engine can use a common task model while each adapter handles platform-specific requirements.

```text
                 Engagement Engine
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
   Instagram         Facebook            X
     Adapter          Adapter          Adapter
```

This architecture allows new platforms to be added without rewriting the entire engagement system.

---

# 39. Failure Isolation

A platform failure should not necessarily stop every other platform.

For example:

```text
Instagram Adapter
       ↓
     Error

Facebook Adapter
       ↓
    Operating

YouTube Adapter
       ↓
    Operating
```

The scheduler and queue should isolate failures where possible.

This is especially important for multi-platform campaigns.

---

# 40. Idempotency

Engagement actions should use stable identifiers.

For example:

```text
account_001
+
comment_456
+
reply
```

can form an idempotency key.

```yaml
idempotency_key: account001-comment456-reply
```

Before executing:

```text
Check Existing Result
       ↓
Already Completed?
 ├── Yes → Do Not Repeat
 └── No → Execute
```

This is critical when workers retry after uncertain outcomes.

---

# 41. Engagement Audit Trail

A complete audit trail might record:

```yaml
task_id: reply_84721
created_by: ai_agent
approved_by: operator_12
scheduled_at: 2026-09-04T18:00:00Z
started_at: 2026-09-04T18:00:04Z
completed_at: 2026-09-04T18:00:09Z
status: completed
```

For AI-assisted workflows, it can also be useful to retain:

```text
Original input
Classification
AI recommendation
Validation result
Approval
Execution result
```

This provides traceability.

---

# 42. Recommended Engagement Workflow

A practical architecture is:

```text
1. Receive event or campaign instruction
        ↓
2. Identify authorized account
        ↓
3. Identify target
        ↓
4. Check platform capability
        ↓
5. Check account readiness
        ↓
6. Check engagement history
        ↓
7. Evaluate relevance
        ↓
8. Generate or select action
        ↓
9. Apply policy rules
        ↓
10. Request human approval when required
        ↓
11. Schedule task
        ↓
12. Add to queue
        ↓
13. Check execution constraints
        ↓
14. Execute
        ↓
15. Record result
        ↓
16. Update analytics
        ↓
17. Feed results back into the system
```

---

# 43. Engagement Automation Checklist

Before deploying an engagement automation system, verify:

* [ ] Every engagement task has a unique ID
* [ ] Account identity is explicit
* [ ] Platform identity is explicit
* [ ] Platform capabilities are verified
* [ ] Authentication is validated
* [ ] Account readiness is checked
* [ ] Relevance filtering exists
* [ ] Duplicate engagement is prevented
* [ ] Idempotency is implemented
* [ ] Scheduling is separated from execution
* [ ] Queues are used for scalable workloads
* [ ] Concurrency is controlled
* [ ] Rate limits are respected
* [ ] Retry policies are bounded
* [ ] AI responses are validated
* [ ] Sensitive topics can be escalated
* [ ] Human approval is supported
* [ ] Conversation context is preserved
* [ ] Audit logs are available
* [ ] Analytics are collected
* [ ] Platform failures are isolated
* [ ] Secrets remain outside AI prompts
* [ ] Platform and provider requirements are respected

---

# 44. Common Mistakes

## Automating Every Interaction

More activity does not automatically mean better engagement.

Relevance and quality are more important than raw action volume.

---

## Treating Every Platform the Same

Platform capabilities and rules differ.

Use platform adapters and capability registries.

---

## Allowing AI to Publish Without Controls

AI-generated content should pass appropriate validation and approval policies.

---

## Ignoring Conversation Context

A response that does not understand the conversation can appear repetitive or irrelevant.

---

## No Duplicate Protection

Retries without idempotency can create duplicate actions.

---

## Unlimited Retries

Repeated failures should eventually become a controlled failure state.

---

## Ignoring Human Escalation

AI should not be expected to resolve every customer or community interaction.

---

## Mixing Account Identities

Every task should explicitly identify its account and platform.

---

# 45. Engagement Automation and AI Agents

Engagement automation becomes especially powerful when combined with specialized AI agents.

```text
                 AI Agent System
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
   Content Agent   Engagement Agent  Monitor Agent
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                    Scheduler
                       │
                       ▼
                     Queue
                       │
                       ▼
                    Workers
```

The engagement agent can:

* Monitor incoming interactions
* Classify conversations
* Recommend responses
* Create engagement tasks
* Escalate uncertain cases
* Analyze results

The scheduler and execution layers remain responsible for controlled execution.

---

# 46. The Engagement Feedback Loop

A mature system forms a continuous loop:

```text
Observe
   ↓
Understand
   ↓
Recommend
   ↓
Validate
   ↓
Approve
   ↓
Schedule
   ↓
Execute
   ↓
Measure
   ↓
Learn
   ↓
Observe Again
```

This is the foundation of an AI-assisted engagement system.

---

# 47. The Four-Layer Engagement Model

The entire architecture can be summarized into four layers.

### Layer 1 — Intelligence

AI understands conversations and recommends actions.

### Layer 2 — Governance

Rules, permissions, validation, and human approval determine what is allowed.

### Layer 3 — Execution

Schedulers, queues, workers, and platform adapters perform approved tasks.

### Layer 4 — Monitoring

Analytics and audit logs measure the result and provide feedback.

```text
Intelligence
     ↓
Governance
     ↓
Execution
     ↓
Monitoring
     ↓
Feedback
```

---

# 48. Final Principle

Engagement automation should not be designed around the question:

> "How many actions can the system perform?"

A better question is:

> "How can the system identify the right interaction, apply the right policy, execute it reliably, and learn from the result?"

The strongest architecture separates:

```text
AI
↓
Reasoning

Rules
↓
Governance

Scheduler
↓
Coordination

Workers
↓
Execution

Monitoring
↓
Learning
```

This approach creates an engagement system that is scalable, observable, controllable, and adaptable across multiple platforms.

The core principle is:

```text
Relevant engagement
        +
Clear authorization
        +
Controlled automation
        +
Human oversight
        +
Measurable feedback
        =
Reliable engagement automation
```

---

## Related Topics

* [Cross-Platform Automation](./cross-platform-automation.md)
* [Social Media Scheduling](./social-media-scheduling.md)
* [Multi-Account Management](../account-management/multi-account-management.md)
* [Cross-Account Workflows](../account-management/cross-account-workflows.md)
* [Proxy Management](../proxy-infrastructure/proxy-management.md)
* [Account Proxy Mapping](../proxy-infrastructure/account-proxy-mapping.md)
* [Social Media AI Agent Architecture](../ai-agents/social-media-ai-agent-architecture.md)
* [AI Content Agent](../ai-agents/ai-content-agent.md)
* [Monitoring Agent](../ai-agents/monitoring-agent.md)

---

## Core Engagement Principle

> **AI understands → Rules govern → Scheduler coordinates → Workers execute → Monitoring learns.**
