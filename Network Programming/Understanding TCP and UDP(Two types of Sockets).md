- Essentially there are Two ways of transporting DATA in transport layer One is thorugh streams and one is through packets.
- Think Logically When you want to transfer Data From one machine to another what are diffulties you may face
   - You may be interrupted while sending
   - You may send the data out of order which may be incomprehensible to other guy.
- This is Exactly What TCP adress by sending the data in STreams which means Once you two clients are in a TCP connection they follow some of theeese rules
     - They will maintain a persistant connection..
     - They will Send the data in streams which means they will be in order 
     - There will be no loss of data
     - Retry when data is lost
All right, already. What are the two types? One is “Stream Sockets”; the other is “Datagram Sockets”, which may hereafter be referred to as “`SOCK_STREAM`” and “`SOCK_DGRAM`”, respectively. Datagram sockets are sometimes called “connectionless sockets”. (Though they can be `connect()`’d if you really want. See [`connect()`](https://beej.us/guide/bgnet/html//index.html#connect), below.)

Stream sockets are reliable two-way connected communication streams. If you output two items into the socket in the order “1, 2”, they will arrive in the order “1, 2” at the opposite end. They will also be error-free. I’m so certain, in fact, they will be error-free, that I’m just going to put my fingers in my ears and chant _la la la la_ if anyone tries to claim otherwise.

What uses stream sockets? Well, you may have heard of the `telnet` or `ssh` applications, yes? They use stream sockets. All the characters you type need to arrive in the same order you type them, right? Also, web browsers use the Hypertext Transfer Protocol (HTTP) which uses stream sockets to get pages. Indeed, if you telnet to a web site on port 80, and type “`GET / HTTP/1.0`” and hit RETURN twice, it’ll dump the HTML back at you!

> If you don’t have `telnet` installed and don’t want to install it, or your `telnet` is being picky about connecting to clients, the guide comes with a `telnet`-like program called [`telnot`](https://beej.us/guide/bgnet/source/examples/telnot.c)[7](https://beej.us/guide/bgnet/html//index.html#fn7). This should work well for all the needs of the guide. (Note that telnet is actually a [spec’d networking protocol](https://tools.ietf.org/html/rfc854)[8](https://beej.us/guide/bgnet/html//index.html#fn8), and `telnot` doesn’t implement this protocol at all.)

How do stream sockets achieve this high level of data transmission quality? They use a protocol called “The Transmission Control Protocol”, otherwise known as “TCP” (see [RFC 793](https://tools.ietf.org/html/rfc793)[9](https://beej.us/guide/bgnet/html//index.html#fn9) for extremely detailed info on TCP). TCP makes sure your data arrives sequentially and error-free. You may have heard “TCP” before as the better half of “TCP/IP” where “IP” stands for “Internet Protocol” (see [RFC 791](https://tools.ietf.org/html/rfc791)[10](https://beej.us/guide/bgnet/html//index.html#fn10)). IP deals primarily with Internet routing and is not generally responsible for data integrity.

Cool. What about Datagram sockets? Why are they called connectionless? What is the deal, here, anyway? Why are they unreliable? Well, here are some facts: if you send a datagram, it may arrive. It may arrive out of order. If it arrives, the data within the packet will be error-free.

Datagram sockets also use IP for routing, but they don’t use TCP; they use the “User Datagram Protocol”, or “UDP” (see [RFC 768](https://tools.ietf.org/html/rfc768)[11](https://beej.us/guide/bgnet/html//index.html#fn11)).

Why are they connectionless? Well, basically, it’s because you don’t have to maintain an open connection as you do with stream sockets. You just build a packet, slap an IP header on it with destination information, and send it out. No connection needed. They are generally used either when a TCP stack is unavailable or when a few dropped packets here and there don’t mean the end of the Universe. Sample applications: `tftp` (trivial file transfer protocol, a little brother to FTP), `dhcpcd` (a DHCP client), multiplayer games, streaming audio, video conferencing, etc.

“Wait a minute! `tftp` and `dhcpcd` are used to transfer binary applications from one host to another! Data can’t be lost if you expect the application to work when it arrives! What kind of dark magic is this?”

Well, my human friend, `tftp` and similar programs have their own protocol on top of UDP. For example, the tftp protocol says that for each packet that gets sent, the recipient has to send back a packet that says, “I got it!” (an “ACK” packet). If the sender of the original packet gets no reply in, say, five seconds, he’ll re-transmit the packet until he finally gets an ACK. This acknowledgment procedure is very important when implementing reliable `SOCK_DGRAM` applications.

For unreliable applications like games, audio, or video, you just ignore the dropped packets, or perhaps try to cleverly compensate for them. (Quake players will know the manifestation this effect by the technical term: _accursed lag_. The word “accursed”, in this case, represents any extremely profane utterance.)

Why would you use an unreliable underlying protocol? Two reasons: speed and speed. It’s way faster to fire-and-forget than it is to keep track of what has arrived safely and make sure it’s in order and all that. If you’re sending chat messages, TCP is great; if you’re sending 40 positional updates per second of the players in the world, maybe it doesn’t matter so much if one or two get dropped, and UDP is a good choice.