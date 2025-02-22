# Blocky Setup Guide

This will guide you through the basic setup of Blocky which is the preferred DNS solution for TrueCharts. This guide will cover basic setup options which will get you up and running and is not all inclusive.  Setting up your systems to point to Blocky is out of scope of the document.

## Upstream DNS

Blocky has the following DNS entries configured by default.  They can be overridden to your personal preferences or left as default.
  - 1.1.1.1
  - 1.0.0.1
  - 8.8.8.8
  - 8.8.4.4
  - 9.9.9.9
  - 149.112.112.112
  - 208.67.222.222
  - 208.67.220.220
  - 8.26.56.26
  - 8.20.247.20
  - 185.228.168.9
  - 185.228.169.9
  - 76.76.19.19
  - 76.223.122.150
  - 76.76.2.0
  - 76.76.10.0

Blocky supports 3 methods for upstream DNS.  You can choose multiple options, but I'm not sure if the order matters.

- UDP - Basic DNS
- DoT - DNS over TLS
- DoH - DNS over HTTPS

While UDP provides no security for DNS both DoT and DoH will encrypt DNS request. DoH has the added benefit of privacy since DNS traffic will appear as HTTPS traffic.  

### UDP DNS Setup

- Google DNS: `8.8.8.8` `8.8.4.4`
- Cloudflare DNS: `1.1.1.1` `1.0.0.1`

![blocky-udp-upstream-google](./img/blocky-udp-upstream-google.png)

### DoT DNS Setup

- Google DNS ([Bootstrap DNS Required](#bootstrap-dns)): `tcp-tls:dns.google:853`
- Cloudflare DNS: `tcp-tls:1.1.1.1:853` `tcp-tls:1.0.0.1:853`

![blocky-dot-upstream-google](./img/blocky-dot-upstream-google.png)

### DoH Upstream

- Google DNS ([Bootstrap DNS Required](#bootstrap-dns)): `https://dns.google/dns-query`
- Cloudflare DNS: `https://1.1.1.1/dns-query` `https://1.0.0.1/dns-query`

![blocky-doh-upstream-google](./img/blocky-doh-upstream-google.png)

## Bootstrap DNS

If you entered a non-IP address (meaning you used a domain name) for DoT or DoH, then you need to ensure that a bootstrap DNS provider
is configured to resolve the DoT or DoH address. This provider can be any UDP upstream DNS.
In the below example I am using Google DNS.  I don't know if you need a bootstrap DNS if you have a UDP server listed above.

![blocky-bootstrap-google](./img/blocky-bootstrap-google.png)

## DNS Blacklists

DNS Blacklists are used to prevent DNS resolution of advertisement, malware, trackers
and adult sites domains. This is completed with public maintained blocklists.
A good source for these is [firebog.net](https://firebog.net).

:::warning Warning

While publicly maintained blocklists usually do a good job of allowing legitimate traffic they
can sometimes be too broad and catch traffic that you wish to allow. You may need to disable
certain blocklists if you find legitimate traffic being blocked.

:::

1. Pick a Group Name for your blocklists.
2. Add List entries for each blocklist by URL.
   ![blocky-blacklist](./img/blocky-blacklist.png)
3. Add a Clients Group Block and set Client Group Name to `default`
4. Under Groups Entry enter the Group name you used above.
   ![blocky-blacklist-group](./img/blocky-blacklist-group.png)

## Networking
If you want to use Blocky on your local network to take advantage of the filtering above, or the k8s-gateway below, you 
need to ensure your loadbalancer IP address is set and won't change.

You need to configure the systems on your network to use the loadbalancer IP address configured above.  This will either be done per system
or by changing the DHCP settings on your router.  

## k8s-Gateway Configuration

k8s-Gateway will automatically provide split DNS for your local domain. This will allow
you to resolve all ingress configured subdomains locally. All that is required for setup
is to add your root domain in the Domain name block.

![blocky-k8s-gateway](./img/blocky-k8s-gateway.png)

## Prometheus/Grafana

TBD
