# Bitcoin Research - Peer Observer

## peer-observer: A tool and infrastructure for monitoring the Bitcoin P2P network for attacks and anomalies

Over the past few years, I’ve been working on monitoring tools for the Bitcoin network. One of these projects is [peer-observer](https://github.com/0xB10C/peer-observer): A tool and infrastructure for monitoring the Bitcoin P2P network for attacks and anomalies. This post describes the motivation for starting yet another Bitcoin network observer. It details how the tool works, what my honeypot infrastructure looks like, and finishes with an idea for a decentralized Bitcoin Network Operations Collective and incident response team.

### Motivation

At some point in late 2021, I stumbled across reports of an `addr` message flooding having happened a few months earlier on the Bitcoin network. It was first reported by Piotr Narewski, the maintainer of the [Gocoin](https://github.com/piotrnar/gocoin) Bitcoin node implementation, in a thread called [Loads of fake peers advertised on bitcoin network](https://bitcointalk.org/index.php?topic=5348856.0). Piotr details that his node was receiving “hundreds of thousands of non-working [IP] addresses” via the `addr` P2P message in July 2021. Since his node implementation stores all addresses[^1], he is experiencing high resource usage and needs more connection attempts until a working peer is found.

[^1] To avoid this problem, Bitcoin Core’s IP address manager (addrman) does not store all IP addresses it receives. It has a table with a fixed size and a DoS-resistant insertion and eviction policy.

Matthias Grundmann and Max Baumstark from the Karlsruhe Institute of Technology noticed this attack on their DSN Bitcoin Monitoring infrastructure, too. In a [preprint](https://arxiv.org/pdf/2108.00815v1) for a paper, they write: “Some peers in the Bitcoin P2P network distributed a huge amount of spam IP addresses during July 2021. These spam IP addresses did not belong to actual Bitcoin peers.”.