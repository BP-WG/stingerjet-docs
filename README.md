# StingerJet: wallet declarative language

Design principles:
- human-readable; small visual clutter (python-like, thus "stinger")
- supporint client-side-validation
- deterministic and fully transcodable with bitcoin script (`decode(encode(jet)) = jet; encode(decode(bitcoin_script)) = bitcoin_script`; this means LN support)
- supporting any existing scriptPubkey descriptors
- compact: frequently used complex expressions can be convolved into a short terms ("jets")
- evolving: new jets may be added with time (for new bitcoin features etc)

Example:
```bash
# This means derivation standard used. Here we use m/33/chain/org/unit/account/scope/index
use lnpbp derivation

# This defines organization units in derivation (see "unit" in the path above).
let board = unit 1

# This defines account index used by all participants under the same org unit
# (see "account" in the path above).
let current = account 2

# These are participant-specific components of derivation paths:
# - seed fingerprint
# - organization index (different participants may use different indexes for the same org)
# - organization unit
# - account index - taken from the definition above
let maxim = seed bf8564fd org 2 board current xpub.... with descriptor-wallet at paris-server
let giacomo = seed 88eca544 org 5 board current xpub.... with electrum at desktop-home
let olga = seed 9245a67c org 0 board current xpub.... with bitbox02 at home
let lawyer = seed 7835dc3d org 122 board current xpub.... with trezor at bank-safe-box

# These defines a group of users participating in multisig
let boardMembers = group maxim, giacomo, olga

descriptors: tr, wsh, shWsh

# Conditions which must be satisfied for UTXO spending:
# they are organized as a list, transformed into a tree for taproot
# outputs or linearized with OR operators for other types of outputs.
# For taproot the order of conditions must follow the probability
# of their usage (DFS ordering).
satisfying any:
  all of ordered boardMembers signed
  tapret          # here we allow wallets to put tapret commitments
  at least 2 of ordered boardMembers signed after 2 years
  at least 1 of ordered boardMembers signed after 3 years
  lawyer signed after 5 years

scopes: normal, change, rgb20, rgb21, rgb30
indexes used:
  normal up to 54
  change up to 98
  rgb20 up to 12
  rgb21 up to 5
  rgb30 up to 876
taprets:
  rgb20/1   <~9jqo^BlbD-BleB1DJ+*+F(f pos 1
  normal/5  ,q/0JhKF<GL>Cj@.4Gp$d7F!, pos 1
  rgb30/35  L7@<6@)/0JDEF<G%<+EV:2F!, pos 1
```
