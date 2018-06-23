# Network

## Protocols

- Ethernet - Unit: frame.
- IP - Has IP addresses. Unit: packet/datagram?
  - [RFC 791 - INTERNET PROTOCOL](https://tools.ietf.org/html/rfc791)
- UDP - On top of IP. Has ports. Unit: datagram
  - [RFC 768 - User Datagram Protocol](https://tools.ietf.org/html/rfc768)
- TCP - On top of IP. Has ports. Unit: segment
  - [RFC 793 - TRANSMISSION CONTROL PROTOCOL](https://tools.ietf.org/html/rfc793)
  - [RFC 7414 - A Roadmap for Transmission Control Protocol (TCP)](https://tools.ietf.org/html/rfc7414)
- TLS - On top of TCP. Successor of SSL.
  - [RFC 5246 - The Transport Layer Security (TLS) Protocol Version 1.2](https://tools.ietf.org/html/rfc5246)
  - https://security.stackexchange.com/questions/20803/how-does-ssl-tls-work
- HTTP - On top of TCP.
- HTTPS - On top of TLS.
  - [RFC 2818 - The Secure HyperText Transfer Protocol](https://tools.ietf.org/html/rfc2818)
