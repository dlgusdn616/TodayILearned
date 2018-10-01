# Mnemonics, PrivateKeys
#Ethereum

## 작업 매커니즘
[Mnemonic-Creator](https://freewallet.org/bip39-recovery-from-mnemonic-bitcoin-and-other-cryptocurrencies)
니모닉 생성기로 단어들을 넣고 Private Key들을 쭈욱 추출할 수 있다. 단, 생성기를 이용할 때는 온라인에서 이용하는 것이 아니라 오프라인 환경에서 이용할 수 있도록 한다.

* 프라이빗 키를 잃어버렸다면 어떻게 대처할 수 있을까?
1. 니모닉 생성기에 자신이 백업해둔 니모닉 + 규약(BIP32 or 44 or 49 or 141)을 조합하여 입력한다.
2. 개인키와 공개키가 쭈욱 나열될 것이다. 이 중 자신이 백업해둔 Path (m/0/2)에 매칭되는 공개키, 개인키를 사용하면 된다.

* 백업해둬야  할 데이터
1. Mnemonics (만약 passphrase까지 설정해줬다면, 함께 기억해야 한다.)
2. Derivation Path (m/0/0)

위 둘을 기억해야 개인키와 공개키를 되찾을 수 있다. 
관련 튜토리얼 글은 아래의 링크에 잘 기술되어 있다.
[How To Use Ian Coleman's BIP39 Tool For Finding Bitcoin Addresses And Private Keys From A Seed Phrase - Forkdrop.io](https://forkdrop.io/using-ian-colemans-bip-39-tool)
[How To Run Ian Coleman's BIP39 Tool In A Secure Offline Ubuntu 16.04 Temporary Live Boot Session - Forkdrop.io](https://forkdrop.io/running-bip-39-tool-on-secure-offline-ubuntu-system)

## Security Issues
[Are 12-word Seeds for Bitcoin Private Key Secure?](https://www.reddit.com/r/Bitcoin/comments/6twuj1/are_12word_seeds_for_bitcoin_private_keys_secure/)
[BIP39 brute-force complexity (or how hard it is to break someone's secret words) - Education - Cardano Forum](https://forum.cardano.org/t/bip39-brute-force-complexity-or-how-hard-it-is-to-break-someones-secret-words/12650/12)

## 관련 BIP 정리
* [bip39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) only describes how to generate a mnemonic, and how the binary seed is derived from it, but not how to use that seed to implement wallets.

* [bip32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) describes how to derive a hierarchy from a binary seed to implement deterministic wallets.

* [bip44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) standardizes how to use the BIP44 hierarchy. This includes the concept of accounts. It also points to SLIP44 which registers constants to use for different coins.



## Github
* All mnemonics can go to keys but you can't go from key to mnemonic.
* MyEtherWallet

## Reddit
* Every mnemonic has (multiple) private keys, but not every key is a valid mnemonic due to the word list.

* mnemonic을 생성할 수 있는 방법은 다양한데, 

## MyEtherWallet
### How to Create a Wallet
* H/W Wallet의 경우 아래의 제품들을 추천한다. 보안성 및 MEW와의 상호 작용도 가능하다. 가격도 100달러 미만으로 저렴하다.
	* Ledger Nano S
	* TREZOR
	* Digital Bitbox

위 기기들은 키를 보관하는 기능과 트랜잭션 발생시 서명하는 기능까지 제공한다. MEW는 위 기기들과 통합되어 있기 때문에 친숙한 인터페이스 또한 친숙할 것이다. 이러한 특징들은 사용자들이 인터넷으로부터 차단되는 효과를 준다.
* Phishers can't get them, Malware can't get them, Keyloggers can't get them.

24개의 단어(your key)만 오프라인으로 잘 보관하면 된다., (e.g fire proof cabinet or perhaps in a safety deposit box at your local bank). 24개의 단어는 절대로 온라인 디바이스 및 MEW 사이트에서 입력하지 않도록 한다.

### private key와 keystore의 차이점
#### Keystore File (UTC / JSON)
* 유저가 선택한 password에 의해 암호화된다.
* This is the recommended version to save.
* This Keystore file matches the format used by Mist so you can easily import it in the future.
* Make sure to have multiple backups.

#### Mnemonic Phrases
* 보통 12단어 또는 24단어로 구성된다.
* 하나의 니모닉으로부터 여러 개의 주소가 파생될 수 있다. 즉, 하나의 니모닉은 많은 계정, 주소, 그리고 개인키를 만들어낼 수 있다.
* Ledger, TREZOR, Metamask, and Jaxx create these for you.
* MEW does not currently derive mnemonic phrases, but may in the future.

#### Private Keys (unencrypted)
* This is unencrypted text version of your private key, meaning no password is necessary.
* If someone were to find your unencrypted private key, they could access your wallet without a password.


### Change your wallet password (Unencrypted -> Encrypted)
* 암호화 되지 않은 개인키를 암호화된 개인키로 변환할 수 있을까?
> MEW의 Chrome Extension을 사용하거나 importing your raw private key into geth를 하는 방법으로 개인키를 암호화 시킬 수 있다.  

Chrome Extension



## Ethereum StackExchange
### 이더리움 계정은 어떻게 생성되는가?
* 16년도에 작성된 답글이므로 최신 버전의 Yellow Paper를 참조해야하나, 아직 관련 내용을 찾지 못했다. 어디 나와있는 걸까..?
1. Create a random private key (64 (hex) characters / 256 bits / 32 bytes)
2. Derive the public key from this private key (128 (hex) characters / 512 bits / 64 bytes)
3. Derive the address from this public key. (40 (hex) characters / 160 bits / 20 bytes)

일반적으로 공개키를 주소라고 인식하는 경향이 있는데, 이더리움에서는 적용이 되지 않는 개념이다. There is a separate public key that acts as a middleman that you won't ever see, unless you go poking around a pre-sale wallet JSON file.

#### 1. Generating private key
The private key is 64 hexadecimal characters. Every single string of 64 hex are, hypothetically, an Ethereum private key that will access an account. If you plan on generating a new account, you should be sure these are seeded with a proper RNG. Once you have that string..

#### 2. Private Key -> Public Key
This is hard and beyond normal. There is something with ECDSA and stuff. But in the end you end up with a public key that is 64 bytes.

#### 3. Public key -> Address
1. Start with public key (128 characters / 64 bytes)
2. Take the Keccak-256 hash of the public key. You should now have a string that is 64 characters / 32 bytes. (note: SHA3-256 eventually become the standard)
3. Take the last 40 characters / 20 bytes of this public key (Keccak-256). Or, In other words, drop the first 24 characters / 12 bytes. These 40 characters / 20 bytes are the address. When prefixed with 0x it becomes 42 characters long.

### How to import a plain private key into geth or Mist?
1. Paste the key into a text file, save it to disk and use the path to that file with `geth account import <file path>`. 
2. Using geth console like `web3.personal.importRawKey("<Private Key>", "<New Password>")`


## Medium
### BIP 0039 Mnemonic Phrases
* A mnemonic phrase, mnemonic recovery phrase or mnemonic seed is a list of words which store all the information needed to recover a Bitcoin wallet

## Steemit
