# SSL/TLS

Transport Layer Security (TLS), the successor of the now-deprecated Secure Sockets Layer (SSL), 
is a cryptographic protocol designed to provide communications security over a computer network. 
The protocol is widely used in applications such as email, instant messaging, and voice over IP, 
but its use in securing HTTPS remains the most publicly visible.

The TLS protocol aims primarily to provide cryptography, 
including privacy (confidentiality), integrity, and authenticity through the use of certificates,
between two or more communicating computer applications. It runs in the application layer and is itself composed of two layers: 
the TLS record and the TLS handshake protocols.

TLS is a proposed Internet Engineering Task Force (IETF) standard, 
first defined in 1999, and the current version is TLS 1.3, defined in August 2018. 
TLS builds on the earlier SSL specifications (1994, 1995, 1996) developed by Netscape Communications for adding the HTTPS protocol to their Navigator web browser.


Client-server applications use the TLS protocol to communicate across a network in a way designed to prevent eavesdropping and tampering.

Since applications can communicate either with or without TLS (or SSL), it is necessary for the client to request that the server sets up a TLS connection.
One of the main ways of achieving this is to use a different port number for TLS connections. 
For example, port 80 is typically used for unencrypted HTTP traffic while port 443 is the common port used for encrypted HTTPS traffic. 
Another mechanism is for the client to make a protocol-specific request to the server to switch the connection to TLS; for example, 
by making a STARTTLS request when using the mail and news protocols.
