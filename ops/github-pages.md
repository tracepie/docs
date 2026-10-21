# GitHub Pages and DNS

Target: `https://tracepie.com`, with `www.tracepie.com` redirecting to the apex
when the site is eventually published.

## Current setup — 2026-10-21

- Source repository: <https://github.com/tracepie/docs> (public).
- GitHub Pages build type: `workflow` (GitHub Actions).
- Repository custom domain: `tracepie.com`.
- Deployment: none. No deployment workflow is installed; Pages status is null.
- HTTPS enforcement: pending GitHub certificate issuance and eventual launch.
- Authoritative DNS: `dns1.registrar-servers.com` and `dns2.registrar-servers.com`
  (Namecheap BasicDNS).
- DNS changes and organization domain verification: pending authenticated browser
  access. The existing apex is a Namecheap parking/redirect address,
  `192.64.119.129`; `www` points to `parkingpage.namecheap.com`.

## Required DNS configuration

First add `tracepie.com` in the **tracepie organization → Settings → Pages →
Verified domains**. GitHub supplies a domain-specific TXT value. Add it at
Namecheap using host `_github-pages-challenge-tracepie`, then complete GitHub's
verification. Retain that TXT record. Do not invent the challenge value or use
personal-account verification for this organization-owned site.

Verify ownership before replacing the parking records, since this site will
remain unpublished. Once verified, configure these host records at Namecheap:

| Type | Host | Value |
| --- | --- | --- |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| AAAA | `@` | `2606:50c0:8000::153` |
| AAAA | `@` | `2606:50c0:8001::153` |
| AAAA | `@` | `2606:50c0:8002::153` |
| AAAA | `@` | `2606:50c0:8003::153` |
| CNAME | `www` | `tracepie.github.io` |
| TXT | `_github-pages-challenge-tracepie` | Value supplied by GitHub |

Use the provider's automatic TTL. Replace the apex URL redirect/parking entry
and `www` parking CNAME; preserve unrelated TXT, MX, email and other records.
Do not create wildcard records. Point `www` to the organization hostname,
without `/docs`.

Check authoritative DNS after saving:

```sh
dig @dns1.registrar-servers.com tracepie.com A +short
dig @dns1.registrar-servers.com tracepie.com AAAA +short
dig @dns1.registrar-servers.com www.tracepie.com CNAME +short
dig @dns1.registrar-servers.com _github-pages-challenge-tracepie.tracepie.com TXT +short
```

## Launch remains a separate action

Do not deploy until explicitly requested. A future Pages workflow must build
with the pinned Zola version, upload `public/` using `actions/upload-pages-artifact`,
and deploy with `actions/deploy-pages` with appropriately scoped permissions.
Use the `github-pages` environment. Do not publish raw source from a branch.

After publication and certificate issuance, enable **Enforce HTTPS** and verify
the apex, `www` redirect, nested documentation, 404 page and certificate.
DNS propagation and certificate availability may take up to 24 hours.

Primary references:

- [GitHub custom domain configuration](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
- [Organization domain verification](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages)
- [GitHub Pages REST API](https://docs.github.com/en/rest/pages/pages)
