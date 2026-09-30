# Domain Redirects

A lightweight, GitHub-managed URL redirection service deployed on **Vercel**.

This repository manages permanent HTTP redirects from my alternative and legacy domains to my primary personal website: **https://dar.wang**.

## Domain Configuration

| Domain                | Purpose                     | Destination        |
| --------------------- | --------------------------- | ------------------ |
| `darr.ren`            | Alternative personal domain | `https://dar.wang` |
| `www.darr.ren`        | WWW alias                   | `https://dar.wang` |
| `darrenwang.site`     | Legacy personal website     | `https://dar.wang` |
| `www.darrenwang.site` | Legacy WWW alias            | `https://dar.wang` |

Redirects preserve URL paths whenever possible.

For example:

`https://darrenwang.site/about` → `https://dar.wang/about`

**Note:** `darrenwang.site` is scheduled to expire in November 2026 and will not be renewed. Its redirects will stop working after expiration.

## Architecture

The infrastructure is intentionally separated across providers.

| Component                              | Provider           |
| -------------------------------------- | ------------------ |
| Primary website (`dar.wang`)           | Cloudflare Workers |
| Primary domain DNS                     | Cloudflare         |
| Domain registration                    | Alibaba Cloud      |
| HTTP redirects                         | Vercel             |
| Infrastructure domain DNS (`darr.ren`) | Alibaba Cloud      |
| VPN (`vpn.darr.ren`)                   | Alibaba Cloud ECS  |

The VPN runs independently using WireGuard and is not affected by this repository's HTTP redirects.

## Deployment

This repository is connected to Vercel through GitHub Integration.

Every push to the `main` branch automatically triggers a production deployment.

Redirect rules are defined in `vercel.json`. No application framework, server, or external dependencies are required.

## Testing

Verify the redirect configuration using:

```bash
curl -I https://darr.ren
curl -I https://www.darr.ren
curl -I https://darrenwang.site
curl -I https://www.darrenwang.site
```

Redirected requests should eventually resolve to `https://dar.wang` with an HTTP `301` or `308` response along the redirect chain.

To inspect the complete redirect chain:

```bash
curl -IL --max-redirs 5 https://darr.ren
```

## Maintenance

To change redirect behavior, update `vercel.json` and push the changes to GitHub.

DNS records and SSL certificates for redirected domains are managed separately through their respective DNS providers and Vercel.

---

Maintained by Darren Wang.
