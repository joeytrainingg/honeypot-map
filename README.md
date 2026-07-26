# Honeypot Attacker Heatmap

Live world heatmap of who's attacking a personal SSH/service honeypot. Source
IPs are collected by the honeypot, correlated in Wazuh SIEM, geolocated, and
rendered on a Leaflet dark map with a ranked attacker table (hits, city,
country, ISP).

Single static page — no build step, no backend. `index.html` fetches
`data.json` and draws the map using the vendored Leaflet in `lib/`.

## Run locally

The page fetches `data.json` over HTTP, so it must be **served** — opening the
file directly (`file://`) won't load the data. Any static server works:

```bash
git clone https://github.com/joeytrainingg/honeypot-map.git
cd honeypot-map
python3 -m http.server 8000    # or: npx serve .
```

Then open http://localhost:8000.

## Deploy

Published as a static site via **GitHub Pages** (Settings → Pages → deploy from
branch, root). Any static host works — upload `index.html`, `lib/`, and
`data.json`.

## Data

`data.json` has the shape:

```json
{
  "updated": "2026-07-24T03:49:41+00:00",
  "attackers": [
    { "ip": "45.153.34.167", "hits": 6846, "lat": 50.89, "lon": 6.06,
      "city": "Eygelshoven", "country": "The Netherlands", "cc": "NL",
      "isp": "TechTies Inc." }
  ]
}
```

It's produced externally by the honeypot + Wazuh pipeline and dropped into the
repo (or Pages deploy) to refresh the map; that collection code isn't part of
this repo. Location = IP datacenter, not the operator.
