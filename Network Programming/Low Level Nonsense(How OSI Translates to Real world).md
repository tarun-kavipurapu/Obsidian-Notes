Since I just mentioned layering of protocols, it’s time to talk about how networks really work, and to show some examples of how `SOCK_DGRAM` packets are built. Practically, you can probably skip this section. It’s good background, however.

Data Encapsulation.
![[Pasted image 20241016225540.png]]
![[Pasted image 20241016225619.png]]


Hey, kids, it’s time to learn about _Data Encapsulation_! This is very very important. It’s so important that you might just learn about it if you take the networks course here at Chico State `;-)`. Basically, it says this: a packet is born, the packet is wrapped (“encapsulated”) in a header (and rarely a footer) by the first protocol (say, the TFTP protocol), then the whole thing (TFTP header included) is encapsulated again by the next protocol (say, UDP), then again by the next (IP), then again by the final protocol on the hardware (physical) layer (say, Ethernet).

When another computer receives the packet, the hardware strips the Ethernet header, the kernel strips the IP and UDP headers, the TFTP program strips the TFTP header, and it finally has the data.

Now I can finally talk about the infamous _Layered Network Model_ (aka “ISO/OSI”). This Network Model describes a system of network functionality that has many advantages over other models. For instance, you can write sockets programs that are exactly the same without caring how the data is physically transmitted (serial, thin Ethernet, AUI, whatever) because programs on lower levels deal with it for you. The actual network hardware and topology is transparent to the socket programmer.

Without any further ado, I’ll present the layers of the full-blown model. Remember this for network class exams:

- Application
- Presentation
- Session
- Transport
- Network
- Data Link
- Physical

The Physical Layer is the hardware (serial, Ethernet, etc.). The Application Layer is just about as far from the physical layer as you can imagine—it’s the place where users interact with the network.

Now, this model is so general you could probably use it as an automobile repair guide if you really wanted to. A layered model more consistent with Unix might be:

- Application Layer (_telnet, ftp,HTTP etc._)
- Host-to-Host Transport Layer (_TCP, UDP_)
- Internet Layer (_IP and routing_)
- Network Access Layer (_Ethernet, wi-fi, or whatever_)

At this point in time, you can probably see how these layers correspond to the encapsulation of the original data.

See how much work there is in building a simple packet? Jeez! And you have to type in the packet headers yourself using “`cat`”! Just kidding. All you have to do for stream sockets is `send()` the data out. All you have to do for datagram sockets is encapsulate the packet in the method of your choosing and `sendto()` it out. The kernel builds the Transport Layer and Internet Layer on for you and the hardware does the Network Access Layer. Ah, modern technology.

So ends our brief foray into network theory. Oh yes, I forgot to tell you everything I wanted to say about routing: nothing! That’s right, I’m not going to talk about it at all. The router strips the packet to the IP header, consults its routing table, _blah blah blah_. Check out the [IP RFC](https://tools.ietf.org/html/rfc791)[12](https://beej.us/guide/bgnet/html//index.html#fn12) if you really really care. If you never learn about it, well, you’ll live.