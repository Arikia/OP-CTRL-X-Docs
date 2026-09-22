# OnionPress

This repository holds my research notes and testing work on
[OnionPress](https://github.com/brewsterkahle/onionpress) — this is **not**
the official project. For the actual software, installers, and
documentation, go to the upstream repo:
[brewsterkahle/onionpress](https://github.com/brewsterkahle/onionpress).

## Who I am

I'm an independent researcher with a journalistic background. I'm not
affiliated with the OnionPress team — I'm developing and testing the
project on my own, focused specifically on **usability**: how it actually
feels to set up, use, and depend on as a real, ordinary person, not just
whether the underlying technology works.

## Why I'm doing this

I think OnionPress is a genuinely interesting answer to a problem a lot of
us feel right now: the platforms we use to publish and connect with people
keep getting worse. Algorithms change without warning, accounts get
suspended with no recourse, feeds get filled with ads and engagement bait,
and ownership of what you post is never really yours. That slow decline
has a name people use now — "enshittification" — and it isn't slowing
down.

OnionPress runs your site from your own computer, gives you a permanent
address that no company or registrar controls, and backs your posts up to
the Internet Archive automatically. Nobody can suspend it, sell it out
from under you, or bury it in an algorithm. I think tools like this can
work as a **lifeboat** — somewhere to land, and somewhere to actually own
your own words, as the platforms people have built their communities on
keep getting less trustworthy.

## What's in this repo

Notes, findings, and write-ups from my testing, including some deeper
dives into how specific parts of the system work and bugs I've found
along the way:

- [`why-tor.md`](why-tor.md) — why this project runs on Tor/onion
  services at all, and what that actually gives you as a publisher
- [`tor-timeout-report.md`](tor-timeout-report.md) — a writeup of a real
  bug I found and fixed, where a too-short network timeout was silently
  breaking both the Wayback Machine backups and the "Following" feature
- [`ONIONHEAVEN.md`](ONIONHEAVEN.md) — how OnionPress's failover system
  keeps a site reachable even when the person's own computer is offline

Any fixes that came out of this testing get submitted back upstream as
pull requests against
[brewsterkahle/onionpress](https://github.com/brewsterkahle/onionpress).
