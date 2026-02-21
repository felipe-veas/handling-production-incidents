# Incident Coordination: Managing the Chaos

## The Reality of Multi-Responder Incidents

When a high-severity incident kicks off, the natural tendency is for engineers to swarm. If the outage is highly visible, the incident Slack channel or Zoom bridge will quickly fill with 15 to 30 people: on-call engineers, service owners, database administrators, engineering managers, and anxious executives.

In theory, more eyes on the problem should lead to a faster resolution. In operational reality, unstructured swarming is actively detrimental.

Without explicit coordination, a crowded incident bridge devolves into chaos. You will see three different engineers running conflicting queries against the same overloaded production database, further degrading its performance. You will have two different teams attempting to apply contradictory mitigation strategies simultaneously—one team rolling back the deployment while another attempts to push a hotfix. Meanwhile, the executive team is asking for status updates every two minutes, breaking the concentration of the engineers who are trying to read logs.

This is the "too many cooks" problem, and it is the primary reason why incidents that should be resolved in 20 minutes stretch into 2-hour outages.

## The Bystander Effect and Uncoordinated Action

Two opposing, dangerous behaviors emerge in uncoordinated incidents:

1. **The Bystander Effect**: When twenty people are in a channel, everyone assumes someone else is running point. Critical actions—like checking the database health, reviewing recent deployments, or notifying customer support—are delayed because everyone thinks another engineer is handling it.
2. **The Cowboy Responder**: Conversely, an engineer might jump into the fray, formulate a hypothesis in isolation, and execute a destructive command (e.g., restarting a master database node) without telling anyone, drastically altering the state of the system while others are actively trying to debug it.

Both behaviors stem from the absence of a singular, authoritative command structure.

## Establishing Incident Command

To resolve chaos, you must establish an Incident Command System (ICS). The core of this system is the Incident Commander (IC).

The Incident Commander is the single source of truth and authority during the incident. They are not the person debugging the code. They are not the person querying the database. The IC's job is to orchestrate the response, manage the cognitive load of the team, and maintain forward momentum.

**How to Establish Command:**
Command is established through explicit, public declaration. When you join a chaotic, unmanaged incident channel, the very first action is to claim command or ask who is in command.

* *Correct*: "I am assuming the role of Incident Commander. All actions on production systems must be cleared through me. Who is currently looking at the API logs?"
* *Incorrect*: (Silently logging into AWS and starting to restart instances).

If the current IC needs to step away or handle a specific technical task, the handoff must be explicit.

* "I am stepping down as IC to investigate the Redis cluster. Alice, do you accept the role of IC?" Alice must reply, "I accept."

If command is not explicitly handed over, it has not been transferred.

## Dividing Responsibilities Without Chaos

Once the Commander is established, the incident team must be divided into specific, non-overlapping roles. Do not let engineers float ambiguously between tasks.

### 1. The Incident Commander (IC)

* **Role**: The decision-maker and orchestrator.
* **Actions**: Evaluates hypotheses, authorizes changes to production, maintains the overall mental model of the incident, and dictates the pace of the response.
* **Rule**: The IC does not execute technical commands. If the IC opens a terminal to start debugging, they have lost sight of the overall incident and must hand off command immediately.

### 2. Operations / Execution (Subject Matter Experts)

* **Role**: The hands on the keyboard.
* **Actions**: These are the engineers investigating logs, writing queries, and executing the mitigation steps approved by the IC.
* **Rule**: Operations engineers must state what they are going to do, wait for the IC's approval, and then report the results. (e.g., "IC, I want to bounce the auth pods to clear the connection pool." / IC: "Approved, proceed and report back.")

### 3. Communications Lead (Scribe / Comm)

* **Role**: The shield and the translator.
* **Actions**: Handles all communication outside the incident bridge. They update the status page, answer questions from executives, and maintain the internal timeline of events.
* **Rule**: The Comm Lead intercepts all incoming questions from non-responders so the IC and Operations team are not distracted. If the CEO asks "When will this be fixed?", the Comm Lead answers, not the engineer fixing the database.

By strictly enforcing these roles, you convert a mob of panicked engineers into a disciplined response unit. You eliminate duplicate work, prevent conflicting state changes, and create the space necessary to think clearly under pressure.
