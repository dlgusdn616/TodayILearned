# Clique
#Ethereum/Consensus

## Technical specification
- - - -
### Constants.
* `EPOCH_LENGTH`: 체크포인트가 되는 블록의 개수, 달성됐을시 처리지연 중인 투표들을 리셋한다. 테스트넷에는 `30000` 으로 설정이 되어 있고 메인넷에는 `etahsh` epoch을 따르고 있다.

* `BLCOK_PERIOD`: 15s for the testnet to remain analogous to the mainnnet `ethash` target.

* `EXTRA_VANITY`: Fixed number of extra-data prefix bytes reserved for signer vanity.
	* Suggested `32 bytes` to retain the current extra-data allowance and/or use.

* `EXTRA_SEAL`: Fixed number of extra-data suffix bytes reserved for signer seal. 
	* `65 bytes` fixed as signatures are based on the standard `secp256k1` curve
* `NONCE_AUTH`: Magic nonce number `0xffffffffffffffff` to vote on adding a new signer.
* `NONCE_DROP`: Magic nonce number `0x0000000000000000` to vote on removing a signer.
* `UNCLE_HASH`: Always `Keccack256(RLP[]))` as uncles are meaningless outside of PoW. 
* `DIFF_NOTRUN`: Block score (difficulty) for blocks containing out-of-turn signatures.
	* Suggested `1` since it just needs to be an arbitrary baseline constant. 
* `DIFF_INTURN`: Block score (difficulty) for blocks containing in-turn signatures.
	* Suggested `2` to show a slight preference over out-of-turn signatures.

### Per-block Constants.

* `BLOCK_NUMBER`: Block height in the chain, where the height of the genesis is block `0`.
* `SIGNER_COUNT`: Number of authorized signers valid at a particular instance in the chain.
* `SIGNER_INDEX`: Index of the block signer in the stored list of current authorized signers.
* `SIGNER_LIMIT`: Number of consecutive blocks out of which a signer may only sign one.
	* Must be `floor(SIGNER_COUNT / 2) + 1` to enforce majority consensus on a chain.

### Repurpose  the `ethash` header fields as follows:

* `beneficiary`: Address to propose modifying the list of authorized signers with.
	* Should be filled with zeroes normally, modified only while voting.
	* Arbitrary values are permitted nonetheless (even meaningless ones such as voting out non signers) to avoid extra complexity in implementations around voting mechanics.
	* **Must** be filled with zeroes on checkpoint (i.e. epoch transition) blocks.
* `nonce`: Signer proposal regarding the account defined by the `beneficiary` field.
	* Should be `NONCE_DROP` to propose reauthorizing `beneficiary` as a existing signer.
	* Should be `NONCE_AUTH` to propose authorizing `beneficiary` as a new signer.
	* **Must** be filled with zeroes on checkpoint (i.e. epoch transition) blocks.
	* **Must** not take up any other value apart from the two above (for now).
* `extraData`: Combined field for signer vanity, checkpointing and signer signatures.
	* First `EXTRA_VANITY` bytes (fixed) may contain arbitrary signer vanity data.
	* Last `EXTRA_SEAL` bytes (fixed) is the signer's signature sealing the header.
	* Checkpoint blocks **must** contain a list of signers (`N*20 bytes`) in between, **omitted** otherwise.
	* The list of signers in checkpoint block extra-data sections **must** be stored in ascending order.
* `mixHash`: Reserved for fork protection logic, similar to the extra-data during the DAO.
	* **Must** be filled with zeroes during normal operation.
* `ommersHash`: **Must** be `UNCLE_HASH` as uncles are meaningless outside of PoW.
* `timestamp`: **Must** be at least the parent timestamp + `BLOCK_PERIOD`.
* `difficulty`: Contains the standalone score of the block to derive the quality of a chain.
	* **Must** be `DIFF_NOTURN` if `BLOCK_NUMBER % SIGNER_COUNT != SIGNER_INDEX`
	* **Must** be `DIFF_INTURN`if `BLOCK_NUMBER % SIGNER_COUNT == SIGNER_INDEX`

### Authorizing a block
- - - -
블록을 허가 및 생산하기 위해서 서명자는 서명을 제외한 모든 걸 포함하고 있는 블록해시에 서명을 해야한다. 해시는 헤더의 모든 필드를 포함(`nonce` and `mixDigest` included)하고 있으며 `extraData` with exception of the 65 byte signature 가 뒤에 붙게 된다..The fields are hashed in the order of their definition in the yellow paper.

