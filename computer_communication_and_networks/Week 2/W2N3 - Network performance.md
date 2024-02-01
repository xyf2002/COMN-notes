An ideal network would be able to move an unlimited amount of data between any two connected devices, instantaneously and without any data loss. This is (obviously) not possible, as physics introduces delay, loss, and throughput constraints.
# Types of delay
As a packet travels through a network, it suffers from several types of delay at *every* node it encounters.
## Processing delay
When a packet arrives at a node, the header is examined and used to determine where the packet should be directed - this takes some time and is **processing delay**. Processing delays in modern high-speed routers are usually microseconds or less.
## Queuing delay
After a packet is processed, it is added to queue to wait for its transmission link to become available. This is dependent on how many packets have arrived before it that are still waiting to be transmitted, if no packets have arrived recently then it will be close to 0, but if traffic is heavy then there may be a long delay. In practice, queuing delays are between microseconds and milliseconds.
## Transmission delay
A link between routers will have a transmission rate in ($R$ bits per second). If a packet is of size $L$ bits, then it will take $L/R$ seconds to transmit one full packet. Transmission delays tend to be between microseconds and milliseconds in practice.
## Propagation delay
Once a bit is pushed into a link, it must propagate down it to the next router. This is governed by the propagation speed (the speed that the signal can move within the link's medium, e.g. speed of light (~$3*10^8\text{ m/s}$) in fibre lines or the speed of electricity (~$2*10^8\text{ m/s}$) in copper). Propagation delay can be milliseconds in large networks, or nearly negligible in local ones.

## Overall delay
If we let $d_{proc}$, $d_{queue}$, $d_{trans}$, $d_{prop}$ be the processing, queuing, transmission, and propagation delays respectively, then the total nodal delay
$$d_{nodal}=d_{proc}+d_{queue}+d_{trans}+d_{prop}$$
