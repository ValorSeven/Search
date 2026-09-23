# Security model

This fork treats local browser automation as privileged code.

## Trust boundaries

### Personal browsing
Normal tabs use Search's primary WebKit data store and may use the macOS
Keychain password flow.

### Automation
Bench tabs use a separate persistent WebKit data store. In a normal run they:

- can only see/control tabs created by Bench
- can only navigate HTTP and HTTPS
- do not load user-installed extensions
- do not participate in Search's password/form relay
- do not write browsing history or session state
- cannot drive Search's settings/password UI

The Unix socket is still local-user-only. Do not expose or proxy it over TCP.

### Test worlds
A run started with `SEARCH_PROBE` is intentionally privileged. Regression tests
may address normal test tabs, exercise extensions, use local/file/data URLs and
drive application UI. Test worlds have their own Application Support folder,
UserDefaults suite and WebKit data store.

Use a named world for development:

```bash
SEARCH_PROBE=valor-next ./fresh.sh
./bench --world valor-next tabs
```

Never point a test-world script at data you care about.

## Release rules

- Keep Web Inspector development-only.
- Never add telemetry by default.
- Do not add remote control transports without authentication and an explicit
  user-visible enable step.
- Keep automation and personal credentials separated.
- Preserve updater signature and Team ID verification.
- Treat extension/native-message changes as security-sensitive.
