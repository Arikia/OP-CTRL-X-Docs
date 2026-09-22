# Why OnionPress runs on Tor

If you're used to "normal" websites, running yours on Tor probably sounds
strange at first. This explains what's actually going on, and why it
gives you things a normal website can't.

## The problem with a normal website

A typical website works like this: you pay a hosting company to run a
server for you, you pay a registrar for a domain name, and the two are
linked together through the internet's phone book, DNS. That setup comes
with some quiet trade-offs:

- **Someone else is in the middle of every visit.** Your hosting company,
  your domain registrar, and whatever service resolves your DNS can all
  see and, if they choose to, block or shut down access to your site —
  because they're the ones actually running it or pointing to it.
- **Your address can be taken away.** Domain names expire, get
  suspended, get seized, or get bought out from under you. Your site's
  identity isn't really yours; it's leased.
- **You need someone else's infrastructure just to exist online.** A
  personal laptop at home usually can't be reached directly from the
  internet at all — home networks sit behind routers and ISPs that block
  incoming connections. That's exactly why hosting became a business in
  the first place.

## What Tor changes

Tor is best known as a privacy tool for *browsing* the web anonymously.
OnionPress uses a less well-known part of Tor: **onion services** — a way
to run a server that Tor itself makes reachable, without needing a
public IP address, a domain name, or a hosting company at all.

A few things make this different from normal hosting:

- **Your address is generated from a cryptographic key, not rented from
  anyone.** A `.onion` address (the long string ending in `.onion`) is
  mathematically derived from a private key that only you hold. Nobody
  assigns it to you, nobody can revoke it, and nobody can transfer it
  away from you — as long as you keep the key, the address is yours.
  There's no registrar in the loop to seize it or let it lapse.
- **It gets through firewalls and NAT.** Instead of waiting for the
  outside world to connect in, Tor lets your computer reach *out* and
  register itself on the Tor network. That's what lets OnionPress work
  from a laptop on a home Wi-Fi network, behind a school or office
  firewall, without any port forwarding or router configuration.
- **Traffic to your site is end-to-end encrypted automatically.**
  Normal websites need a certificate (the padlock icon, from a
  certificate authority) to encrypt traffic. Onion services get strong
  encryption built into the address itself — there's no certificate to
  buy, install, or let expire.
- **Nobody in the middle can see who's visiting, or block the visit.**
  Tor routes traffic through several relays so that no single party
  — not your ISP, not a network operator, not a government censor
  sitting on the wire — can see both who's visiting and what site
  they're visiting, or selectively block that one connection.

## What this actually gives you as a publisher

Put together, this is a genuinely different relationship to your own
content than typical hosting offers:

- **Nobody can de-platform you.** There's no hosting company's terms of
  service to violate, no app store to get pulled from, no account that
  can be suspended by a company. The server is your own computer; the
  address is your own key.
- **Your identity persists independent of any single machine.** If you
  move your OnionPress install to a new computer, you can carry the same
  key and keep the exact same `.onion` address — visitors and links to
  your site don't need to change.
- **It's resistant to censorship by design**, not as an added feature —
  the same properties that make it hard for anyone to take your address
  away also make it hard for a network to selectively block just your
  site.
- **You're not paying rent to exist online.** No hosting bill, no domain
  renewal fee. The cost is your own computer's electricity and internet
  connection.

## The trade-off, and how OnionPress covers for it

The obvious catch: a `.onion` address only answers while *your*
computer is on and connected. Close your laptop, and a normal onion
service just goes dark.

OnionPress covers this two ways:

1. **The Wayback Machine.** Your posts are automatically submitted to
   the Internet Archive as you publish, so a durable, publicly readable
   copy exists independent of whether your computer is currently
   running.
2. **OnionHeaven.** OnionPress instances register with a shared failover
   service. If your computer goes offline, OnionHeaven notices within a
   few minutes and temporarily serves visitors to your address a link to
   that archived copy, instead of a dead connection. The moment your
   computer comes back online, your real site takes over again
   automatically.

So the address doesn't need a company standing behind it to be
resilient — durability comes from the Wayback Machine backing up your
words, and from a small piece of shared infrastructure covering the gaps
while your own machine is offline. Ownership of the address, and control
over what you publish, stays with you the whole time.
