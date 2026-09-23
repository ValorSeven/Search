# Valor Next

This branch is the hardened experimental fork.

## Run it

On macOS 14+ with Xcode / Swift 6 installed:

```bash
git checkout valor-next
SEARCH_PROBE=valor-next ./fresh.sh
```

That builds `build/Search.app`, launches a named isolated world and leaves your
normal Search profile alone.

Drive that test world from another terminal:

```bash
./bench --world valor-next open https://example.com
./bench --world valor-next tabs
```

For a normal installed-style run:

```bash
./build.sh
open build/Search.app
```

## What changed first

The first pass deliberately targets the highest-leverage boundary rather than
visual churn:

1. Bench has a separate persistent WebKit cookie/site-data profile.
2. Normal Bench can only address Bench-owned tabs.
3. Normal Bench only opens HTTP/HTTPS.
4. Password/form capture and fill are disabled in Bench tabs.
5. User extensions are not attached to normal Bench tabs.
6. App UI and extension automation are test-world-only.
7. Web Inspector is debug-only.
8. macOS CI builds both debug and release configurations.

Keep `main` as the upstream mirror and do experiments on `valor-next`.
