# RescuePad

**Launch memes. Rescue animals.**

A static Solana wallet frontend built with HTML, CSS and vanilla JavaScript. Charcoal surfaces and beige buttons retain the original pixel artwork and layout. No installation or build step is needed. Wallet connections are real; token launches, trading and rescue allocations remain local simulations. No transactions are signed or funds transferred.

## Open it

Use localhost for real wallet extensions, or HTTPS when hosted. You can open `index.html` directly for browsing and local simulations, but some extensions do not inject on `file://`. Keep all the accompanying folders in place.

For a consistent localhost storage origin, you can optionally run:

```sh
python3 -m http.server 4173 --bind 127.0.0.1
```

Then visit <http://localhost:4173>. Direct file opening and localhost use separate browser storage. Different hostnames, ports, browsers and devices also have independent profiles.

## Try the complete flow

1. Connect an installed Phantom, Solflare, Backpack or detected Solana Wallet Standard wallet. Approve sharing its public address. Each address starts with 5 SOL for local trading, separate from the real balance.
2. Explore the real rescue listings and pick a rescue.
3. Create a name, unique ticker and description. Optionally upload a meme image.
4. Select the rescue, review the fee split and launch locally.
5. Open the token and simulate a buy. Switch to Sell to sell tokens; Max sells their current SOL value in the local model.
6. Check your profile, the rescue profile and leaderboard. They read the same data.
7. Refresh to confirm persistence. Your progress remains saved in this browser.

All discovery tabs, country filters, token and rescue pages, wallet options, charts, copy buttons, drafts, uploads, profile activity and trade history are interactive. Save memes from token pages and revisit them in your profile. Refresh your mainnet SOL balance and view your address on Solana Explorer. X opens a notice because no official social account has been configured. Docs is a complete local guide.

## Data and simulation

- Starts with 12 original meme tokens, 535 simulated trades and €620.00 in simulated rescue allocations.
- Eight factual European organization listings, all with status `listed`. There are no implied RescuePad affiliations. Every factual description links to an official source, checked 4 October 2026.
- All token and shelter visuals are original SVG illustrations. They do not depict real rescue animals or facilities. No fonts or images require a network connection.
- Anonymous activity generates a small simulated trade approximately every 28 seconds while the tab is visible. It updates the same dataset as user trades.
- Trading fee: 1%; fee split: 50% rescue, 40% creator/community, 10% platform.
- Fixed fictional exchange rates: €140/SOL and $150/SOL. These are local constants, not live quotes.
- Buys subtract gross SOL and credit tokens net of fees. Sells remove tokens at the current simulated price and credit SOL net of fees. Both allocate rescue fees and increase cumulative volume and trade count.
- Euro allocations are stored as integer cents. Every rescue total is the sum of its supporting token allocations. Creator/community and platform shares describe the local economics; they are not credited to wallets.
- New tokens start at a $5,000 market cap with zero trades and zero rescue allocation. Graduation progress uses a $50,000 local target and has no on-chain effect.
- A profile’s rescue-fee figure includes its launched tokens’ allocations plus personal trade allocations in other tokens, avoiding double counting.
- Profiles are keyed by the real wallet address. Reconnecting the same address restores its local launches, holdings, history and saved memes; another address has separate activity.
- Refreshing clears the active connection; reconnect through your wallet. Persisted addresses are never treated as authorized sessions. Disconnecting preserves profile progress. Account-change and disconnect events update the interface.

## Files and configuration

| File | Purpose |
| --- | --- |
| `index.html` | Shell, navigation, modal, footer |
| `css/style.css` | Responsive theme, animation and components |
| `js/data.js` | `RP.CONFIG`, initial tokens, local storage, trading and launch accounting |
| `js/rescues.js` | Factual organization listings, source links, centralized optional official-image URLs |
| `js/art.js` | Original SVG pixel animals and rescue illustrations |
| `js/wallet.js` | Real injected Solana providers, Wallet Standard discovery and balance reads |
| `js/launch.js` | Persistent four-step launch form, upload handling and launch animation |
| `js/app.js` | Routes, discovery, chart, profiles, docs, activity and optional WebMCP |
| `assets/icon.svg` | Original compact launch/paw icon and favicon |
| `assets/wordmark.svg` | Original full wordmark for branding |

Fee percentages, local SOL rates, starting trading balance, graduation target, activity interval, `rpcUrl` and image mode are centralized in `RP.CONFIG` in `js/data.js`. Shelter totals are derived, never independently randomized. Seeded volumes are computed from seed allocations using the fee configuration.

Official remote image URLs are stored in `js/rescues.js` as requested. Local original illustrations are the default for reliable offline use. Set `useOfficialImages: true` to opt into available official-site imagery; failed loads fall back to the original illustration. Remote assets may change or be unavailable. Check image rights before any public release.

Uploads accept PNG, JPG, WebP and GIF up to 5 MB. They are resized to a maximum of 480 pixels and stored locally as WebP; animated GIFs become a still image. Browser-storage errors are surfaced, and failed writes roll back state changes. The reset function is available internally for isolated model checks; there is no reset button in the interface.

