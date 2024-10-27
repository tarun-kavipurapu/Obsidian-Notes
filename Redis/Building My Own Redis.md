

There are 2 types of channels: packet-based and byte-stream-based. Each offers a different set of capabilities to applications.

UDP is packet-based. A packet is a message of a certain size from the application’s PoV. But Redis is TCP-based, so we’ll ignore UDP.

TCP provides a continuous stream of bytes. Unlike messages, **a byte stream has no boundaries within it**, which is a major difficulty in understanding TCP! That’s why it’s important to code your own Redis.


Sure, let’s break down the concepts of **IP packets**, **TCP**, and **UDP** and understand how they work within the network protocol layers.

## TCP-UDP-IPPackets
### 1. **IP (Internet Protocol) Packets**
- **IP** operates at the **Network Layer** (Layer 3 of the OSI model). It is responsible for sending data packets from one device (sender) to another (receiver) over a network.
- An **IP packet** is a small unit of data that has a specific structure, consisting of:
  - **Sender’s Address**: The IP address of the device sending the packet.
  - **Receiver’s Address**: The IP address of the device that should receive the packet.
  - **Message Data**: The actual data being sent.
- **IP packets** are independent and have no built-in mechanism for ensuring delivery or maintaining order. They can be lost, duplicated, or arrive out of order. 

IP handles the **routing** of these packets, making sure they take the right path across networks to reach their destination.

### 2. **TCP (Transmission Control Protocol)**
- **TCP** operates at the **Transport Layer** (Layer 4 of the OSI model) and is built on top of IP. It provides a **reliable, connection-oriented service** for applications.
- **TCP** solves several issues that occur with raw IP packets:
  - **Fragmentation**: If a message is too large for a single IP packet, TCP breaks it into smaller segments that fit within the IP packets.
  - **Reliability**: TCP ensures that all segments arrive correctly. If a segment is lost, TCP retransmits it.
  - **Order**: Even if packets arrive out of order, TCP reorders them before passing them to the application.
  - **Flow Control and Congestion Control**: TCP manages the rate of data transfer to avoid overwhelming the network and ensures a stable connection.
  
TCP essentially turns the unreliable and unordered IP packets into a **reliable byte stream**—an ordered sequence of bytes that applications can read without worrying about the underlying packet issues.

### 3. **UDP (User Datagram Protocol)**
- **UDP** also operates at the **Transport Layer** (Layer 4) and, like TCP, it is built on top of IP.
- Unlike TCP, **UDP** is a **connectionless** and **unreliable** protocol. It sends messages, called **datagrams**, without establishing a connection or ensuring that they arrive correctly.
  - **No Fragmentation Handling**: UDP does not split data into segments if it exceeds the maximum packet size.
  - **No Reliability**: If a datagram is lost or corrupted, UDP does not retransmit it.
  - **No Order**: Datagram order is not guaranteed; packets can arrive out of sequence.
- The main advantage of UDP is its **speed**. Since it doesn’t spend time establishing connections or managing reliability, it is faster and has lower latency compared to TCP.
- UDP is commonly used for applications where speed is crucial and minor losses are acceptable, such as video streaming, online gaming, or VoIP (Voice over IP).

### Summary
- **IP**: Delivers packets between devices but does not guarantee delivery or order.
- **TCP**: Adds reliability, ordering, and error-checking on top of IP, providing a byte stream for applications.
- **UDP**: Adds minimal overhead to IP, offering fast but unreliable and unordered packet delivery.

These protocols work together to provide different types of communication suited to various needs in networked systems.

### Socket Primitives 

Certainly! Let’s go through the concepts step by step to understand socket programming, focusing on socket primitives and how they are used in network communication, particularly on Linux systems.

### 1. **Sockets and OS Handles**
- A **socket** is an endpoint for communication between two machines over a network. In programming, a socket is represented by a **handle**, which is a unique identifier for that socket.
- On Linux, this handle is called a **file descriptor (fd)**. Even though it’s named like a file, it represents any input/output resource, not just files.
- The `socket()` syscall creates a socket and returns a file descriptor (fd). This fd is then used to perform operations like binding to an address or reading/writing data.

