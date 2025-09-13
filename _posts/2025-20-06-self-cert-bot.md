---
layout: post
title: Self Certification Bot
date: 2025-06-20 09:00:00
description: how I solved certificate generation in closed networks
categories: networking software
featured: true
---

> Every man at some point in his life decides either to farm or to run his own Root Certification Authority.

## Introduction

The _Let's Encrypt_ CertBot[^1] is surely an exceptionally useful tool, however it requires the host to be public reachable.
This means that it's not useable when dealing with private networks.

<!--
%As introduced in my [previous post]({% link _posts/2024-11-14-network-arch.md %}) about my networking infrastructure,
-->

As I'll describe in another post about my network infrastructure, I'm running various servers and services in a closed network environment.
This realm came at the price of manually handling TLS certificates, without the advantages of using a certification bot and an ACME CA.

So far I handled the certificate generation with a Python script, which is available in my homeLab scripts repository[^4].
However, such script required to generate the certificate on the same machine hosting my Root CA files, and then to transfer manually the certificate files to the right machine.
Too much exposure and overhead for a task that could be improved with a cool project.
And so the journey began.

## Some Background

I'm sure that if you're reading this post, you should have all the required knowledge to understand it.
Despite this, it's always useful to review some background theory.

HTTPS consists in the addition of an encryption layer to HTTP, which is a plain text protocol.
This translates in exchanging the HTTP protocol on a TLS (Transport Layer Security) session[^2].
Lastly, the TLS protocol requires an RSA key pair to initialize the session and derive a symmetric key.

Skipping why we need domains over ip addresses, the point is: how can we be sure of the server identity?
In other words, how can we assure that the TLS connection has been truly established with _example.com_?
Here's where certificates and the DNS join the party.
As we all know, a DNS (Domain Name System) bounds a domain name to an IP address and other kind of records.

On the other hand, cryptographically speaking in a broad sense, a certificate is a signed piece of information.
We can trust the content of a certificate as truth if we trust the entity that signed the certificate itself.
In this specific case, the certificate holds the public key held by the server and the website domain.
Hence, a certificate is bounding a domain to a public key, other than auxiliary information like the organization's data.

### Getting certified

The last section brings us to the question: how do I get a certificate for my domain?
Well, the issuer needs to verify that who's asking for a certificate truly is the owner of the certificate.
While many registrar provide their own wildcard certificates if you're holding your domain on their system, we are focusing on the other type of issuance, i.e. the challenge based one.
The other - cooler - way, is to ask to an ACME certification authority, which will deploy an ownership challenge[^3] to solve like the following ones:

- HTTP-01
- DNS-01
- TLS-ALPN-01

Once the challenge has been solved, the ACME CA will issue the certificate and that's done.

## The solution

> Yeah, well, I'm gonna go build my own ~~theme park~~ certification bot. With blackjack and hookers!

<div class='d-flex row'>
    <div class='col-sm d-flex justify-content-center'>
        <div class="tenor-gif-embed d-flex justify-content-start" data-postid="20104589" data-share-method="host" data-aspect-ratio="1.33333" data-width="50%"><a href="https://tenor.com/view/futurama-blackjack-and-hookers-bender-gif-20104589">Futurama Blackjack GIF</a>from <a href="https://tenor.com/search/futurama-gifs">Futurama GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>
    </div>
</div>

Theoretically, HTTPS shouldn't be required when packets are traveling on safe networks, like in my homelab setting.
However, modern browsers are - rightfully - complaining when you are visiting a plain-HTTP website or one serving an untrusted TLS certificate.
Hence, I decided to add TLS to all my services using a main nginx-gateway for encryption handling.

As introduced in the first section, my current setting involves a Root CA of mine (MostSerene Root), which certificates are installed on the clients' machines.
All the child certificates are eventually signed with the Root CA private key and hence accepted by the browsers.

Driven by the constant desire of automate tedious activities like these ones, the solution was to build a simple certification bot and certification challenge on my own.
Regarding the technical stack, since I'm forced to work with Python in my current job, and that snake makes me feel dumber the more I use it, I decided to work with C++ to keep me sharp.

Obviously, similarly to the Let's encrypt, the self certification bot must be able to reach and connect to the claimant machine.

## The protocol

For the sake of this project, we can define the domain ownership as control over the requested domain.
Namely, the claimant controls the server with ip corresponding to the DNS record value.

To do so, the standard approach is to use a challenge, i.e. a generated token that the certifier is expected to retrieve back when resolving the domain.

In my simple protocol, the client starts the communication by performing a TLS handshake with the certifier server.
When a secure channel has been established, the client communicates the desired domain name to the server, its X509 fields and its public key.
The certifier proceeds to generate the secret and send it to the claimant.

After the client has sent an acknowledge and communicated a port, the server will resolve the requested domain and establish a secondary connection on the given port and request the token.

If the peer on the secondary connection replies with the correct token, the certifier will proceed to generate and sign the child certificate, and eventually send it to the client.
Once received, the claimant will save it to the disk and close the connection.

In case the server cannot establish the secondary connection or in case of a challenge mismatch, the client will be informed that an error occurred and the connection will be closed.
In such case, a rate-limiting strategy will be enforced in future versions.

## Limitations

A main issue of my approach and implementation appears in case of a DNS spoofing attack.
However, since the project is meant to be used in closed environments, I considered it a secondary issue.
To address this, a common solution would be to use multiple DNSs or to use DNSSEC.

Other kind of limitation may be discovered with a penetration testing activity.

## Future work

Beyond improving the security of the project with rate-limiting, etc., I'm intrigued by the certificates usage outside of the traditional ways like HTTPS.
For instance, zero trust networks may use certificates to enforce user-based capabilities with distributed consistency.
In this sense, one area that I plan to explore in my next free time projects are wireguard protocol extensions, aimed to enforce certificate based authorization.

## Distribution

The entire codebase of the project is available on a repository[^5] hosted by GitHub.
Regarding packaging and distribution, the project will be shortly packaged and distributed as an APT package and a docker image.

[^1]: [Let's Encrypt certbot](https://certbot.eff.org/)
[^2]: [Cloudflare TLS handshake](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)
[^3]: [ACME Challenges](https://letsencrypt.org/docs/challenge-types/)
[^4]: [HomeLab script repo](https://github.com/Gotti27/homelab-scripts)
[^5]: [Self-cert-bot repository](https://github.com/Gotti27/self-cert-bot)
