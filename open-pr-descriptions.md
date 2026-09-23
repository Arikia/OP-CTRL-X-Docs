# OnionPress open pull requests in plain English

Sep 23, 2026 · @Arikia

## Overview

OnionPress has 10 open pull requests. Together they fix reliability problems, let people publish sites not made in WordPress, make the app work in censored countries, and make the software easier to verify and build.

They fall into six groups:

- **Slow Tor connections treated as failures** (#281, #282). The app gave up on Tor connections after 15 seconds, but real connections often take 20–30 seconds.
- **Launcher and security fixes** (#273, #279). These cover start/stop bugs, a redirect loophole in auto-login, and a case where OnionPress could take over someone else's Docker setup.
- **Static sites** (#272, #275, #276). These let people publish a site built with other tools (Hugo, Jekyll, plain HTML) instead of WordPress.
- **Censorship resistance** (#274). This lets OnionPress connect to Tor from behind national firewalls such as China's.
- **Build and supply chain** (#280). Everything OnionPress ships can be rebuilt on a developer's own machine from pinned, checked inputs.
- **Member-only sharing** (#261). This adds a private catalog that only approved members can see.

## What each PR does

| PR | Author | Opened | In one line |
| --- | --- | --- | --- |
| [#282](https://github.com/brewsterkahle/onionpress/pull/282) | Arikia | Sep 22 | Stops Following from wrongly flagging slow sites as broken |
| [#281](https://github.com/brewsterkahle/onionpress/pull/281) | Arikia | Sep 22 | Makes Wayback Machine archiving actually work over slow Tor links |
| [#280](https://github.com/brewsterkahle/onionpress/pull/280) | ivar | Sep 18 | Lets anyone rebuild every OnionPress download from pinned, checked inputs |
| [#279](https://github.com/brewsterkahle/onionpress/pull/279) | ivar | Sep 17 | Stops OnionPress from installing itself into someone else's Docker VM |
| [#276](https://github.com/brewsterkahle/onionpress/pull/276) | guoliu | Aug 24 | Adds a way for outside tools to upload a finished static site |
| [#275](https://github.com/brewsterkahle/onionpress/pull/275) | guoliu | Aug 24 | Serves a static site first, with WordPress behind it |
| [#274](https://github.com/brewsterkahle/onionpress/pull/274) | guoliu | Aug 24 | Makes OnionPress work behind national firewalls |
| [#273](https://github.com/brewsterkahle/onionpress/pull/273) | guoliu | Aug 24 | A bundle of bug fixes, including one security hole |
| [#272](https://github.com/brewsterkahle/onionpress/pull/272) (draft) | ivar | Aug 17 | Lets a user choose "static site" instead of WordPress at setup |
| [#261](https://github.com/brewsterkahle/onionpress/pull/261) | fractastical | Jun 11 | Adds a members-only vault for sharing files with approved people |

### Slow Tor connections (#281, #282)

A connection through Tor has to build a path through several relays first, and that often takes 20–30 seconds. OnionPress gave up after 15 seconds, so many connections failed before they had started.

**#281 — Wayback Machine archiving.** OnionPress saves each post to the Internet Archive automatically. Because of the 15-second limit, nearly every save failed, and the failures weren't reported, so the archiver looked healthy while saving nothing. The fix raises the limit to 45 seconds and logs failures. On a test install, all 10 posts plus the home page and feed were archived within minutes.

**#282 — Following.** The same 15-second limit applied to every outside web request WordPress makes through Tor, including checking the sites a user follows. A followed site that was up but slow got a warning triangle and was retried less and less often. The fix uses the same 45-second limit.

### Launcher and security fixes (#273, #279)

**#273 — Bug fixes.** This covers several separate problems:

- After a restart, the app could point WordPress at the wrong port.
- A successful start could still report failure.
- **Security:** the auto-login link could be tricked into sending a visitor to any outside website (an "open redirect"). This is now blocked, with tests.
- Visitors to a claimed onion name on the normal web were sent to a `.onion` address their browser usually can't open. They now get a working page.
- A stalled plugin download could freeze first-time setup for 5 minutes. It now gives up after 30 seconds and tries another route.
- Status now says "reachable", "unreachable" or "unknown" instead of just yes/no, so "we don't know yet" no longer looks like "down". Anything that reads the status file needs updating.
- The VM's default memory rises from 1 GB to 2 GB, because 1 GB ran out under normal use.

**#279 — Using the wrong Docker VM.** Some people also run Colima (the tool OnionPress uses for its Linux VM) for their own work. If OnionPress's own connection to its VM failed, the launcher quietly connected to the user's personal VM instead and installed WordPress, the database and Tor there. The fix removes that fallback, cleans up links left behind by it, waits up to 45 seconds for OnionPress's own VM, and shows a clear error if it never arrives.

### Static sites (#272, #275, #276)

Many people write sites with tools that output plain HTML files (Hugo, Jekyll, or the desktop app moss). These PRs let them publish those files through OnionPress and keep its onion address, archiving and backups. There are two separate designs:

**#272 — Static site as a setup option (draft).** At setup the user picks WordPress or a static site. If they pick static, a small web server replaces WordPress and `onionpress publish <folder>` puts a folder online. Archiving, backups, onion names and Following are all extended to work with it. Existing WordPress installs are unchanged. It has not yet been tested end to end on a real machine.

**#275 — Serve static pages first.** WordPress stays installed. If a static site has been published, its pages are served, and WordPress answers anything the static site doesn't cover, such as the admin screens. Nothing changes for users who never publish a static site.

**#276 — Upload from outside tools.** This adds a small, documented upload service that only works from the user's own computer. A tool uploads a finished site, then switches to it in one step, so visitors never see a half-published site. It also adds a command-line way to claim an onion name and checks that reject malicious uploads. moss is the first tool to use it.

\#275 and #276 are parts 3 and 4 of guoliu's series, which is tracked in [#277](https://github.com/brewsterkahle/onionpress/issues/277). #276 contains #274 and #275, so it should be merged after them.

### Censorship resistance (#274)

**#274 — Bridges and proxies.** Countries such as China block ordinary connections to Tor. This adds Tor bridges (unlisted entry points), obfs4 and Snowflake (which make Tor traffic look like ordinary traffic), and support for proxies the user already runs, for both Tor programs OnionPress uses (C Tor and Arti). The author tested it from behind China's firewall. It also stops the health check from calling Tor healthy when it's connected but not serving anything. Because it changes the Tor container image, that image has to be rebuilt, and several files that point to it have to be updated together.

### Build and supply chain (#280)

**#280 — Local, pinned builds.** Before this PR, some OnionPress parts could only be built by the project's CI servers, and a few files in the repo had no build instructions at all. This PR makes every download buildable on a developer's own machine. Every outside input is locked to an exact version and checked. It fixes a mix-up in v2.4.110, which shipped two different Tor versions in one install. It closes two supply-chain gaps: a tool downloaded without checking it, and a Tor signing key that wasn't verified. It also deletes an old `make build` script that could damage the app bundle, and adds `make doctor` and build guides. Because it changes what goes into the published container images, the author asks for maintainer review.

### Member-only sharing (#261)

**#261 — Member Vault.** An optional feature for sharing files with approved people only. Visitors apply to become members, the site owner approves them, and approved members can see a private catalog and download its files. Public posts stay public. Its test plan hasn't been run yet, and it has been open the longest (since June 11).

## How they fit together

The biggest open decision is which static-site design to adopt: #272 (static site *instead of* WordPress) or #275 + #276 (static site *in front of* WordPress). They solve the same problem in different ways and change many of the same files.

Other things for a reviewer to know:

- **#282 is the easiest to merge.** It touches one file that no other open PR changes.
- **#281 shares a file with #274 and #276.** All three edit the Wayback archiving plugin, so whichever merges later will need its conflicts resolved.
- **Many PRs touch the same core files.** The macOS launcher script (`app/MacOS/onionpress`) is changed by 7 of the 10 PRs. The Tor container setup and `docker-compose.yml` are changed by #272, #274, #276 and #280. Expect merge conflicts after each merge.
- **#280 and #274 both change the Tor container image.** #280 adds a single file listing the exact image versions used everywhere, and #274 notes that those versions are currently out of sync. Merging #280 first would give #274 one place to update.
- **#276 contains #274 and #275.** Merge those two first. #276's own changes are its last nine commits.
- **#273 changes the status file's format.** Anything that reads reachability as yes/no has to be updated at the same time.
- **Several need an app rebuild or new images before users see them:** #274 and #280 (container images), and #279 (launcher scripts are copied in at build time).
- **#261 and the static-site PRs** (#275, #276) both edit `multisite.py`.

## Sources

- [Open pull requests on GitHub](https://github.com/brewsterkahle/onionpress/pulls), read through the public GitHub API
- [#277: guoliu's static-site series tracker](https://github.com/brewsterkahle/onionpress/issues/277)
