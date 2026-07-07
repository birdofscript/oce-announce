# oce-announce

Signed announcements channel for the **OneClickExportPRO** Adobe Premiere Pro plugin.

The plugin periodically fetches [`announce-v1.json`](./announce-v1.json) (served via GitHub
Pages at `https://birdofscript.github.io/oce-announce/announce-v1.json`) and shows the
messages inside it as in-panel banners.

## This file is public on purpose

Security comes from the **Ed25519 signature**, not from secrecy (Kerckhoffs's principle).
Every payload is signed with a private key that never leaves the developer's machine, and
the plugin **verifies the signature offline** before trusting a single byte. Anyone can read
this JSON, but nobody can forge or modify it without the private key — a tampered or unsigned
payload is rejected (fail-closed: the plugin simply shows nothing).

No account, token, or telemetry is involved: the plugin only performs an anonymous `GET`.
All message targeting is evaluated **locally on the user's machine**; nothing is ever sent back.

## Do not edit `announce-v1.json` by hand

It is a signed envelope (`{ schemaVersion, keyId, payloadB64, sig }`). Editing the payload
breaks the signature and the plugin will ignore it. To publish new announcements, regenerate
it with the signing script in the plugin repo:

```bash
node scripts/sign-announcements.mjs pool.json announce-v1.json
```

then commit the result here.
