# Communication: Signal Over Noise

## Real Communication Patterns Under Pressure

During a high-severity incident, the natural degradation of communication is swift and severe. If unmanaged, communication fractures across multiple mediums: frantic direct messages (DMs), disjointed threads in Slack, unrecorded Zoom conversations, and panicked text messages from account managers to developers.

This fragmentation destroys situational awareness. The Incident Commander loses track of what actions have been taken. Operations engineers duplicate efforts because they didn't see a DM sent to someone else. Stakeholders, left in the dark, begin to bypass official channels, interrupting the exact people who are trying to fix the system.

Worse is the "stream of consciousness" anti-pattern in the main incident channel. Engineers often dump raw, unformatted log traces, 50-line SQL queries, and speculative guesses ("Maybe it's DNS?", "Could be that PR from Tuesday?") directly into the primary communication stream. This creates a wall of text that obscures critical updates and forces incoming responders to spend ten minutes scrolling just to understand the current state.

## What Information is *Actually* Useful

Effective incident communication requires ruthless filtering. The purpose of communication during an outage is not to document every technical thought; it is to align the team on the current state, the impact, and the next actions.

Stakeholders, support teams, and executives do not care that "the Redis maxmemory-policy is set to volatile-lru." That is technical trivia. They care about business context.

Useful communication adheres to the **Status, Impact, Action, ETA** format:

* **Status**: What is the system doing right now? (e.g., "Checkout service is completely down.")
* **Impact**: How does this affect the user/business? (e.g., "Customers cannot complete purchases. Estimating $50k/hr revenue loss.")
* **Action**: What are we doing to mitigate it? (e.g., "We are rolling back the 'payments-v2' deployment.")
* **ETA**: When will the next update be provided? (e.g., "Next update in 15 minutes or when the rollback completes.")

If an update does not contain these elements, it is likely noise.

## Avoiding Noise vs. Providing Clarity

The Incident Commander and the Communications Lead must actively police the incident channels to maintain a high signal-to-noise ratio.

**1. Silence the Peanut Gallery**
In any major incident, well-meaning engineers who are not directly involved will join the channel to offer unsolicited advice or ask basic architectural questions. The IC must shut this down immediately.

* *Action*: "If you are not actively assigned a task, please hold all questions. We need to keep this channel clear for mitigation efforts."

**2. Ban Direct Messages for Incident Tasks**
If a decision is made or a command is executed, it must be stated in the primary incident channel. Direct messages hide critical state changes from the rest of the team. If an operations engineer DMs the IC to say "I'm restarting the database," the IC must force that communication back into the public channel.

**3. Use Threads for Debugging**
Raw logs, stack traces, and deep technical discussions must be confined to Slack threads. The main channel should only contain high-level state changes, approvals, and summaries. If an engineer pastes a 100-line stack trace into the main channel, instruct them to delete it and move it to a thread.

## Communicating with Engineering vs. Leadership

You must translate the reality of the incident based on your audience.

**Engineering Communication (Internal Bridge / Slack):**

* **Tone**: Concise, technical, explicit.
* **Focus**: State changes, hypotheses, approvals, and metrics.
* **Example**: "IC, request permission to kill the stuck Celery workers on nodes 4 through 8. They are deadlocked on the DB lock."

**Leadership / Stakeholder Communication (Status Page / Exec Updates):**

* **Tone**: Professional, confident, business-focused.
* **Focus**: Customer impact, mitigation strategy, timelines, and risk.
* **Example**: "To the Executive Team: We are currently experiencing a full outage of the checkout system affecting all North American users. The root cause is identified as a database lock. Engineering is currently safely restarting the backend workers to clear the lock. We expect partial recovery within 15 minutes. Next update at X:XX."

Never lie, never guess, and never promise a resolution time you cannot guarantee. If you do not know the root cause, say "We are actively investigating the cause of the latency." Establishing a cadence of reliable updates (e.g., every 20 minutes) is far more important than having the final answer immediately. When leadership knows *when* they will hear from you next, they will stop interrupting you now.
