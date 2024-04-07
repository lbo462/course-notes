# Transmission Control Protocol

Author: **LBO** 🥖

Before reading this course, note that TCP was invented in a weird context, i.e. the cold war.
We were afraid that the Russians might destroy part of the network.
Hence, TCP was created such that it's able to work while part of the network is down.
The principle allowing this is the __end-to-end principle__, which is a part of the net neutrality law.

> The end-to-end principle states that all the work and computations should be done at each ends of a communication.
> This way, the network isn't aware of the content of the paquets going through it.
> Nowadays, this principle is out-to-date since our phones and other user equipments are likely to be less powerful than our operator's routers.

# Necessary vocabulary (don't skip)

In order to start this course on solid grounds, one should define some terms we'll be using.

With TCP, we send __segments__, not ~~packets~~ since _packet_ is for the IP.

The __MSS__ is the _Maximum Segment Size_, which is the second layer MTU equivalent for the forth layer on the OSI model.

The __flux control__ is control of the segment flux such that the emitters adapts its speed with respect to its receiver.

The __congestion control__ the control of the congestion defined to avoid network congestion without the human intervention.

The __Receiver Window__ (_rwdn_ or _awnd_ for __Advertised Window__) is the maximum amount of segments that the host can receive for each time slot.
It is used for __flux control__.

The __Congestion Window__ (_cwnd_) is the maximum amount of segments that the network can handle simultaneously.
It is used for __congestion control__.

A __sequence number__ (_SN_) is a number attributed for each TCP segment, to order them.
It increases by one at each sent segment.
To avoid identical starting sequence number, it's designed to start with a number depending on the clock time, the source and destination IP address and a random number, for security reasons.

The __Round Trip Time__ (_RTT_) is the time between the sending and receiving a segment.
This is used to know if a segment is _lost_ or _on-flight_.

TCP is an acknowledged protocol.
An __ACK__ (_ACKnowledgment_) acknowledges a received segment.
An ACK can contains some data.
This is called __piggybacking__.

# ACKed or not ACKed ?

We will talk a bit more about ACK in the future of this lesson but one needs to know that TCP only acknowledges the last continuous sequence number.

> __Example__
> 
> A TCP receiver receives the segments numbered 1, 2, 3, 5, 6 and 9.\
> Can you guess which sequence number it will ACK ?
>
> The receiver will ACK the segment numbered __3__.\
> Since it didn't received the segment numbered 4, it won't acknowledge the segments with a higher number.

The basic __transmission principle__ states that a segment can only be transmitted if the sequence number is lower than the highest ACKed segment plus the minimum between the rwnd and the cwnd.
In more mathematical terms :

> Highest_SN_transmittable = Highest_SN_ACKed + min(rwnd, cwnd)

# From RTT to SRTT 🚀

The __SRTT__ (Smoothed RTT) is the estimation of the RTT.
This SRTT is estimated base on the previous estimated SRTT and the previous measured RTT and a factor _a_ :

`SRTT[k] = a * SRTT[k-1] + (1-a) * RTT[k-1]`

The factor _a_ is static and should be chosen regarding the channel stability :

- Unstable channel => big _a_
- Stable channel => small _a_

It's almost impossible to estimate it correctly.
Hence, two cases might occur :

- The SRTT is underestimated (SRTT < RTT), leading to unwanted retransmissions.
- The SRTT is overestimated (SRTT > RTT), leading to useless waiting times.

Overall, it's best to overestimate the SRTT to avoid network congestions.