### 2. **Listening Socket vs. Connection Socket**
- **Listening Socket**: 
  - This is a socket that listens for incoming connections. It is associated with a specific IP address and port. 
  - When a server application starts, it creates a listening socket using the `socket()`, `bind()`, and `listen()` syscalls:
    - **`bind(fd, address)`**: This binds the socket to a specific address (IP + port).
    - **`listen(fd)`**: This marks the socket as a listening socket, meaning it will wait for incoming client connections.
  - The listening socket does not directly communicate with clients; it simply waits for connection requests.

- **Connection Socket**:
  - When a client connects, the server accepts the connection using the `accept()` syscall.
  - **`accept(fd)`**: This syscall accepts a client’s connection and creates a new socket specifically for that connection (a connection socket). This new socket has its own file descriptor, separate from the listening socket’s fd.
  - The server uses this connection socket to communicate directly with the client, reading from and writing to this fd.

### 3. **Socket Operations: Read and Write**
- **Sending and Receiving Data**: In network programming, sending data is often referred to as "writing," and receiving data is referred to as "reading."
- There are different syscalls available for reading and writing data in Linux:
  - **`read(fd, buffer, size)`** and **`write(fd, buffer, size)`**: The most basic syscalls for reading/writing data. They operate with a single, continuous buffer.
  - **`recv()`** and **`send()`**: Similar to `read()` and `write()`, but with extra flags for more control.
  - **`recvfrom()`** and **`sendto()`**: These are used with datagram-based protocols like UDP, allowing you to specify the remote address (sender/receiver).
  - There are other variations like **`readv`/`writev`** (for multiple buffers) and **`recvmsg`/`sendmsg`** (for more complex control). However, these advanced syscalls aren’t always necessary for basic socket programming.

### 4. **Client-Side Socket Operation: Connecting to a Server**
- A client creates a socket and connects to a server using:
  - **`connect(fd, address)`**: This syscall initiates a connection to a server’s address (IP + port). It sets up a TCP connection for the client to communicate with the server.
  - Once connected, the client can use `read()` and `write()` syscalls to exchange data with the server.
  - After the communication is done, the socket must be closed using `close(fd)` to release resources.

### 5. **Summary of Socket Primitives**
Here’s a concise list of the most common socket primitives used in socket programming:

- **For a Listening Socket (Server)**:
  - **`bind(fd, address)`**: Assigns the socket to a specific IP address and port.
  - **`listen(fd)`**: Marks the socket as a listening socket that waits for client connections.
  - **`accept(fd)`**: Accepts a new connection and returns a new socket fd for the client connection.
  - **`close(fd)`**: Closes the socket when you’re done with it.

- **For Using a Socket (Data Transmission)**:
  - **`read(fd, buffer, size)`**: Reads data from the socket into a buffer.
  - **`write(fd, buffer, size)`**: Writes data from a buffer into the socket.
  - **`close(fd)`**: Closes the socket once the communication is complete.

- **For Creating a Connection (Client)**:
  - **`connect(fd, address)`**: Initiates a TCP connection to the server.
  - **`close(fd)`**: Closes the socket when done.

### Putting It All Together
Here's a simple **pseudo code** summary of how a server and a client might use these socket primitives:

#### Server
```python
# Pseudo code for a server
fd = socket()            # Create a socket
bind(fd, address)        # Bind it to an address (IP + port)
listen(fd)               # Mark it as a listening socket
while True:
    conn_fd = accept(fd) # Accept a new client connection
    do_something_with(conn_fd) # Communicate with the client
    close(conn_fd)       # Close the connection socket after communication
```

#### Client
```python
# Pseudo code for a client
fd = socket()            # Create a socket
connect(fd, address)     # Connect to the server address
do_something_with(fd)    # Communicate with the server
close(fd)                # Close the socket after communication
```

### Summary
- **Listening sockets**: Set up on the server side to wait for connections (using `bind()` and `listen()`).
- **Connection sockets**: Created when a client connects (using `accept()` on the server, and `connect()` on the client).
- **Data transmission**: Managed through `read()` and `write()` for exchanging data between the client and server.

This structure allows applications to create, manage, and terminate network connections in a controlled manner using socket primitives!

![[Pasted image 20241010222645.png]]