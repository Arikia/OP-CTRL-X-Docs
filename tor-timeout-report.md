# How Tor timeouts broke Wayback archiving and Following — and how we fixed it

This is a plain-English writeup of a bug we found and fixed across two
pull requests: [#281](https://github.com/brewsterkahle/onionpress/pull/281)
and [#282](https://github.com/brewsterkahle/onionpress/pull/282). Both PRs
fix the same underlying mistake in two different places.

## The big picture

OnionPress talks to the outside world over Tor for a few reasons: to save
your posts to the Internet Archive's Wayback Machine, to check on sites
you're Following, and to reach a few other onion services. All of that
outbound traffic goes through a Tor SOCKS proxy.

Connecting to something over Tor is slower than connecting over the
regular internet, because Tor has to build a private, encrypted path
through several relays before your request can even start. In our testing
this routinely took **20 to 30 seconds**, sometimes more, before the
connection was even established — and that's for a completely healthy,
reachable site.

## How it was originally configured

In two separate places in the code, the "give up and call it a failure"
timer for that connection step was set to **15 seconds**:

1. **The Wayback archiver** (`onionpress-wayback-archive.php`) — the
   background process that submits your posts to the Internet Archive.
2. **The Tor proxy layer** (`onionpress-tor-proxy.php`) — the shared code
   that routes *all* of WordPress's outbound web requests through Tor,
   including the Following feature checking on sites you follow.

15 seconds is shorter than the time it normally takes just to connect —
before a single byte of the actual request or response is exchanged. So
in practice, almost every one of these outbound Tor calls was being cut
off before it ever had a chance to succeed.

## Why this was hard to notice

The failures weren't loud. Nothing crashed, and nothing showed an obvious
error:

- **Wayback**: when a submission failed, the code just quietly treated it
  as "nothing to report" and moved on. Worse, when the initial "how many
  submission slots do I have?" check also timed out, the code assumed an
  optimistic default number instead of raising an alarm — so the logs
  showed what looked like a perfectly healthy process ("40 slots
  available, 0 submitted") running forever, when in reality it had never
  successfully talked to the Internet Archive at all. Your first blog
  post sat unarchived for the entire time this went unnoticed.
- **Following**: when a feed check failed, the site got flagged as
  unreachable and a caution triangle appeared next to it in your
  dashboard. Repeated failures made the system back off and stop
  checking that site for hours at a time. But the site wasn't actually
  down — we confirmed this directly by connecting to it ourselves with a
  longer timeout, and it answered immediately.

In both cases, a *timing* problem was silently disguising itself as a
*connectivity* problem — the system kept telling you things were failing
or offline when they were actually just slow to reach.

## How it should work (and now does, once these PRs are merged)

- The connection timeout for reaching other onion services over Tor is
  raised from 15 seconds to **45 seconds**, which gives a real Tor
  circuit enough time to actually finish connecting.
- The overall time budget for the whole request (connect + get a
  response) is raised to **60 seconds**, so it can't accidentally cut a
  request short again just because the connection step alone used up
  most of the old, smaller budget.
- When a connection genuinely does fail now, the system **logs what
  actually happened** (the real error and response code) instead of
  silently shrugging and moving on. That means a future problem — a site
  that's really down, credentials that are missing, whatever it turns
  out to be — will show up in the logs as itself, instead of looking
  identical to "everything's fine."

## What we confirmed, concretely

- Your first blog post had never been archived to the Wayback Machine.
  After the fix, the archiving daemon successfully archived it along
  with every other post and page on the site within a few minutes.
- Two sites you follow (OnionHeaven and BK OnionPress) were showing
  caution warnings and had accumulated several failed attempts each.
  Both were actually online the whole time. After the fix, a fresh check
  succeeded for both, and the caution warnings cleared.

## Where to look

- PR #281 — the Wayback archiver fix:
  https://github.com/brewsterkahle/onionpress/pull/281
- PR #282 — the shared Tor proxy fix (Following and any other feature
  using WordPress's normal HTTP requests over Tor):
  https://github.com/brewsterkahle/onionpress/pull/282
