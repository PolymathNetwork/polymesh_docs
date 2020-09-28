Polymesh provides protocol layer logic on top of a distributed storage ledger.

Updates to the ledger are processed across a decentralised network of Polymesh nodes.

Polymesh is a permissioned network, and in order to run an operator node, the associated identity must be permissioned through an on-chain [governance process](./governance.md).

Polymesh is built on top of Substrate, and uses BABE block production with a GRANDPA finality gadget. This separates block production from the block finalisation process efficiently, allowing the blockchain to have a high throughput and rapid non-probabilistic finalisation.

Polymesh operators are real world entities, typically regulated or licensed in their home jurisdiction. Requiring a diverse set of high reputation organisations to reach byzantine consensus offers an additional strong security guarantee to the network.

For more details on Consensus see:  
<https://substrate.dev/docs/en/knowledgebase/advanced/consensus>