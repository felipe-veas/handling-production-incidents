# Communication: Signal Over Noise

## Real Communication Patterns Under Pressure

During a high-severity incident, communication degrades fast. Left unmanaged, it splinters across mediums: frantic DMs, disjointed Slack threads, unrecorded Zoom chats, and panicked texts from account managers to developers.

This fragmentation destroys situational awareness. The Incident Commander (IC) loses track of state changes. Responders duplicate effort because they missed a DM. Left in the dark, stakeholders bypass official channels and interrupt the people actively debugging the system.

Worse is the "stream of consciousness" anti-pattern. Engineers often dump unformatted log traces, 50-line SQL queries, and random guesses ("DNS?", "Could be that PR from Tuesday?") directly into the main incident channel. This creates a wall of text. Incoming responders have to scroll for ten minutes just to figure out what's broken.

## What Information is *Actually* Useful

Incident communication requires ruthless filtering. You aren't there to document every thought. You're there to align the team on system state, business impact, and next steps.

Stakeholders and executives don't care that "the Redis maxmemory-policy is set to volatile-lru." That's technical trivia. They care about business impact.

Keep updates to the **Status, Impact, Action, ETA** format:

* **Status**: What is the system doing right now? (e.g., "Checkout service is completely down.")
* **Impact**: How does this affect the user or business? (e.g., "Customers cannot complete purchases. Estimating $50k/hr revenue loss.")
* **Action**: What are we doing to mitigate it? (e.g., "Rolling back the 'payments-v2' deployment.")
* **ETA**: When is the next update? (e.g., "Next update in 15 minutes or when the rollback completes.")

If an update lacks these elements, it's probably noise.

## Enforcing Clarity

The IC and Communications Lead must actively police channels to protect the signal-to-noise ratio.

**1. Manage Unassigned Observers**
Major incidents attract well-meaning engineers who aren't on call. They join the channel to offer unsolicited advice or ask architecture questions. The IC needs to shut this down.

* *Action*: "If you aren't assigned a task, please hold all questions. We need to keep this channel clear for mitigation."

**2. Ban DMs for Incident Tasks**
If you make a decision or run a command, put it in the main channel. DMs hide state changes. If a responder DMs the IC to say "I'm restarting the primary database," the IC must force them to repeat it in the public channel.

**3. Thread the Debugging Context**
Raw logs, stack traces, and deep technical debates belong in Slack threads. The main channel is for state changes, approvals, and summaries. If someone drops a 100-line stack trace in the main channel, tell them to delete it and move it to a thread.

## Engineering vs. Leadership Context

Tailor your updates to the audience.

**Engineering Communication (Internal Bridge / Slack):**

* **Tone**: Concise, technical, explicit.
* **Focus**: State changes, hypotheses, approvals, and metrics.
* **Example**: "IC, requesting permission to kill stuck Celery workers on nodes 4-8. They're deadlocked on the DB lock."

**Leadership / Stakeholder Communication (Status Page / Exec Updates):**

* **Tone**: Professional, objective, business-focused.
* **Focus**: Customer impact, mitigation strategy, timelines, and risk.
* **Example**: "To the Executive Team: We have a full outage of the checkout system affecting North American users due to a database lock. Engineering is restarting backend workers to clear the lock. We expect partial recovery within 15 minutes. Next update at X:XX."

Never lie, never guess, and never promise a resolution time you can't guarantee. If you don't know the root cause, say "We are actively investigating the latency spike." A reliable update cadence (e.g., every 20 minutes) matters more than having the final answer right away. Once leadership knows *when* you'll update them next, they'll stop interrupting you.
