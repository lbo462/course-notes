# Transmission Control Protocol

Author: **LBO** 🥖

Sources:
  - [Course on Moodle](https://moodle.insa-lyon.fr/course/view.php?id=5794)
  - [Wikipedia / TCP Congestion Control](https://en.wikipedia.org/wiki/TCP_congestion_control)

_Every newly defined terms are written in __bold__._
_If a term is not __bold__, either it was defined previously or it's too obvious to be defined._

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
To avoid identical starting sequence number (__ISN__, _Initial Sequence Number_), it's designed to start with a number depending on the clock time, the source and destination IP address and a random number, for security reasons.

> The ISN is sent in the SYN segment during the handshake.

The __Round Trip Time__ (_RTT_) is the time between the sending and receiving a segment.
This is used to know if a segment is _lost_ or __on-flight__ (somewhere in the channel during its transmission).

TCP is an acknowledged protocol.
An __ACK__ (_ACKnowledgment_) acknowledges a received segment.
An ACK can contains some data.
This is called __piggybacking__.

TCP defines the __flight size__ as the number of non-ACKed segments, supposing the lost or on-flight segments.
__This is no cwnd !__

# ACKed or not ACKed ?

We will talk a bit more about ACK in the future of this lesson but one needs to know that TCP only acknowledges the last continuous sequence number.

> __Example__
> 
> A TCP receiver receives the segments numbered 1, 2, 3, 5, 6 and 9.\
> Can you guess which sequence number it will ACK ?
>
> The receiver will ACK the segment numbered __3__.\
> Since it didn't received the segment numbered 4, it won't acknowledge the segments with a higher number.
> 
> Note for later : TCP will continue ACKing the SN 3 until it receives the SN 4.
> These are called __duplicated ACK__.
> Keep this in mind for the congestion control section !

The basic transmission principle states that a segment can only be transmitted if the sequence number is lower than the highest ACKed segment plus the minimum between the rwnd and the cwnd.
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

# Congestion control 🚗🚘💥🚙💢

Before controlling the congestion, TCP needs to know when congestion occurs !
Remember that TCP was invented before the wireless communications, in a time were channels were pretty stables.
Hence, it considers that a lost segment implies congestion.
That's rude, I agree I agree I agree 🐺

Now the question _"When do I know that a segment was lost ?"_ raises.
Not receiving a ACK doesn't imply that the segment was lost.
Maybe the ACK is lost, or maybe the SRTT was badly estimated.
But still, default TCP waits for a timer based on the RTT before it considers a segment lost.
This process is bettered by a particular TCP extension, named __fast retransmit__ that we'll talk about later on.

## Slow start, congestion avoidance and tutti quanti

When TCP initiate a connection, it starts its __slow start__ process.
This process consist in increasing the supposed cwnd exponentially until it reaches a threshold called __ssthresh__ (_slow start threshold_).

After the cwnd reaches this threshold, it continues incrementing the cwnd but one by one.
This moment is called __congestion avoidance__.

When a segment got lost, TCP adapts the ssthresh and resets the cwnd to start over the slow start.
At this moment :
`ssthresh = flight-size / 2` ; `cwnd = 1` and the slow start restarts.

## Fast retransmit

This is a TCP enhancement created to better the retransmission method for the lost segments.

Remember duplicated ACKs from the ACK section ?
Fast retransmit considers that receiving __three__ identical duplicated ACK implies that a segment was lost, since the receiver didn't received it.

> Considering a segment lost after a single duplicated ACK is rude.
> Maybe the channel disordered the segments, creating duplicate ACKs even when no segment is lost.
>
> __Example__
>
> A receiver receives the segments 1, 2, 3, 5, 4.\
> It will ACK 1, 2, 3, 3, 5, creating a duplicated ACK when no segments are lost.
>
> Note that this implies that TCP receiver have a buffer to store disordered segments.

If you're curious and smart or maybe if you already know about TCP enhancements, you might ask yourself something like 
_"Duplicated ACKs implies that I start over the slow start process ?_
_Isn't rude if I'm able to receive the ACKs ?_
_The network should not be that congested ..."_
And you would be right !

