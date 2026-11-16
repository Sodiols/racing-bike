# Data, Real-World Prices, Progression and Networking

**Status:** Data contract planning. No Supabase project or migrations have been created.

## Current release posture

Single-player R0-R2 should launch without authentication or a remote database. Store graphics/audio/control preferences locally, with reliable reset behavior. Only introduce backend obligations when there is a real account or competition feature.

## Logical records (not SQL yet)

`motorcycle_catalog`: stable internal ID, original/licensed display name, year, model variant, asset references, powertrain and handling config version.

`motorcycle_market_prices`: bike ID, market country, currency, numeric price, price type (MSRP / dealer quotation / historical listing / estimate), source URL, verified_at date, valid model year, region, taxes/fees notes. Never substitute exchange-rate conversion for dealer price.

`player_profile`: identity, display name, settings (later), safe public fields.

`owned_motorcycles`: user ID, bike ID, acquisition type and timestamp; server authority in connected release.

`race_results`: event ID, course/config version, time, telemetry/trust flags, verification state, ownership.

`economy_ledger`: immutable transaction trail for virtual credits, source, amount, idempotency key, server authorization.

## Regional pricing concept

Players may view prices matching real markets when sources are documented. Real-world retail prices must be labeled with market, model year and verification date. Game unlock cost in credits is a distinct game-balancing parameter, not the same as legal tender price. Do not promise live market accuracy without a refresh process.

## Backend protection (Phase 09 only)

Enable least privilege RLS on all user data; default deny unknown access. Validate transaction amounts and ownership changes server side. Use unique idempotency keys for purchase and result updates. Do not embed Supabase service role secrets in frontend code. Implement backup, migration review and rollback strategies before production changes.

## Multiplayer boundary (Phase 10 only)

Do not use Supabase table updates as the high-frequency authoritative vehicle simulator. Research appropriate dedicated server/synchronization stack with cost, geography, client prediction and latency evidence. Competitive results must be server validated. Consider signed replay/input logs but do not assume these alone prevent cheating.

## Privacy and telemetry

Collect only performance diagnostics needed for optimization; prefer anonymous local logs initially. If collecting accounts, analytics or player identifiers, document consent, retention, access and deletion requirements before launch.
