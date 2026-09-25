---
title: OnionPress Upcoming Features
---

# OnionPress upcoming features

These features are being worked on but **aren't in OnionPress yet**. Each one is an open pull request on [brewsterkahle/onionpress](https://github.com/brewsterkahle/onionpress/pulls) that still needs review, and any of them could change or be dropped before release. For what OnionPress does today, see the [journalist's guide](journalists-guide.md).

Status as of September 25, 2026.

| Feature | Pull request | What it would let you do |
| --- | --- | --- |
| Publishing from censored countries | [#274](https://github.com/brewsterkahle/onionpress/pull/274) | Run OnionPress where Tor is blocked |
| Static sites | [#272](https://github.com/brewsterkahle/onionpress/pull/272), or [#275](https://github.com/brewsterkahle/onionpress/pull/275) + [#276](https://github.com/brewsterkahle/onionpress/pull/276) | Publish a site made with Hugo, Jekyll, moss or plain HTML |
| Member Vault | [#261](https://github.com/brewsterkahle/onionpress/pull/261) | Share files with approved people only |
| Verifiable builds | [#280](https://github.com/brewsterkahle/onionpress/pull/280) | Check that the app you download matches its source code |
| Reliable archiving and Following | [#281](https://github.com/brewsterkahle/onionpress/pull/281), [#282](https://github.com/brewsterkahle/onionpress/pull/282) | Fixes, not new features: see below |

## Publishing from censored countries

**Pull request:** [#274](https://github.com/brewsterkahle/onionpress/pull/274)

Some countries, including China, block ordinary connections to Tor, so OnionPress can't connect there today. This adds:

- **Tor bridges:** unlisted entry points to Tor that are harder for censors to block.
- **obfs4 and Snowflake:** ways of disguising Tor traffic as ordinary internet traffic.
- **Using a proxy you already have:** OnionPress can connect through a proxy you already use to get around censorship.

The author tested it from behind China's firewall. For journalists, this would make OnionPress usable for publishing from inside countries that censor Tor, not just for reaching readers there.

## Static sites

**Pull requests:** [#272](https://github.com/brewsterkahle/onionpress/pull/272) (draft), or [#275](https://github.com/brewsterkahle/onionpress/pull/275) and [#276](https://github.com/brewsterkahle/onionpress/pull/276)

Many reporters write with tools that produce plain web pages, like Hugo, Jekyll or the desktop app [moss](https://github.com/Symbiosis-Lab/moss-releases), instead of WordPress. These pull requests would let you publish those pages through OnionPress and still get an onion address, Wayback archiving and backups.

There are two competing designs, and the team hasn't chosen between them:

- **#272** replaces WordPress with a static site, chosen during setup.
- **#275 and #276** keep WordPress and serve the static site in front of it. Other publishing tools can upload a finished site to OnionPress, and visitors never see a half-published version.

A static site runs no database or server code, so there's less that can break or be attacked. That matters for high-risk publishing.

## Member Vault

**Pull request:** [#261](https://github.com/brewsterkahle/onionpress/pull/261)

An optional private section of your site. People apply to become members, you approve them, and approved members can see a private catalog and download its files. Everything else on your site stays public.

For journalists, this could be a way to share material with editors, collaborators or subscribers without putting it on a separate service. The pull request hasn't been tested yet.

## Verifiable builds

**Pull request:** [#280](https://github.com/brewsterkahle/onionpress/pull/280)

This lets anyone rebuild every OnionPress download on their own computer and check it against the published version. Every component it uses is locked to an exact version and checked. It also fixes two places where OnionPress downloaded tools without checking they were genuine.

This isn't something you'd see in the app. It matters for journalists at risk, and for their security advisers, because it makes it harder for anyone to slip a tampered version of OnionPress to them.

## Reliable archiving and Following

**Pull requests:** [#281](https://github.com/brewsterkahle/onionpress/pull/281), [#282](https://github.com/brewsterkahle/onionpress/pull/282)

These are bug fixes rather than new features, but they affect two things the journalist's guide relies on:

- **#281:** Wayback archiving gave up after 15 seconds, while connections over Tor often take 20 to 30 seconds. As a result, many posts were never archived, and no error was shown. The fix gives connections 45 seconds and logs failures. Until it's released, check that your important posts actually appear on the Wayback Machine.
- **#282:** the same problem made **Following** mark working sites as broken. It has the same fix.
