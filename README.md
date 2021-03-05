# Title

### Intended audience

alkdjflakdsjf

### Introduction

alkjsdflaksdjf

### Glossary

##### Something

laksjdflaskdfj


### Subtitle

alkdsjflakdsfj

### Appendix

##### Table

| Size | Description          | Data type | Comments                                                     |
| ---- | -------------------- | --------- | ------------------------------------------------------------ |
| 4    | version              | int32_t   | Block version, typically 4.                                  |
| 32   | prev_block           | char[32]  | The previous block hash.                                     |
| 32   | merkle_root          | char[32]  | The Merkle tree root of the block's transactions.            |
| 4    | timestamp            | uint32_t  | Block creation timestamp, the 4 least significant bits must be set to 0. Must be greater than the previous block's timestamp. |
| 4    | bits                 | uint32_t  | The difficulty of this block.                                |
| 4    | nonce                | uint32_t  | A nonce, unused in PoS blocks and therefore usually set to 0. |
| 32   | state_root           | char[32]  | The root of the state trie                                   |
| 32   | utxo_root            | char[32]  | The root of the contract utxo balances trie.                 |
| 32   | stake_prevout_hash   | char[32]  | The delegated staking utxo's hash                            |
| 4    | staking_prevout_vout | uint32_t  | The delegated staking utxo's vout index                      |
| 131  | blockSig             | varlength | The block's signature created by the staker concatenated with the PoD created by the delegator during delegation. Note that the field will be prefixed by a 0x82 byte specifying the length. |

