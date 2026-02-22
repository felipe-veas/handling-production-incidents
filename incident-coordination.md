# Incident Coordination: Managing the Chaos

## The Reality of Multi-Responder Incidents

When a high-severity incident kicks off, engineers naturally swarm. If the outage is highly visible, your Slack channel or Zoom bridge quickly fills with 15 to 30 people: on-call engineers, service owners, DBAs, engineering managers, and anxious executives.

In theory, more eyes mean a faster fix. In reality, unstructured swarming is detrimental.

Without explicit coordination, a crowded bridge devolves into chaos. Three different engineers will run heavy queries against the same overloaded production database, degrading it further. Two teams will try contradictory mitigations at the same time—one rolling back a deployment while another pushes a hotfix. Meanwhile, executives demand status updates every two minutes, breaking the concentration of the people reading logs.

This is the "too many cooks" problem. It's why 20-minute fixes stretch into 2-hour outages.

## The Bystander Effect and Uncoordinated Action

Two dangerous behaviors emerge in uncoordinated incidents:

1. **The Bystander Effect**: With twenty people in a channel, everyone assumes someone else is running point. Critical tasks—checking database health, reviewing deployments, notifying support—get delayed because everyone thinks another engineer has it covered.
2. **The Cowboy Responder**: An engineer jumps in, forms an isolated hypothesis, and executes a destructive command (e.g., restarting a database primary) without telling anyone. This drastically alters system state while others are actively debugging.

Both stem from a lack of authoritative command.

## Establishing Incident Command

To resolve the chaos, establish an Incident Command System. The core is the Incident Commander (IC).

The IC is the single source of authority. They don't debug code. They don't query the database. The IC orchestrates the response, manages the team's cognitive load, and maintains forward momentum.

**How to Establish Command:**
Command requires an explicit, public declaration. When you join an unmanaged incident channel, your first action is to claim command or ask who holds it.

* *Correct*: "I am assuming the role of Incident Commander. Clear all production actions through me. Who is currently looking at the API logs?"
* *Incorrect*: (Silently logging into AWS and restarting instances).

If the IC needs to step away or handle a technical task, the handoff must be explicit.

* *Handoff*: "I am stepping down as IC to investigate the Redis cluster. Alice, do you accept the role of IC?" Alice must reply, "I accept."

If command isn't explicitly handed over, it hasn't transferred.

## Dividing Responsibilities

Once command is established, divide the team into specific, non-overlapping roles. Don't let engineers float ambiguously between tasks.

### 1. The Incident Commander (IC)

* **Role**: Decision-maker and orchestrator.
* **Actions**: Evaluates hypotheses, authorizes production changes, maintains the incident's mental model, and dictates pace.
* **Rule**: The IC does not execute technical commands. If the IC opens a terminal to debug, they've lost sight of the overall incident and must hand off command.

### 2. Operations / Execution (Subject Matter Experts)

* **Role**: Hands on the keyboard.
* **Actions**: Investigates logs, writes queries, and executes mitigations approved by the IC.
* **Rule**: Operations engineers state their intent, wait for IC approval, and report the result. (e.g., "IC, I want to bounce the auth pods to clear the connection pool." / IC: "Approved, proceed and report back.")

### 3. Communications Lead (Scribe / Comm)

* **Role**: The shield and translator.
* **Actions**: Handles external communication. Updates the status page, answers executive questions, and maintains the internal timeline.
* **Rule**: The Comm Lead intercepts questions from non-responders to protect the IC and Operations team's focus. If the CEO asks, "When will this be fixed?", the Comm Lead answers, not the engineer fixing the database.

By enforcing these roles, you turn a panicked mob into a disciplined response unit. You eliminate duplicate work, stop conflicting state changes, and create the space needed to think clearly under pressure.
