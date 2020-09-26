# Governance Council

The Polymesh Governance Council is ultimately responsible for actioning PIPs which were submitted either via the community curation process, or directly by one of the committees.

Any member of the governance council can trigger a PIP snapshot. The snapshot summarises the current list of PIPs (both from the community and committees) and orders community based PIPs based on their signal.

The Governance Council then meet to discuss the PIPs included in the snapshot. For community currated PIPs, the Governance Council must work through them based on their curated order, choosing whether to ratify, reject or skip each PIP in turn.

For PIPs which are skipped, we track how many times they have been skipped, and limit the Governance Council to skipping a particular PIP too many times. This is designed to give the Governance Council the flexibility they need to reasonable govern the evolution of the Polymesh network, as well as ensuring that community signalling of the relative importance of PIPs is closely considered by the Governance Council.

For PIPs submitted by committees rather than through the community curation process, the Governance Council is free to ratify or reject these PIPs in any order.

The Governance Concil can be thought of as a multisig controlled by its members, and has an associated voting threshold that must be reached in order to execute an action through the Governance Council. This applies both for PIP management as well as some additional non-PIP related actions that the Governance Council may need to execute.

These non-PIP actions include managing the membership of the Governance Council itself, with existing members needing to agree and vote on the addition or removal of members, as well as changes to the voting structure, for example the voting threshold.

The Goverance Council is also able to issue a Customer Due Diligence claim (TODO: add link) should the need arise.

## Release Coordinator

One member of the Governance Council is elected as the Release Coordinator. The role of the Release Coordinator is to schedule PIPs that have been ratified by the Governance Council.

Every ratified PIP has a default execution time, set as a specific amount of blocks from the time it was ratified. The Release Coordinator can re-schedule any PIP to a different execution block or choose to enact it immediately. This provides flexibility to coordinate the release of critical fixes, and ensure that any stakeholders in a particular PIP are coordinated and well prepared before the PIP is executed.