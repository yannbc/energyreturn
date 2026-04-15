# Infrastructure

Last updated: 15 April 2026

## Hosting

- **Platform:** GitHub Pages
- **Repo:** https://github.com/yannbc/energyreturn
- **Deploy:** Push to `main` triggers `.github/workflows/pages.yml`, which deploys the `site/` directory
- **Custom domain:** energyreturn.co (configured via `site/CNAME`)

## DNS (Cloudflare)

- **Zone:** energyreturn.co
- **Zone ID:** e65983df27f4e960943c5f338a26e293
- **Account ID:** 3e21eac5b3d141f9316933b66c12287c
- **Account email:** yannburden@gmail.com

### Records managed by Cloudflare Email Routing (auto-created)

| Type | Name | Content | Priority |
|------|------|---------|----------|
| MX | energyreturn.co | route1.mx.cloudflare.net | 5 |
| MX | energyreturn.co | route2.mx.cloudflare.net | 82 |
| MX | energyreturn.co | route3.mx.cloudflare.net | 66 |
| TXT | energyreturn.co | v=spf1 include:_spf.mx.cloudflare.net ~all | -- |
| TXT | cf2024-1._domainkey.energyreturn.co | DKIM key (auto-managed) | -- |

### Removed records

| Record | Reason | Date |
|--------|--------|------|
| MX `mx.hover.com.cust.hostedemail.com` (priority 10) | Replaced by Cloudflare Email Routing | 15 Apr 2026 |
| TXT `canva-domain-verify=40e206d3-...` | Canva account cancelled; CV now hosted on /me | 15 Apr 2026 |

## Email

- **Provider:** Cloudflare Email Routing (free)
- **Forwarding rules:**
  - `yann@energyreturn.co` -> `yannburden@gmail.com`
  - `hello@energyreturn.co` -> `yannburden@gmail.com`
- **Sending:** Requires Gmail "Send mail as" configuration (see below)

### Gmail "Send mail as" setup (manual)

To send from `yann@energyreturn.co` via Gmail:

1. Gmail Settings > Accounts and Import > Send mail as > Add another email address
2. Enter `yann@energyreturn.co`, leave "Treat as an alias" ticked (this means Gmail treats both addresses as you, so incoming and outgoing both work in your normal inbox)
3. SMTP server: `smtp.gmail.com`, port 587, Gmail username + Google App Password
4. App Password: Google Account > Security > 2-Step Verification > App Passwords
5. Gmail sends a verification email to `yann@energyreturn.co`, which forwards to your inbox. Click the link to confirm.

## Site structure

```
site/
  index.html        -- Landing page (energyreturn.co)
  me/index.html     -- CV page with highlight switcher (Full / Operations-CTO / AI Leadership / Advisory-Consulting)
  cv/index.html     -- Redirect to /me
  og-card.png       -- 1200x630 social card for og:image
  CNAME             -- energyreturn.co
```

## Social / og:image

- `og:title`, `og:description`, `og:url`, `og:image` tags added to both `index.html` and `me/index.html`
- Social card: `site/og-card.png` (dark green, "Yann Burden / Founder - Advisor - Technology Leader")
- LinkedIn Featured links should show the thumbnail once LinkedIn re-fetches the URL metadata

## CV variants

```
cv/
  cv-base.md        -- Base CV, data science and AI emphasis
  cv-consulting.md  -- Consulting/advisory variant
```

## Previous services (no longer used)

| Service | Was used for | Status |
|---------|-------------|--------|
| Canva | Hosting a designed PDF CV | Cancelled. DNS verification record removed. CV now lives at /me |
| Hover email | MX record for energyreturn.co | MX removed. Email routing moved to Cloudflare |

## Other accounts

- **Domain registrar:** Hover (energyreturn.co registered there; DNS managed by Cloudflare)
- **LinkedIn:** https://www.linkedin.com/in/yannburden/
- **Podcast:** Tech Trajectory with Kavita Karwar (July 2025) -- https://techtrajectorypodcast.buzzsprout.com/2457356/episodes/17422143
