<template lang=md>

# Frequently Asked Questions

## What is DNSCrypt?

<a href="https://dnscrypt.info/">DNSCrypt</a> is a protocol that authenticates communications between a DNS client and a DNS resolver. It prevents DNS spoofing. It uses cryptographic signatures to verify that responses originate from the chosen DNS resolver and haven't been tampered with.

It is an open specification, with free and open source reference implementations, and it is not affiliated with any company or organization.

## What is DNS?

DNS is a globally distributed database that maps names to values. It is constantly used by all applications communicating over the Internet and private networks.

It typically translates names such as `www.google.com` into IP addresses such as `216.58.199.36`, so that devices can determine the path to follow in order to communicate with each other.

So what is the problem?

Originally, the DNS protocol didn't include any encryption nor authentication mechanisms; any device between a client and a DNS server could monitor and interfere with DNS traffic, without this being detected by the client.

Attackers on a local network can abuse this to conduct trivial attacks. More recently, malicious DNS servers have been massively used by malware.

The DNS security extensions (DNSSEC) were designed to partially mitigate this design flaw, by adding digital signatures to DNS responses.

<v-container text-xs-center>
  <img class=fill-width alt="DNS" src="../assets/DNS.png"/>
</v-container>

## What is DNSSEC

DNSSEC is a DNS Extension that allows a client to validate the DNS response on supported domains and TLDs.

Resolvers check the digital signature of DNS responses to verify that the data match what the zone owner initially configured.