## Verification

Dependency-free financial and persistence checks:

```sh
node tests/model.cjs
```

Optional browser tests require an already installed Playwright module and Chromium. The application does not require either:

```sh
PLAYWRIGHT_MODULE=/absolute/path/to/playwright \
CHROME_PATH=/absolute/path/to/chromium \
node tests/browser.cjs
```

Run the optional server first. `PREVIEW_URL` defaults to `http://127.0.0.1:4173`; `QA_OUTPUT` defaults to `/tmp/rescuepad-qa`. The suite uses an isolated browser context and does not alter your normal browser’s local progress.

Browser checks use test-only provider fixtures, not personal wallets. Verified: wallet approval/rejection, connection/disconnection, account switching, profile isolation and persistence; profile; all rescue filters, profiles and official-link destinations; launch, upload and selection; fee display; discovery and profile inclusion; buy/sell/Max; insufficient-funds protection; balance, holdings, chart, volume, trade and fee updates; consistent shelter and leaderboard totals; persistence; all routes at 320px and 390px; direct `file://` operation; image loads; no uncaught JavaScript errors.

Optional WebMCP tools are feature-detected on supporting browsers: read the local local and open a launch form. Unsupported browsers work normally. Native WebMCP validation was unavailable in this environment; this does not affect the visible interface.

Real wallet connection is implemented using the [Phantom provider API](https://docs.phantom.com/solana/establishing-a-connection) and the [Wallet Standard discovery protocol](https://github.com/wallet-standard/wallet-standard/blob/master/packages/core/app/src/wallets.ts). Token deployment, on-chain trading, fee routing and organizational agreements require a separate on-chain implementation. A wallet connection shares a public address; it is not an authenticated backend login.

## Rescue progress and launch preview

- Rescue directory cards show progress toward the next allocation milestone. Rescue profiles show the full €100 / €250 / €500 milestone ladder, reached targets and the amount remaining. Milestones use existing supporting-token totals; targets are centralized in `RP.CONFIG.rescueMilestonesEUR`.
- “Rescues I support” groups personal fee allocations and links to supporting memes. Launched memes include all fees they generate; personal trades in other memes add their own fees. Trades in your own launched tokens are counted once. Empty, refreshed and reset profiles are supported.
- The launch sidebar previews the token name, ticker, description, image, chosen rescue and configured rescue share. It updates while typing without replacing the form or moving focus. Uploads, image removal and saved drafts update the same preview.

Run `node tests/features.cjs` with the same optional Playwright/Chromium environment variables above for focused browser checks of these features. It opens the local file directly and uses isolated browser storage.

## GitHub Pages

Keep the site local until you choose to publish. When ready, upload `index.html`, `css/`, `js/`, `assets/` and `.nojekyll` to the repository root. Enable GitHub Pages using that branch and its root folder. No build step, secrets or server are required. All assets use relative URLs and pages use hash routes, so repository subpaths work without rewrite rules. Do not upload browser storage or local test artifacts.

On desktop, install a wallet extension. On mobile, open the HTTPS URL inside Phantom, Solflare or another compatible wallet browser. Other detected **Solana Wallet Standard** wallets appear automatically; arbitrary EVM wallets and wallet apps without a compatible provider are not supported.

The mainnet balance uses `RP.CONFIG.rpcUrl`, initially Solana’s public RPC. Rate limiting or CORS restrictions can make reads temporarily unavailable; the interface offers retry and an explorer link. For high traffic, configure a browser-compatible mainnet RPC URL. Any RPC URL in a static site is public, so never embed private credentials. Wallet connection itself does not depend on RPC availability.

Additional wallet protocol and hosting tests:

```sh
PLAYWRIGHT_MODULE=/absolute/path/to/playwright \
CHROME_PATH=/absolute/path/to/chromium \
node tests/wallet.cjs
```

These verify protocol discovery, rejected approval, account events, separate profiles, saved memes, successful and failed balance reads, reconnect behavior, mobile layouts and a GitHub Pages repository subpath. Real extension approval requires a manual check with an installed wallet.

## Graduation progress

Each meme’s progress rises gradually while the page is visible, starting from its market-cap percentage. The simulated milestone advances at 0.9–1.26 percentage points per minute, updates every five seconds, and persists in browser storage. Progress never decreases and stops at 100%; there is no offline catch-up. Each progress step represents a small anonymous buy using the same fee accounting as user trades, so volume, market cap, trade count and rescue fees rise together. Completed memes stop generating progress-step buys; normal trades can still increase their fees. Trade-driven market-cap increases can also advance progress.

At 100%, the card badge and progress bar turn green and read “Reached GRAD”. The token page displays “Reached GRAD” and its accumulated rescue allocation as a simulated donation amount. These are local simulation milestones, not verified transfers. Rates are configured in `RP.CONFIG`.
