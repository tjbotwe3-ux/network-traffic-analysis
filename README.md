# Network Traffic Analysis with Wireshark

For my first cybersecurity portfolio project, I used Wireshark on a Mac to inspect a DNS lookup and follow a separate TCP connection into the start of TLS negotiation. The project shows how to match requests to responses, identify the packets that open a connection, and interpret the metadata visible around encrypted traffic.

| Protocol | What it does | What I observed |
| --- | --- | --- |
| DNS | Looks up information about a domain name, including IP addresses. | The lookup for `chat.openai.com` returned a Cloudflare hostname and two IPv4 addresses in 4.161 ms. |
| TCP | Establishes a connection so two endpoints can exchange data reliably. | Three packets opened a connection to `17.248.228.12:443` in 9.568 ms from the capture's perspective. |
| TLS | Negotiates how to protect data exchanged over the connection. | The client named `p143-contacts.icloud.com` in its Client Hello, and the server replied with a Server Hello. |

## How I inspected the traffic

1. I used Wireshark's `dns` display filter to find the DNS query and response. I matched the transaction IDs and expanded the answer records.
2. I used `tcp.flags.syn == 1` to find connection attempts, then filtered by the remote IP and client port to follow both directions of one connection. The SYN filter alone hides the final ACK.
3. I kept that connection filter active and examined the Client Hello and Server Hello that followed the TCP handshake.

See the [packet-by-packet findings](analysis/findings.md) for the filters, timing calculations, and cropped screenshots.

## Publishing the evidence

The screenshots are cropped to protect local network details, and the raw packet capture is kept private.
