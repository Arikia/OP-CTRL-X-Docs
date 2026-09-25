---
title: OnionPress for Journalists
---

# OnionPress for Journalists

OnionPress lets you publish from your own computer to a permanent `.onion` address that no host, registrar or platform controls, and it saves each post to the Internet Archive's Wayback Machine. For journalists, that means work that is hard to take down, an address that can't be faked, and a public, timestamped record of what you published. It also has real limits, covered in [Know the trade-offs](#know-the-trade-offs). Read that section before you use OnionPress for sensitive work.

This guide is for independent reporters and freelancers, newsrooms, and journalists working under pressure in any country. It covers what OnionPress does today (version 2.4.110). Features that are still in development are in [Upcoming features](upcoming-features.md). To install OnionPress and learn the basics, see the [getting-started guide](getting-started-guide/).

## What makes OnionPress different

Almost every way of publishing online puts a company in the middle: a web host, a domain registrar, or a platform like Substack or Medium. Each one can be pressured, sued, hacked or bought, or can simply change its rules. OnionPress removes those middlemen.

- **Nobody to send a takedown to.** Your site runs on your own computer. Its address comes from a key you hold, not from a registrar. There's no host or platform to receive a takedown notice, a subpoena for the server, or a terms-of-service complaint.
- **An address that proves it's you.** A `.onion` address is derived from your key. If a page loads at your address, it came from whoever holds that key. Impostors can copy your design, but they can't copy your address.
- **A timestamped public record.** OnionPress automatically submits each post to the Wayback Machine. That's an independent copy of what you published and when.
- **Readers are protected.** People who read your site over Tor are hidden from their internet provider and anyone watching the network. OnionPress has no analytics, ads or trackers.
- **No hosting bill.** OnionPress is free and open source. It works from home, hotel or office Wi-Fi without any router setup.

## What journalists can use it for

### Publishing under pressure

- **Investigations that might draw legal or political pressure.** No hosting company can be pressured into pulling your site, and no registrar can suspend your domain.
- **Keeping your address if your computer is lost or seized.** An OnionPress backup includes the key to your address. Restore it on a new computer and your site comes back at the same address, so links you've already shared keep working. Keep a backup somewhere other than your computer.
- **Publishing from restrictive networks.** Tor connects out from your computer, so OnionPress works behind hotel, campus and office firewalls without port forwarding.

### Proof and credibility

- **A verified identity.** Publish your onion address on your social profiles, business card and bylines. Readers can check that a post came from you by checking the address.
- **Evidence of what you published and when.** The Wayback Machine copy is kept by the Internet Archive, not by you. If someone claims you changed a story after publication, the archived copy shows what was there on the date it was captured.
- **Publishing primary sources.** Files you put in the `~/OnionPress/Creations` folder are published on a **My Creations** page, so you can post documents, datasets or audio alongside your reporting.

### Protecting your body of work

- **Your social media history, on your own site.** The **Social Archive** imports your posts from X/Twitter, Bluesky, Mastodon and Reddit into your OnionPress site. If a platform suspends you or shuts down, your posts are still on a site you control.
- **Reporting that outlives your computer.** Because every post is also on the Wayback Machine, it stays readable even if your computer is off for good.

### Research and newsrooms

- **Following sources without revealing yourself.** The **Following** feature reads other sites' feeds through Tor. The sites you follow don't see your IP address.
- **A small newsroom on one computer.** OnionPress uses WordPress Multisite, so each reporter can have their own section under one onion address, like `…onion/reporter-name`.

### Reaching readers in censored countries

- Readers who can't safely visit a news site on the regular web can read it over Tor. If you also connect a regular domain (see [Know the trade-offs](#know-the-trade-offs)), OnionPress tells Tor Browser users that an onion version exists, so they can switch to it.

## Things only OnionPress does

| What you can do | Where to find it |
| --- | --- |
| Publish at a permanent `.onion` address, with no host or domain | Happens automatically once OnionPress is running |
| Save each post to the Wayback Machine automatically | On by default |
| Keep readers from reaching a dead site while your computer is off | OnionHeaven, on by default. Read the trade-offs first |
| Move to a new computer and keep the same address | Onion menu → **Backup...** / **Restore...** |
| Claim a readable name that points to your onion address | Onionnames: your WordPress username becomes your onionname |
| Bring your social media posts onto your own site | Dashboard → **Social Archive** |
| Follow other sites through Tor | Dashboard → **OnionPress** → **Following** |
| Publish files from a folder on your Mac | `~/OnionPress/Creations` and the **My Creations** page |

## Know the trade-offs

OnionPress protects some things well and others not at all. Know the difference before you rely on it.

### Your identity is not hidden

Tor hides where your server is. It does not hide who you are. Your name, your writing style, the people you quote and the files you upload can all identify you.

- **Photos can reveal your location.** WordPress keeps the original file you upload, including any GPS location and camera details stored in it. Remove that metadata before you upload.
- **Your WordPress username is public and permanent.** OnionPress registers each username with OnionHome as an onionname. The name is claimed forever and can be looked up on the regular web at `onionpress.org/<name>`. If you publish under a pseudonym, choose a username that doesn't identify you before you create your site.

### Your computer is the server

Your drafts, published posts, database and the key to your onion address are all on your computer. Anyone who takes your computer, or gets into it, gets all of that. Use full-disk encryption (FileVault on a Mac) and a strong login password. Keep your backup and its password somewhere safe.

### Treat the Wayback Machine copy as permanent

Automatic archiving is what makes your record trustworthy, but it also means you can't reliably take something back after you publish it. Anything you publish, including mistakes and names you later wish you'd left out, may stay on the Wayback Machine. Check each post carefully before publishing.

### OnionHeaven holds a copy of your key

OnionHeaven is the shared service that answers for your site when your computer is off. It's on by default. To answer at your address, it needs the key to your address, so **OnionPress sends your site's private key to the OnionHeaven hub** and checks in about once a minute. That means:

- Whoever runs the OnionHeaven hub could publish at your address as if they were you.
- The hub gets a minute-by-minute record of when your computer is online.

For everyday use, that's a reasonable trade for keeping your site reachable. **For sensitive work, turn it off:** click the onion, choose **Settings...**, uncheck **Register with OnionHeaven (advanced)**, and save. Your site will then be unreachable while your computer is off. The Wayback Machine copies stay available directly on archive.org.

### Linking a regular domain reveals your IP address to Cloudflare

You can also make your site available at a regular domain like `example.com` through a free Cloudflare Tunnel. Cloudflare can then see your computer's IP address, and your domain has a registrar that can be pressured. For sensitive work, use only the onion address.

### It's not a way to receive tips

Comments on your site are public. OnionPress has no private way for sources to contact you. For confidential tips, use [SecureDrop](https://securedrop.org/) or Signal, and put those details on your site instead.

## Checklists

### Everyday reporting

- [ ] Save your WordPress password in a password manager. You need it for backups.
- [ ] Put your onion address on your profiles and bylines so readers can verify you.
- [ ] Leave Wayback archiving and OnionHeaven on so your site stays readable.
- [ ] Make a backup after important posts and keep a copy off your computer.

### Higher-risk work

- [ ] Use a computer that is used only for this, with FileVault turned on.
- [ ] Choose a WordPress username that doesn't identify you, before you set up your site. It becomes a permanent public onionname.
- [ ] Turn off **Register with OnionHeaven** in **Settings...**.
- [ ] Don't connect a regular domain or Cloudflare Tunnel.
- [ ] Remove location data and other metadata from every photo and document before uploading.
- [ ] Assume everything you publish is archived permanently.
- [ ] Keep your backup (it contains your address key) encrypted and stored separately from your computer.
- [ ] Take tips through SecureDrop or Signal, not through your site.

---

*Based on OnionPress 2.4.110, September 2026. The trade-offs above come from the OnionPress source code; the OnionPress team is reviewing them. Found something wrong? Tell us.*
