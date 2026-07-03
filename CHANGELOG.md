# Changelog

All notable changes to this module are documented here.

## Security: portal-only licensing (removes forgeable key path)

Closes a licensing bypass. Previous versions shipped the HMAC signing secret
inside `LicenseValidator` (`SECRET_FRAGMENTS` / `BUNDLE_SECRET_FRAGMENTS`) and
validated a locally-computed key against it, so anyone with the module source
could forge a valid key for their own domain and run the module unlicensed. A
`production_environment = No` toggle and a client-settable issued-key grace gave
further bypasses — all values the customer controls.

- Validation is now portal-only: `isValid()` honours a key only when the
  ETechFlow portal confirms it. The module ships no signing secret.
- Removed `computeKey()`, `computeBundleKey()`, `checkKey()`, the HMAC
  `SECRET_FRAGMENTS` / `BUNDLE_SECRET_FRAGMENTS` constants, and the sandbox
  `production_environment` bypass (production is always enforced).
- Offline grace is now un-forgeable: it derives solely from a cached genuine
  portal "valid" response (48h TTL); an explicit portal reject clears it.
- Bundle subscriptions continue to work via a portal-issued `SP-` bundle key.
- Rewrote the unit suite incl. a hard test that a forged `SP-` key with
  attacker-set config and no portal is rejected.

## v1.0.0 — 2026-06-05

Initial public release.

301/302 redirect manager + 404 catcher. Admin grid + hit counters, custom router runs before urlrewrite. One-click create-redirect from a 404 row.
