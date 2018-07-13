# How do I make my DAPP "Serenity-Proof"?

## Answer #1

31down voteaccepted

It's not certain yet, but:

1. Do NOT rely on very fine-grained calculations of current gas costs. Assume that gas costs of contract calling may go up or down by up to an order of magnitude in a future hard fork.
2. If creating contracts in assembly (ie. not serpent, solidity or LLL), do NOT use dynamic JUMP/JUMPI operations (ie. every JUMP/JUMPI should be **immediately preceded** by a PUSH value specifying the exact point in the code to jump to)
3. Do NOT do anything important based on the DIFFICULTY opcode.
4. Do NOT rely on the assumption of a block time between 12-20 seconds.
5. Do NOT assume that BLOCKHASHes will be a reliable source of randomness.
6. Do NOT assume that tx.origin will continue to be usable or meaningful.
7. Do NOT assume that opcodes other than 0xfe will continue to be invalid.

It's not certain that all of those steps will be required, or that they will be sufficient, but that should get you most of the way there.