Unfortunately, [DNSSEC has received limited adoption](https://labs.ripe.net/Members/rene_bakker/bang-for-the-buck-the-adoption-of-dnssec-and-return-on-investment), remains incompatible with some devices and software pieces, and doesn't prevent the traffic from being monitored.

<v-container text-xs-center>
  <img class=fill-width alt="DNSSEC" src="../assets/DNS-DNSSEC.png"/>
</v-container>

## Why use DNSCrypt?

* <v-icon color=green>thumb_up</v-icon> Encrypts and authenticates the DNS traffic
* <v-icon color=green>thumb_up</v-icon> Specifically designed for DNS
* <v-icon color=green>thumb_up</v-icon> Has been battle tested since 2011
* <v-icon color=green>thumb_up</v-icon> A good amount of servers support the protocol
* <v-icon color=green>thumb_up</v-icon> Includes mitigations against DNS amplification attacks
* <v-icon color=green>thumb_up</v-icon> Can use UDP and TCP for transport
* <v-icon color=green>thumb_up</v-icon> Inherently supports reordering, parallelism and priorities
* <v-icon color=green>thumb_up</v-icon> Keeps a minimal number of states server-side
* <v-icon color=green>thumb_up</v-icon> Very simple to implement; requires only two standard cryptographic constructions
* <v-icon color=green>thumb_up</v-icon> Doesn't require a TLS stack, which vastly reduces the attack surface
* <v-icon color=green>thumb_up</v-icon> Doesn't have any insecure parameters
* <v-icon color=green>thumb_up</v-icon> Doesn't rely on X509 certificates and Certificate Authorities
* <v-icon color=green>thumb_up</v-icon> Cannot be MITM'd by standard tools
* <v-icon color=green>thumb_up</v-icon> Enforces certificate signatures
* <v-icon color=green>thumb_up</v-icon> Has a complete specification since 2013
* <v-icon color=green>thumb_up</v-icon> Regular DNS and DNSCrypt can share the same port (although port 443 is recommended due to routers frequently hijacking port 53)
* <v-icon color=green>thumb_up</v-icon> DNSCrypt and DoH can also be served simultaneously on the same port
* <v-icon color=green>thumb_up</v-icon> Can hide client IP addresses from servers (Anonymized DNSCrypt)
* <v-icon color=green>thumb_up</v-icon> A prototype using post-quantum cryptography is available
* <v-icon color=red>thumb_down</v-icon> The <a href="https://datatracker.ietf.org/doc/draft-denis-dprive-dnscrypt/">RFC</a> is still a work in progress

<v-container text-xs-center>
  <img class=fill-width alt="DNSCrypt with DNSSEC" src="../assets/DNSCrypt-DNSSEC.png"/>
</v-container>

## How do I get started with using DNSCrypt?

See <router-link to="/implementations">Client Implementations</router-link>

## How do I get started with running my own DNSCrypt server?

See <router-link to="/implementations#server-implementations">Server Implementations</router-link>

## Other protocols

### [DNS over SSH]

* <v-icon color=green>thumb_up</v-icon> Full encryption of the DNS protocol
* <v-icon color=green>thumb_up</v-icon> Can be deployed on any system already running an SSH server
* <v-icon color=green>thumb_up</v-icon> Battle-tested, widely implemented transport protocol
* <v-icon color=green>thumb_up</v-icon> Doesn't depend on root CAs
* <v-icon color=green>thumb_up</v-icon> Can use DNSSEC or private CAs for public key verification
* <v-icon color=green>thumb_up</v-icon> Supports multiple authentication mechanisms
* <v-icon color=green>thumb_up</v-icon> Doesn't require a TLS stack; modern implementations do not even require OpenSSL any more
* <v-icon color=red>thumb_down</v-icon> Very tricky to configure securely
* <v-icon color=red>thumb_down</v-icon> Requires TCP
* <v-icon color=red>thumb_down</v-icon> Current implementations don't scale well server-side
* <v-icon color=red>thumb_down</v-icon> UDP queries [require TCP encapsulation](http://zarb.org/~gc/html/udp-in-ssh-tunneling.html)
* <v-icon color=red>thumb_down</v-icon> The SFTP protocol supports reordering and parallelism, but common DNS-over-SSH tunelling cannot use this

### DNS over TLS ([RFC7858](https://tools.ietf.org/html/rfc7858))

* <v-icon color=green>thumb_up</v-icon> Full encryption of the DNS protocol
* <v-icon color=green>thumb_up</v-icon> Has a low, but increasing number of servers in deployment
* <v-icon color=green>thumb_up</v-icon> Partially specified as a RFC
* <v-icon color=green>thumb_up</v-icon> [Many implementations completed at various stages](https://dnsprivacy.org/wiki/display/DP/DNS+Privacy+Implementation+Status)
* <v-icon color=red>thumb_down</v-icon> Provides more information than regular DNS to resolver operators in order to fingerprint clients, and this has (intentionally?) never been addressed in the specification
* <v-icon color=red>thumb_down</v-icon> Uses a dedicated port (853) likely to be blocked or monitored in situations where DNS encryption is useful
* <v-icon color=red>thumb_down</v-icon> Initial connection is slow due to the long handshake (until TLS 1.3 is deployed, which can take time due to [middleboxes](https://chefkochblog.wordpress.com/2017/12/20/tls-1-3-gets-blocked-by-cisco-avast-nsa/))
* <v-icon color=red>thumb_down</v-icon> Not well understood even by its proponents. It is a truck, as it is heavy and slow to load, but most if not all implementations perform a full round trip for every packet (even the excellent `miekg/dns` library as used by Tenta).
* <v-icon color=red>thumb_down</v-icon> Padding rules haven't been specified besides a draft that doesn't have any implementations, and a last-minute hack that requires altering DNS record sets before wrapping them
* <v-icon color=red>thumb_down</v-icon> Requires a full TLS stack, introducing a large attack surface
* <v-icon color=red>thumb_down</v-icon> Difficult to implement securely. Validating TLS certificates in non-browser software is [the most dangerous code in the world](https://crypto.stanford.edu/~dabo/pubs/abstracts/ssl-client-bugs.html)
* <v-icon color=red>thumb_down</v-icon> Readily compatible with industry-standard TLS interception/monitoring devices. Having people [install additional root certificates](https://sk.tl/3n7mJ9K4) is easier than custom software. Vendors are always ready to [passively extract information from TLS 1.3 sessions](https://sk.tl/7xmjiWwx).
* <v-icon color=red>thumb_down</v-icon> Requires TCP
* <v-icon color=red>thumb_down</v-icon> Requires sessions tracking on the server
* <v-icon color=red>thumb_down</v-icon> TLS is a generic transport mechanism. It doesn't support reordering and parallelism and doesn't include any ways to manage priorities. New mechanisms need to be invented and implemented to do so.
* <v-icon color=red>thumb_down</v-icon> Key management can be surprisingly hard especially if public key pinning is used by clients
* <v-icon color=red>thumb_down</v-icon> Allows insecure algorithms and parameters
* <v-icon color=red>thumb_down</v-icon> Will be difficult to improve without introducing more hacks. Unlikely to benefit from any improvements besides new TLS versions or homegrown reinventions.
* <v-icon color=red>thumb_down</v-icon> Questionable practical benefits over DoH

### [DNS over HTTPS (DoH)](https://www.rfc-editor.org/rfc/rfc8484.txt)

* <v-icon color=green>thumb_up</v-icon> Full encryption of the DNS protocol
* <v-icon color=green>thumb_up</v-icon> New implementations are being developed
* <v-icon color=green>thumb_up</v-icon> The minimum version of HTTP used by DoH should be HTTP/2
* <v-icon color=green>thumb_up</v-icon> Uses standard HTTP/2 or HTTP/3, on the standard port (443)
* <v-icon color=green>thumb_up</v-icon> Will automatically benefit from improvements made to HTTP
* <v-icon color=green>thumb_up</v-icon> Less likely to be blocked than other options
* <v-icon color=green>thumb_up</v-icon> Can be trivially deployed on any web server, and run along existing websites; DNS response are served like simple web pages
* <v-icon color=green>thumb_up</v-icon> Can share the same infrastructure as existing websites, and share the same certificates
* <v-icon color=green>thumb_up</v-icon> Simple to implement over any existing web stack
* <v-icon color=green>thumb_up</v-icon> Immediately compatible with caching proxies and CDNs
* <v-icon color=green>thumb_up</v-icon> Allows web browsers to perform DNS queries using Javascript
* <v-icon color=green>thumb_up</v-icon> Supports reordering, parallelism and priorities, thanks to HTTP/2
* <v-icon color=green>thumb_up</v-icon> Can leverage existing padding mechanisms (HTTP/2 frames padding)
* <v-icon color=green>thumb_up</v-icon> Implemented by Google DNS
* <v-icon color=green>thumb_up</v-icon> Supported by CuRL: will soon be available in all programming languages with bindings for libcurl
* <v-icon color=green>thumb_up</v-icon> Available in [Firefox](https://bugzilla.mozilla.org/show_bug.cgi?id=1434852)
* <v-icon color=green>thumb_up</v-icon> Specified as a RFC
* <v-icon color=green>thumb_up</v-icon> Can be accessed via relays (ODoH, DooH), even though this leaks more information that expected
* <v-icon color=red>thumb_down</v-icon> Provides more information than regular DNS to resolver operators in order to fingerprint clients, but this is being addressed in the specification
* <v-icon color=red>thumb_down</v-icon> Requires a full TLS stack and a web server
* <v-icon color=red>thumb_down</v-icon> Interception/monitoring tools are readily available
* <v-icon color=red>thumb_down</v-icon> Key management can be surprisingly hard especially if public key pinning is used by clients
* <v-icon color=red>thumb_down</v-icon> Allows insecure algorithms and parameters
* <v-icon color=red>thumb_down</v-icon> Requires TCP
* <v-icon color=red>thumb_down</v-icon> Rarely deployed in a way that would prevent it to be trivially blocked without ECH

### DNS-over-DTLS ([RFC8094](https://tools.ietf.org/html/rfc8094))

* <v-icon color=red>thumb_down</v-icon> No known implementations
* <v-icon color=red>thumb_down</v-icon> Many security vulnerabilities in OpenSSL due to DTLS

### [DNS over QUIC] ([RFC9250](https://tools.ietf.org/html/rfc9250))

* <v-icon color=green>thumb_up</v-icon> Full encryption of the DNS protocol
* <v-icon color=red>thumb_down</v-icon> Uses a dedicated port: 853, can't use port 53
* <v-icon color=red>thumb_down</v-icon> Client devices and IP addresses can be linked
* <v-icon color=red>thumb_down</v-icon> No clear advantage over HTTP/3

## Practical considerations

All the solutions above offer the same practical security level. Compatibility with existing tools and infrastructure is what makes an actual difference.

</template>

<script>
export default {
  head() {
    return {
      title: "DNSCrypt FAQ - DNSCrypt vs DoH vs DoT Comparison",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Frequently asked questions about DNSCrypt, DNS-over-HTTPS (DoH), and DNS-over-TLS (DoT). Compare secure DNS protocols and understand DNS encryption benefits.",
        },
        {
          hid: "og:title",
          property: "og:title",
          content: "DNSCrypt FAQ - DNSCrypt vs DoH vs DoT Comparison | DNSCrypt",
        },
        {
          hid: "og:description",
          property: "og:description",
          content: "Frequently asked questions about DNSCrypt, DNS-over-HTTPS (DoH), and DNS-over-TLS (DoT). Compare secure DNS protocols and understand DNS encryption benefits.",
        },
        {
          hid: "og:url",
          property: "og:url",
          content: "https://dnscrypt.info/faq",
        },
        {
          hid: "twitter:title",
          name: "twitter:title",
          content: "DNSCrypt FAQ - DNSCrypt vs DoH vs DoT Comparison | DNSCrypt",
        },
        {
          hid: "twitter:description",
          name: "twitter:description",
          content: "Frequently asked questions about DNSCrypt, DNS-over-HTTPS (DoH), and DNS-over-TLS (DoT). Compare secure DNS protocols.",
        },
      ],
      link: [
        {
          rel: "canonical",
          href: "https://dnscrypt.info/faq",
        },
      ],
    };
  },
};
</script>
