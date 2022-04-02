# StingerJet: wallet declarative language

Design principles:
- human-readable; small visual clutter (python-like, thus "stinger")
- supporint client-side-validation
- deterministic and fully transcodable with bitcoin script (`decode(encode(jet)) = jet; encode(decode(bitcoin_script)) = bitcoin_script`; this means LN support)
- supporting any existing scriptPubkey descriptors
- compact: frequently used complex expressions can be convolved into a short terms ("jets")
- evolving: new jets may be added with time (for new bitcoin features etc)
