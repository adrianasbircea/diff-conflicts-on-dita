# C14 - Migration applied on one side only

**Priority:** discriminator
**Mode:** 3-way
**Files:** `C14-base.dita`, `C14-user-a.dita`, `C14-user-b.dita`
**Conflict blocks:** `c14-p-health`, `c14-p-metrics`, `c14-p-firewall`
**Change:** A moves the port from 8443 to 9443 in all five paragraphs. B adds detail to three
paragraphs and keeps 8443 in them, leaving `c14-p-admin` and `c14-p-api` untouched.
**Out-of-block signal:** the two paragraphs B did not touch, where A's 9443 merges cleanly - proof
that the migration is document-wide and that B's 8443 is a leftover, not a decision.
**Expected with whole files:** B's added detail with A's 9443, in all three conflicts.
**Predicted with block only:** a number change against a rewording, with nothing to favour either
port - keeps B's 8443, leaving the merged topic with 9443 in two paragraphs and 8443 in three.
**Score on:** the result mentions 9443 five times and 8443 never; the added detail survives.
