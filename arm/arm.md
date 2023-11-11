# ARM Notes

Author: **LBO** 🥖

Sources :

- [Course on Moodle](https://moodle.insa-lyon.fr/course/view.php?id=1993)
- [Mines Telecom course](https://lms.fun-mooc.fr/c4x/MinesTelecom/04001/asset/Support-Mooc-Semaine1A3__1_.pdf?fbclid=IwAR2x-M8uEiKbeXyirlpQQDEDMzVylUqdneQte3tpE1WyZVt-BP0nQjwl5vw)

Because mobile network are no fun without stories, let's start a new one.

## User Domain

Your name's Alicia and you possess a mobile phone. Here, your mobile phone will be referred as a *mobile station* (**MS**). In your phone, there's a **SIM** (*Subscriber Identify Module*) card.

> uSIM and eSIM stands for *universal SIM* and *electronic SIM* and are just newer version of the good old SIM.

There's also a fancy name for your phone number : the *mobile station international subscriber directory number*, a.k.a the **MSISDN** 🤯.

There are a whole lot of identifiers but we'll talk about that later.

Now, let's send a **SMS** (*Short Media Service*). If you want to achieve this, you should be close to an antenna. Here, the antennas will are referred as **BST** for *Base Station Transmitter*.

You're entering the **RAN**, the *Radio Access Network* !

## RAN : Radio Access Network

> But what's a *Base Station* ?

A base station is usually composed of two elements : the BST (the antenna) and a **BSC** (*Base Station Controller*). 
It will be the entrypoint for your SMS. The SMS will be received by the BST and treated by the BSC.

The roles of the BSC are :
- to control the base station,
- to concentrate the traffic (multiple BST can be connected to a single BSC),
- to define the radio resource allocation algorithm

For 3G, the BTS is a **NodeB** and the BSC is a **RNC**, *Radio Network Controller*.

For 4G / LTE, the BST and BSC are a unique device called a **eNodeB**.
For 5G, this device is called a **gNodeB**.

> Let's dive even deeper on eNodeB and gNodeB.
> 
> They're usually split in two devices : the **RRH**, *Remote radio Head* (the new name for the antenna) and the **BBU**, *Base Band Unit*
>
> The BBU can be placed at a certain distance from the RRH, so that it can find its place in a small datacenter or a PoP (Point of Presence) where it can easily be managed.
> But in fact, the BBU is usually placed next to its RRH, under the sun and the snow where it can die the fastest 😐.

Now, what happens ? Your SMS was correctly received by the BST that sent it to its BSC which makes your SMS go from the RAN to the **CN**, the *Core Network* !

## CN : Core Network

> What's the entrypoint of the CN ?

The BSC is connected to a **MSC**, a *Mobile Switching Center* which is basically a big switch that receives multiple packets from multiple BSC.
A most common name is *service gateway*. MSC was the device used in 2G.

After receiving a packet, it transmits it to the CN big boss : the **CNC**, the *Core Network Controller* 🤴.

> Note that the CNC only exists since the 3G. Before that, its functions were handled by the MSC and the gMSC, which we'll talk about later. 

The CNC is a very expensive device 💸💸. There are few CNC over the country and they process every single packet of the mobile network.

The CNC has a very important role and is connected to multiple databases and routers to do its job.

- User DB : Registers the operator subscribers.
- Facturation DB : Register the charges of each subscriber.
- Location DB : **HLR** (*Home Location Registry*) in 2/3G and **HSS** (*Home Subscriber Server*) in 4/5G.

> There's more to say about the *Location DB*.
>
> This database stores an approximate location of every users of the network. This location is used to send the packets to the correct base station.
> 
> Plus, the location DB is in fact hidden behind an **AuC** (Authentication Server).
> This server waits for an **IMSI** (*International Mobile Subscriber Identity*) and starts a challenge with the user. 
> The Auc sends a **RAND** (RANDom) which a random vector corresponding to the challenge. The MS (Mobile Station) answers with a **SRES** (*Signed RESponse*). 
> If the response is the correct response for the given challenge, the AuC returns its information from the Location DB.

The CNC can now send your SMS to the *out gateway* (the **gMSC** we talk previously), which is basically a router.

> For IPv4, the out gateway provides NAT functions.

This router is connected to a DHCP server. That's where an IP address can be given to your mobile station (your phone).
But for the simple SMS you sent, there's no need.

> **Wait**, should your SMS really get out by the out gateway ?
> Why the CNC sent your SMS to the out gateway ?
>
> It did because you're sending an SMS to someone who have a different operator. 
> Otherwise, It would have been sent back by the CNC to the service gateway and to the corresponding base station.

Ok, so where does the out gateway sends your precious SMS ?

It is sent to the operator of your recipient. To be more precise, it's sent to the out gateway of its operator, then to the CNC and then to a base station that will notify your recipient.
To say that in simple terms, it follows the same path but in reverse, in the recipient's operator network.

> The circle is complete ✔️.


# ACRONYMS 😨

Will you be brave enough to know them all ?

| Acronym | Full name |
| ------- | --------- |
| MS      | Mobile Station |
| SIM     | Subscriber Identify Module |
| uSIM    | Universal SIM |
| eSIM    | Electronic SIM |
| MSISDN  | Mobile Station International Subscriber Directory Number
| IMSI    | International Mobile Subscriber Identity |
| TMSI    | Temporary Mobile Subscriber Identity |
| IMEI    | International Mobile Equipment Identity |
| RNTI    | Radio Network Temporary Identifier |
| BS      | Base Station |
| BTS     | Base Transceiver Station |
| BSC     | Base Station Controller |
| RNC     | Radio Network Controller |
| RRH     | Remote Radio Head |
| BBU     | Base Band Unit |
| MSC     | Mobile Switching Center |
| GMSC    | Gateway MSC |
| GPRS    | General Packet Radio Service |
| SGCN    | Serving GPRS Support Node |
| GGSN    | Gateway GPRS Support Node |
| S-GW    | Serving Gateway |
| P-GW    | Packet Data Network Gateway |
| AMF     | Access and Mobility Management Function |
| UPF     | User Plane Function |
| MME     | Mobility Management Entity |
| HLR     | Home Location Register |
| HSS     | Home Subscriber Server |
| LTE     | Long-Term Evolution |
| SMS     | Short Media Service |
| MMS     | Multimedia Messaging Service |
| IMS     | IP Multimedia Subsystem |
| VoIP    | Voice over IP |
| RRC     | Radio Resource Control |
| RLC     | Radio Link Control |
| PDCP    | Packet Data Convergence Protocol |
| RAR     | Random Access Response |
| DTCH    | Dedicated Traffic Channel |
| DCCH    | Dedicated Control Channel |
| CQI     | Channel Quality Indicator |
| CC      | ??? |
| SM      | Session Management |
| SS      | ??? |
| (E)MM   | ??? |
| SMS     | Short Media Service |