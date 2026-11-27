# OmegaNexus Fresh Proxy Feed

A continuously running proxy hunter: it collects public proxies from 150+ sources, re-tests every one of them around the clock, grades their anonymity, enriches them with geo/ASN data — and serves the survivors through one simple API. It also checks YOUR proxies: submit any list and get every entry graded.
**Get it on RapidAPI:** https://rapidapi.com/proxyproinsight/api/omeganexus-fresh-proxy-feed
Free BASIC plan included. Paid plans from $12/mo.

## What you get

- **Live feed** of working HTTP, SOCKS4 and SOCKS5 proxies, re-validated continuously (public proxies die fast — the pool is re-checked around the clock)
- **Anonymity grades**: elite / anonymous / transparent, verified by an anonymity judge, not self-declared
- **Noble pool**: the strict cut — elite anonymity AND low risk score
- **Geo enrichment**: country, city, postal code, ASN, connection type (residential / mobile / hosting)
- **Proxy checker**: bring your own list (up to 500 per batch) and get back alive/dead, protocol, latency, anonymity, exit IP, geo and a risk score for every proxy

## Endpoints

| Endpoint | What it does |
|---|---|
| GET /api/proxies | The live pool (filter by protocol, country, anonymity, min score) |
| GET /api/best | Top-ranked proxies |
| GET /api/noble | Elite + low-risk only |
| GET /api/proxies/export | Pool as a plain-text list |
| GET /api/countries | Pool counts by country |
| GET /api/protocols | Pool counts by protocol |
| GET /api/stats | Pool statistics |
| POST /api/check | Submit your own proxies for grading |
| GET /api/check/{job_id} | Poll a checker job for results |
| GET /health | Service status |

All /api/* calls go through RapidAPI with your RapidAPI key.

## Quickstart

Feed (via RapidAPI):

    curl "https://omeganexus-fresh-proxy-feed.p.rapidapi.com/api/proxies?protocol=socks5&anonymity=elite&limit=10" \
      -H "X-RapidAPI-Key: YOUR_KEY" \
      -H "X-RapidAPI-Host: omeganexus-fresh-proxy-feed.p.rapidapi.com"

Checker — submit a list:

    curl -X POST "https://omeganexus-fresh-proxy-feed.p.rapidapi.com/api/check" \
      -H "X-RapidAPI-Key: YOUR_KEY" \
      -H "X-RapidAPI-Host: omeganexus-fresh-proxy-feed.p.rapidapi.com" \
      -H "Content-Type: application/json" \
      -d '{"proxies": ["1.2.3.4:8080", "socks5://5.6.7.8:1080"]}'

Then poll the returned job_id at GET /api/check/{job_id} until status is "done". Bare ip:port entries are auto-detected as HTTP, SOCKS5 or SOCKS4. Only public IPv4 proxy addresses are accepted.

## Free lists

Plain-text samples of the **delayed (free-tier) pool**, refreshed hourly by GitHub Actions:

- [`lists/all.txt`](lists/all.txt) — `scheme://ip:port`, best first
- [`lists/http.txt`](lists/http.txt) — HTTP, `ip:port` per line
- [`lists/socks4.txt`](lists/socks4.txt) — SOCKS4, `ip:port` per line
- [`lists/socks5.txt`](lists/socks5.txt) — SOCKS5, `ip:port` per line

These are a small, honest sample: the delayed pool is what remains once the live pool has moved on, so files can be short — and a protocol file can occasionally be empty for an hour. That's the real state of the free pool at that moment, not a bug. For the full live feed, graded and trust-checked, use the RapidAPI listing above.

## Notes

- Free tier sees a delayed pool; paid tiers see the live pool.
- Checker results include a heuristic risk score (0-100, lower is cleaner) based on the live test — it is a quality grade, not an abuse-history database.
- Built and run by ProxyPro Insight. Questions via the RapidAPI discussion tab.
