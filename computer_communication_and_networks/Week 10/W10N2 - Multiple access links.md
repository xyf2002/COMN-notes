Unlike point-to-point links, broadcast links may have several nodes each trying to send and receive. This means that access to the link must be coordinated, to prevent nodes from "talking over" each other resulting in garbled data being sent. This is controlled using **multiple access protocols**. Such a protocol should ideally have the following properties over an $R$ bits per second link:
1. If only one node has data to send, that node should have a throughput of $R$ bps
2. If $M$ nodes have data to send, then each of those nodes should have an average throughput of $R/M$ bps over a suitably defined interval of time
3. The protocol is decentralised, with no single control node
4. The protocol is simple to minimise implementation costs

There are 3 categories of multiple access protocols: channel partition, random access, and taking-turns protocols.
# Channel partitioning protocols
#We have looked at [[W2N1 - Network core#Time division multiplexing (TDM)|time division]] and [[W2N1 - Network core#Frequency division multiplexing (FDM)|frequency division]] multiplexing, and this is applicable again. TDM and FDM are both completely fair and prevent all collisions, but come at the cost that each slot is allocated to only one sender, so when some nodes don't have data to transmit the remaining nodes still have to leave empty frames/frequencies for it, violating property 4. A third channel division protocol is **code division multiple access (CDMA)**, which uses carefully selected codes to encode the data that is sent, which can allow multiple nodes to transmit simultaneously and still both be comprehensible to their targets.
# Random access protocols
