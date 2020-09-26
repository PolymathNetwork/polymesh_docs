# Polymesh Improvement Proposals

Polymesh Improvement Proposals (PIPs) represent an on-chain action that the community or a committee is recomending to the Governance Council.

Polymesh Improvement Proposals (PIPs) represent any of change to the network, and can be created both by dedicated committees as well as any POLYX token holder and are actioned by the Governance Council.

A PIP is an on-chain dispatchable function w/ parameters alongside some metadata giving some additional context as to why the function should be called. For example, it may be a call to `system::set_code(new_binary)` which is linked to a Github PR describing the change.

These PIP dispatchables can only be executed by the Governance Council and not an individual user.

PIPs are also used to permission certain on-chain identities to have special priviledged roles. This includes adding and removing permissioned operators and CDD service providers.

Some common examples include:  
  - network upgrades (represented as a call to system::set_code TODO: add link) 
  - treasury disbursement
  - tokenomics parameter change
  - adding new permissioned operators
  - adding new permissioned CDD service providers