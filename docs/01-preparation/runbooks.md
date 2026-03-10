# Runbooks: Usage During Emergencies

## The Purpose of a Runbook

A runbook is an operational procedure meant to automate or simplify a known process (e.g., "How to failover the primary database"). We write them during times of peace to execute them during times of war.

However, a runbook is not a substitute for engineering judgment. Blindly following a runbook during an unprecedented failure scenario is a fast way to turn a SEV2 into a SEV1.

## Doctrine for Runbook Execution

When the pager fires and points you to a runbook, adhere to the following rules:

### 1. Read Before Executing

Do not copy-paste commands into a production terminal without understanding what they do.
If a runbook says `kubectl delete pod -l app=payment-gateway`, you must first verify that you are in the correct cluster, in the correct namespace, and that deleting those pods won't cause a massive spike in 500 errors downstream. If you don't understand the command, **do not run it**. Escalate instead.

### 2. State Your Intent (The IC Rule)

As defined in [Incident Coordination](../02-response/incident-coordination.md), Operations engineers must clear actions with the Incident Commander (IC).

* **Correct:** "IC, I am looking at the `payment-failover` runbook. Step 3 requires restarting the Redis cache. I understand the impact and I request permission to proceed."
* **Incorrect:** (Silently running a script from a wiki page while the rest of the team is debugging something else).

### 3. Stop on First Failure

Runbooks are linear. Production systems are non-linear.

If Step 1 of a runbook succeeds, but Step 2 throws an unexpected error, **stop immediately**.

Do not attempt to brute-force Step 2. Do not proceed to Step 3. The runbook's underlying assumptions about system state are now invalid. Continuing to execute the runbook will likely compound the failure. At this point, you must revert to manual debugging and hypothesis testing.

### 4. Runbooks Must Be Reversible

Whenever possible, design runbooks so that running them twice does not cause damage, or so that the actions taken can be easily undone. If a runbook involves destructive actions (like dropping tables or killing instances without graceful shutdown), it must contain massive, explicitly highlighted warnings.

## Runbook Maintenance

A broken runbook is worse than no runbook at all because it provides a false sense of security.

If a runbook fails during an incident, or contains outdated information, **fixing the runbook becomes a mandatory Action Item for the Post-Incident Review (PIR)**.

Do not write "update the runbook" as a vague note. The Action Item must specify exactly what failed, who owns the update, and the deadline. If a runbook is consistently failing or requires manual hacking to work, it should be deleted and rewritten as automation (e.g., a Terraform module, an Ansible playbook, or a script) rather than manual copy-paste markdown instructions.
