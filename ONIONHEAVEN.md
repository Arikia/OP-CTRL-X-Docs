# OnionHeaven

OnionHeaven is a centralized failover service for OnionPress sites. It is not
per-instance infrastructure — it's one fixed, shared hidden service that every
OnionPress installation registers with:

```
oheavenfhbohpdjijmxo3xgvvuo6eleyhhorbompoycle6x5eajlp7qd.onion
```

(hardcoded as `ONIONHEAVEN_ADDRESS` in `src/onionpress/onionheaven.py`)

## Purpose

When a user's OnionPress instance goes offline — laptop closed, containers
crashed, network dropped — visitors to their `.onion` address would normally
just get a timeout. OnionHeaven takes over the address instead and serves a
redirect to the Internet Archive's Wayback Machine, so visitors land on an
archived copy of the site rather than a dead connection.

## How it works

1. **Registration.** On startup, an instance sends its onion service's
   private key material to OnionHeaven in an initial `/online` call. This is
   required because taking over an address later means holding its private
   key.
2. **Heartbeat.** Every 60 seconds after that, the instance sends a
   lightweight "still alive" heartbeat to OnionHeaven.
3. **Takeover.** If OnionHeaven misses 3+ heartbeats (180 seconds of
   silence), `onionheaven-heartbeat.py` on the hub side triggers a takeover:
   an `onionheaven-takeover-worker.py` container stands up the same onion
   address using the stored key and serves `onionheaven-redirect.sh` — a 302
   redirect to the Wayback Machine's onion mirror.
4. **Release.** The moment the real instance's heartbeats resume, the
   takeover releases the address and the instance's real WordPress serves
   again.
5. **Scaling.** Takeover work is spread across multiple
   `onionheaven-takeover-N` worker containers (each with its own Arti/guard
   pool, capped at roughly 10 services each) so that one Tor instance isn't
   overwhelmed managing too many hidden services simultaneously.

## Why archiving traffic is routed through it

Each OnionPress instance runs its own local `onionheaven` Docker container —
a separate Tor daemon from the one serving the actual site
(`onionpress-tor`). Bulk outbound traffic, such as Wayback Machine Save Page
Now submissions (see `app/Resources/plugins/onionpress-wayback-archive.php`),
is routed through this local `onionheaven` Tor instance rather than the
site's own. This keeps heavy outbound bursts from competing with the
heartbeat and inbound traffic that keep the site from being mistakenly taken
over.

## Related but distinct: the naming directory

A separate feature, sometimes seen at a path like `<hub>.onion/<name>`
(implemented in `onionpress-directory.php`), is a phonebook-style directory
that resolves a human-readable name to a user's real onion address. It
shares the same OnionHeaven Tor proxy for lookups but is architecturally
distinct from the heartbeat/takeover failover mechanism described above.
