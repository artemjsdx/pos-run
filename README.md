# pos-run
Public scheduler. Clones the private core, runs one scan cycle every 5 minutes, commits state back.

## Proxy failover (playerok DDoS-Guard)

PlayerOk sits behind DDoS-Guard: most datacenter IPs are dropped. The scanner's
egress proxy lives in the private core repo (`config.json -> proxy`), NOT here —
do not put proxy IPs in this public repo.

- Main proxy + verified reserves + a 5-minute refarm recipe: see private
  `pos-core/docs/proxy_reserve.md`.
- Health probe: `curl -sk -m 12 -x <proxy> https://playerok.com/` must return
  HTTP 200 with page content; a JSON reply (even HTTP 500) on `POST /graphql`
  means the request reached the app.
- Switching = edit `proxy.url` in the core config and commit; the wisp
  deployment hot-reloads its config within seconds, no restart needed.
