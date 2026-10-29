# Nook phone viewer · 1.3.0

This static page receives the paired desktop extension's completed chess suggestions. It displays the move, source and destination, score, depth and optional position diagram. It cannot make moves or send engine/settings commands to the desktop.

Open [Nook Phone](https://zevwtf-boop.github.io/knightlight-phone/) with a pairing link from the desktop extension.

## Pair

1. Install or reload the updated Nook extension on the desktop.
2. Open its Phone pairing page and select the chess tab.
3. Create a session, then scan its QR code or copy its pairing link to the phone.
4. Keep the desktop chess tab, the pairing session and this phone page open. Enable the assistant on the selected chess tab.

The pairing key is in the URL fragment and is not sent to GitHub by an ordinary page request. The viewer keeps the pairing details in browser session storage so refreshing the same phone tab can reconnect. Disconnect removes that saved pairing information. Treat the complete pairing link as private: anyone with it can attempt to open the single viewer connection.

## Prepared replies

On the desktop, open **Control center → Engine → Prepared reply branches** and set **Your side** to White or Black for the current game. The planner can model up to three opponent continuations and calculate a legal response to each. The opponent Elo setting controls an engine approximation; it is not a reliable prediction of a particular person or a probability estimate.

While the opponent thinks, the phone lists **opponent move → your prepared reply**. When the actual complete position matches a ready branch, **Prepared reply matched** means Nook reused the cached response without another engine search. An unexpected move starts a fresh calculation. Position reading, the settle delay and network delivery still add latency. The viewer does not enter premoves or play automatically.

## Connection behavior

The page and extension use a bundled PeerJS library. PeerJS's public cloud service coordinates signaling, and the data channel is encrypted by WebRTC. Chess updates are sent through that channel, rather than stored in GitHub commits or a public database. No third-party scripts, fonts, trackers or images are loaded by this page.

The current transport uses public STUN servers and does not include a TURN relay. Direct connections can fail on restrictive Wi-Fi, cellular networks, VPNs or firewalls. If pairing times out, check that the desktop session is running, create a new pairing link, and try another network. This build cannot guarantee connectivity between every pair of networks.

The phone clears an old suggestion as soon as it receives a changed/invalid/thinking/disabled position. It also clears suggestions after a stale connection is detected. The desktop extension owns the search budget and strength settings; this viewer only receives display updates. A `1700` engine target is a configurable engine strength approximation, not a guaranteed account/player rating.

## GitHub Pages

The published site uses **Deploy from a branch → main → / (root)** in GitHub Pages settings. No server process or repository secret is required.

From the Nook product folder, run `node Developer/build-phone.cjs`. It builds `PhoneSite/dist/index.html`, the editable-source bundle `source.json`, `LICENSE.txt` and `.nojekyll`. Publish these files at the repository root. The index contains the CSS, application code, bundled PeerJS and its license notice, so no separate runtime assets are needed. Include `source.json` and `LICENSE.txt` with each update to distribute the corresponding editable sources and license.

The website is a public application shell. Session keys and game positions must never be added to the repository. The extension's pairing page generates a fresh key for each session.

## Files

- `index.html`: mobile interface.
- `style.css`: responsive layout.
- `app.js`: paired display and stale-state protection.
- `vendor/peerjs.js`: locally bundled PeerJS dependency.
- `vendor/peerjs-LICENSE.txt`: dependency license.

Application source is supplied under the Nook project's GPL-3.0-or-later license. PeerJS retains its upstream license notice.
