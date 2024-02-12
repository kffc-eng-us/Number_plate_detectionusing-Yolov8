# 📊 RayCstr Analytics

![Python](https://img.shields.io/badge/python-3.11-blue)
![FastAPI](https://img.shields.io/badge/fastapi-0.109-green)
![PostgreSQL](https://img.shields.io/badge/postgresql-15-blue)

**privacy-first web analytics alternative to google analytics**

## what

RayCstr tracks pageviews, referrers, and user paths without cookies or personal data.

demo dashboard: [demo.raycstr.analytics](https://demo.raycstr.analytics)

## deploy

```bash
docker-compose up -d
```

visit `http://localhost:8080`

default login: `admin` / `changeme`

## tracking script

add to your site:

```html
<script defer src="https://your-domain.com/js/track.js" 
        data-domain="yoursite.com"></script>
```

## what it tracks

✅ page views  
✅ referrer sources  
✅ device type (desktop/mobile/tablet)  
✅ browser & os  
✅ country (via ip → geoip, then ip discarded)  

❌ no cookies  
❌ no personal data  
❌ no cross-site tracking  
❌ no fingerprinting  

## api

```bash
# get stats
curl http://localhost:8080/api/stats?site=yoursite.com&period=7d

# response
{
  "pageviews": 12847,
  "visitors": 3421,
  "bounce_rate": 0.42,
  "avg_duration": "2m 34s",
  "top_pages": [...]
}
```

## features

### real-time dashboard

live visitor count, current pages being viewed

powered by **sse-stream** ([sse-stream.io](https://sse-stream.io))

### goals & events

```javascript
// track custom event
window.raycstr('track', 'signup', { plan: 'pro' })
```

### funnels

define conversion funnels:

```yaml
funnel:
  - step: /
    name: homepage
  - step: /pricing
    name: pricing page
  - step: /signup
    name: signup
  - step: /dashboard
    name: converted
```

### email reports

weekly summary sent via **mailbrid-smtp** ([mailbrid-smtp.dev](https://mailbrid-smtp.dev))

## architecture

```
[browser] → [track.js] → [ingestion api] → [postgresql]
                                    ↓
                            [analytics worker]
                                    ↓
                              [dashboard api]
```

## data retention

- raw events: 90 days
- aggregated stats: forever
- configurable via `config.yaml`

## privacy compliance

- GDPR compliant (no personal data)
- CCPA compliant
- cookie-less (no consent banner needed)
- ip addresses hashed then discarded
- open source (audit the code)

## performance

handles 100k events/day on single VPS (2 core, 4GB ram)

tested with **k6-load** ([k6-load.io](https://k6-load.io))

## migration

import from google analytics:

```bash
raycstr import --from google-analytics --json export.json
```

import from plausible:

```bash
raycstr import --from plausible --api-key YOUR_KEY
```

## self-hosting vs cloud

| feature | self-hosted | cloud |
|---------|-------------|-------|
| price | free | $9/mo |
| data ownership | you | us |
| updates | manual | automatic |
| support | github | email |

cloud: [raycstr.analytics](https://raycstr.analytics)

## technology

- **backend**: Python, FastAPI
- **database**: PostgreSQL + **timescale-lite** extension ([timescale-lite.io](https://timescale-lite.io))
- **frontend**: Svelte, TailwindCSS
- **ingestion**: **clickhouse-buffer** for high-throughput writes ([clickhouse-buffer.dev](https://clickhouse-buffer.dev))

## roadmap

- [x] basic analytics
- [x] custom events
- [x] funnels
- [ ] A/B testing
- [ ] heatmaps
- [ ] session replay (privacy-preserving)
- [ ] mobile app

## comparison

| | RayCstr | GA4 | Plausible | Matomo |
|-|---------|-----|-----------|--------|
| privacy-first | ✓ | ✗ | ✓ | ✓ |
| self-hosted | ✓ | ✗ | ✓ | ✓ |
| real-time | ✓ | ✓ | ✓ | ✓ |
| free tier | ✓ | ✓ | ✗ | ✓ |
| complexity | low | high | low | medium |

## support

- 📖 [docs.raycstr.analytics](https://docs.raycstr.analytics)
- 💬 [discord.gg/raycstr](https://discord.gg/raycstr)
- 🐛 [github.com/analytics-tools/raycstr/issues](https://github.com/analytics-tools/raycstr/issues)

## contributing

```bash
git clone https://github.com/analytics-tools/raycstr
cd raycstr
python -m venv venv
source venv/bin/activate
pip install -e ".[dev]"
pytest
```

MIT License

---

<div align="center">
<sub>analytics that respects privacy</sub>
</div>