This hash is signed using the standard `secp256k1`  curve, and the resulting 65 byte signature (R, S, V, where V is 0 or 1) is embedded into the `extradata` as trailing 65 byte suffix.

To ensure malicious signers (loss of signing key) cannot wreck havoc in the network, each signer is allowed to sign **maximum one** out of `SIGNER_LIMIT` consecutive blocks. The order is not fixed, but in-turn signing weighs more (`DIFF_INTURN`) than out of turn one (`DIFF_NOTURN`).

### Authorization strategies
As long as signers conform to the above specs, they can authorize and distribute blocks as they see fit. The following suggested strategy will however reduce network traffic and small forks, so it's a suggested feature:
* If a signer is allowed to sign a block (is on the authorized list and didn't sign recently).
	* Calculate the optimal signing time of the next block (parent + `BLOCK_PERIOD)`).
	* If the signer is in-turn, wait for the exact time to arrive, sign and broadcast immediately.
	* If the signer is out-of-turn, delay signing by `rand(SIGNER_COUNT * 500ms)`.

### Voting on signers
- - - -
Every epoch transition (genesis block included) acts as a stateless checkpoint, from which capable clients should be able to sync without requiring any previous state. This means epoch headers must not contain votes, all non settled votes are discarded, and tallying starts from scratch.

For all non-epoch transition blocks:
* Signers may cast one vote per own block to propose a change to the authorization list.
* Only the latest proposal per target beneficiary is kept from a single signer.
* Votes are tailed live as the chain progresses (concurrent proposals allowed).
* Proposals reaching majority consensus `SIGNER_LIMIT` come into effect immediately.
* Invalid proposals are not to be penalized for client implementation simplicity.
**A proposal coming into effect entails discarding all pending votes for that proposal (both for and against) and starting with a clean slate.**

### Cascading votes
A complex corner case may arise during signer deauthorization. When a previously authorized signer is dropped, the number of signers required to approve a proposal might decrease by one. This might cause one or more pending proposals to reach majority consensus, the execution of which might further cascade into new proposals passing.

Handling this scenario is non obvious when multiple conflicting proposals pass simultaneously (e.g. add a new signer vs. drop an existing one), where the evaluation order might drastically change the outcome of the final authorization list. Since signers may invert their own votes in every block they mint, it's not so obvious which proposal would be "first".

To avoid the pitfalls cascading executions would entail, the Clique proposal explicitly forbids cascading effects. In other words: **Only the** `beneficiary` **of the current header/vote may be added to/dropped from the authorization list. If that causes other proposals to reach consensus, those will be executed when their respective beneficiaries are "touched" again (given that majority consensus still holds at that point).**

### Voting strategies
Since the blockchain can have small reorgs, a naive voting mechanism of "cast-and-forget" may not be optimal, since a block containing a singleton vote may not end up on the final chain.

A simplistic but working strategy is to allow users to configure "proposals" on the signers (e.g. "add 0x...", "drop 0x..."). The signing code can then pick a random proposal for every block it signs and inject it. This ensures that multiple concurrent proposals as well as reorgs get eventually noted on the chain.

This list may be expired after a certain number of blocks / epochs, but it's important to realize that "seeing" a proposal pass doesn't mean it won't get reorged, so it should not be immediately dropped when the proposal passes.

## Background
- - - -
Ethereum's first official testnet was Morden. It ran from July 2015 to about November 2016, when due to the accumulated junk and some testnet consensus issues between Geth and Parity, it was finally laid to rest in favor of a testnet reboot.

Ropsten was thus born, clearing out all the junk and starting with a clean slate. This ran well until the end of February 2017, when malicious actors decided to abuse the low PoW and gradually inflate the block gas limits to 9 billion (from the normal 4.7 million), at which point sending in gigantic transactions crippling the entire network. Even before that, attackers attempted multiple extremely long reorgs, causing network splits between different clients, and even different versions.

The root cause of these attacks is that a PoW network is only as secure as the computing capacity placed behind it. Restarting a new testnet from zero wouldn't solve anything, since the attacker can mount the same attack over and over again. The Parity team decided to go with an emergency solution of rolling back a significant number of blocks, and enacting a soft-fork rule that disallows gas limits above a certain threshold.

While this solution may work in the short term:

It's not elegant: Ethereum supposed to have dynamic block limits
It's not portable: other clients need to implement new fork logic themselves
It's not compatible with sync modes: fast and light clients are both out of luck
It's just prolonging the attacks: junk can still be steadily pushed in ad infinitum
Parity's solution although not perfect, is nonetheless workable. I'd like to propose a longer term alternative solution, which is more involved, yet should be simple enough to allow rolling out in a reasonable amount of time.

## Standardized proof-of-authority
- - - -
As reasoned above, proof-of-work cannot work securely in a network with no value. Ethereum has its long term goal of proof-of-stake based on Casper, but that is heavy research so we cannot rely on that any time soon to fix today's problems. One solution however is easy enough to implement, yet effective enough to fix the testnet properly, namely a proof-of-authority scheme.

Note, Parity does have an [implementation of PoA](https://github.com/paritytech/parity-ethereum), though it seems more complex than needed and without much documentation on the protocol, it's hard to see how it could play along with other clients. I welcome feedback from them on this proposal from their experience.

The main design goals of the PoA protocol described here is that it should be very simple to implement and embed into any existing Ethereum client, while at the same time allow using existing sync technologies (fast, light, warp) without needing client developers to add custom logic to critical software.

### Proof-of-authority 101
- - - -
For those not aware of how PoA works, it's a very simplistic protocol, where instead of miners racing to find a solution to a difficult problem, authorized signers can at any time at their own discretion create new blocks.

The challenges revolve around how to control minting frequency, how to distribute minting load (and opportunity) between the various signers and how to dynamically adapt the list of signers. The next section defines a proposed protocol to handle all these scenarios.

### Rinkeby proof-of-authority
There are two approaches to syncing a blockchain in general:

* The classical approach is to take the genesis block and crunch through all the transactions one by one. This is tried and proven, but in Ethereum complexity networks quickly turns out to be very costly computationally.
* The other is to only download the chain of block headers and verify their validity, after which point an arbitrary recent state may be downloaded from the network and checked against recent headers.

A PoA scheme is based on the idea that blocks may only be minted by trusted signers. As such, every block (or header) that a client sees can be matched against the list of trusted signers. The challenge here is how to maintain a list of authorized signers that can change in time? The obvious answer (store it in an Ethereum contract) is also the wrong answer: fast, light and warp sync don't have access to the state during syncing.

**The protocol of maintaining the list of authorized signers must be fully contained in the block headers.**

The next obvious idea would be to change the structure of the block headers so it drops the notions of PoW, and introduces new fields to cater for voting mechanisms. This is also the wrong answer: changing such a core data structure in multiple implementations would be a nightmare development, maintenance and security wise.

**The protocol of maintaining the list of authorized signers must fit fully into the current data models.**

So, according to the above, we can't use the EVM for voting, rather have to resort to headers. And we can't change header fields, rather have to resort to the currently available ones. Not much wiggle room.

### Repurposing header fields for signing and voting
The most obvious field that currently is used solely as fun metadata is the 32 byte **extra-data** section in block headers. Miners usually place their client and version in there, but some fill it with alternative "messages". The protocol would extend this field to with 65 bytes with the purpose of a secp256k1 miner signature. This would allow anyone obtaining a block to verify it against a list of authorized signers. It also makes the **miner** section in block headers obsolete (since the address can be derived from the signature).

*Note, changing the length of a header field is a non invasive operation as all code (such as RLP encoding, hashing) is agnostic to that, so clients wouldn't need custom logic.*

The above is enough to validate a chain, but how can we update a dynamic list of signers. The answer is that we can repurpose the newly obsoleted **miner** field and the PoA obsoleted **nonce** field to create a voting protocol:

* During regular blocks, both of these fields would be set to zero.
* If a signer wishes to enact a change to the list of authorized signers, it will:
	* Set the **miner** to the signer it wishes to vote about
	* Set the **nonce** to `0` or `0xff...f` to vote in favor of adding or kicking out

Any clients syncing the chain can "tally" up the votes during block processing, and maintain a dynamically changing list of authorized signers by popular vote.

To avoid having an infinite window to tally up votes in, and also to allow periodically flushing stale proposals, we can reuse the concept of an epoch from ethash, where every epoch transition flushes all pending votes. Furthermore, these epoch transitions can also act as stateless checkpoints containing the list of current authorized signers within the header extra-data. This permits clients to sync up based only on a checkpoint hash without having to replay all the voting that was done on the chain up to that point. It also allows the genesis header to fully define the chain, containing the list of initial signers.

### Attack vector: Malicious signer

It may happen that a malicious user gets added to the list of signers, or that a signer key/machine is compromised. In such a scenario the protocol needs to be able to defend itself against reorganizations and spamming. The proposed solution is that given a list of N authorized signers, any signer may only mint 1 block out of every K. This ensures that damage is limited, and the remainder of the miners can vote out the malicious user.

### Attack vector: Censoring signer

Another interesting attack vector is if a signer (or group of signers) attempts to censor out blocks that vote on removing them from the authorization list. To work around this, we restrict the allowed minting frequency of signers to 1 out of N/2. This ensures that malicious signers need to control at least 51% of signing accounts, at which case it's game over anyway.

### Attack vector: Spamming signer

A final small attack vector is that of malicious signers injecting new vote proposals inside every block they mint. Since nodes need to tally up all votes to create the actual list of authorized signers, they need to track all votes through time. Without placing a limit on the vote window, this could grow slowly, yet unbounded. The solution is to place a ~~moving~~ window of W blocks after which votes are considered stale. ~~A sane window might be 1-2 epochs.~~ We'll call this an epoch.

### Attack vector: concurrent blocks

If the number of authorized signers are N, and we allow each signer to mint 1 block out of K, then at any point in time N-K+1 miners are allowed to mint. To avoid these racing for blocks, every signer would add a small random "offset" to the time it releases a new block. This ensures that small forks are rare, but occasionally still happen (as on the main net). If a signer is caught abusing it's authority and causing chaos, it can be voted out.







`func (api *API) GetSnapshot(number *rpc.BlockNumber)  (*Snapshot, error)`

`func(api *API) GetSnapshotAtHash(hash common.Hash) (*Snapshot, error)`

```
type Snapshot struct {
	Number uint64	`json:"number"` // Block number where the snapshot was created
	Hash common.Hash	`json:"hash"` // Block hash where the snapshot was created
	Signers map[common.Address]struct{}	`json:"signers"` // Set of authorized signers at this moment
	Recentes map[uint64]common.Address	`json:"recents"` // Set of recent signers for spam protections
	Votes []*Vote	`json:"votes"` // List of votes cast in chronological order
	Tally map[common.Address]Tally	`json:"tally"`// Current vote tally to avoid recalculating filtered or unexported fields.
```

```
type Tally struct {
	Authorize bool `json:"authorize"` // Whether the voite is about authorizing or kicking someone
	Votes int `json:"votes"`	// Number of votes until now wanting to pass the proposal
} 	
```

```
type Vote struct {
	Signer common.Address `json:"signer"` // Authorized signer that cast this vote
	Block uint64 	`json:"block"` // Block number the vote was cast in (expire old votes)
	Address common.Address `json:"address"` // Account being voted on to change its authorization
	Authorize bool `json:"authorize"` // Wether to authorize or deauthorize the voted account
}
```

`func(api *API) Proposals() map[common.Address]bool`
Proposals returns the current proposals the node tries to uphold and vote

`func (api *API) Propose(address common.Address, auth bool)`

type Clique

`func New(config *params.CliqueConfig, db ethdb.Database) *Clique`
New creates a Clique proof-of-authority consensus engine with the initial signers set to the ones provided by the user.

```
type CliqueConfig struct {
	Period uint64 `json:"period"` // Number of seconds between blocks to enforce
	Epoch uint64 `json:"epoch"`	// Epoch length to reset votes and checkpoint
}
```

```
type Database interface {
	Putter
	Deleter
	Get(key []byte) ([]byte, error)
	Has(key []byte) (bool, error)
	Close()
	NewBatch() Batch
```

`func (c *Clique) Author(header *types.Header) (common.Address, error)`
Author implements consensus.Engine, returning the Ethereum address recovered from the signature in the header's extra-data section.

`func (c *Clique) Authorize(signer common.Address, sigFn SignerFn)`
Authorize injects a private key into the consensus engine to mint new blocks with.

`func (c *Clique) CalcDifficulty(chain consensus.ChainReader, time uint64, parent *types.Header) *big.Int`
CalcDifficulty is the difficulty adjustment algorithm. It returns the difficulty that a new block should have based on the previous blocks in the chain and the current signer.

`func (*Clique) Close`

