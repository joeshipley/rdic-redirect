# rdic-redirect

Independent Render static site redirecting the bare `rdic.us` domain to `https://www.rdic.us`. The www site remains hosted by Google.

## Render configuration

- Source: this public repository, branch `main`; automatic deployments off.
- Build command: `mkdir -p public && printf 'RDIC redirect service\n' > public/_redirect-health.txt`
- Publish directory: `public`
- Environment: `SKIP_INSTALL_DEPS=true`
- Redirect rule: source `/*`, destination `https://www.rdic.us/*`, action **Redirect (301)**.
- Do not add `index.html`: existing files take precedence over redirect rules.
- Managed TLS custom domain: `rdic.us`. Render also adds a www alias internally; do not change www DNS to Render.

## DNS

Only the apex uses ANAME `rdic-redirect.onrender.com.` (TTL 1800). Keep `www` CNAME `ghs.googlehosted.com.` and all mail/other records unchanged.

## Verification

Use normal certificate validation; never suppress TLS errors:

```sh
curl -I https://rdic.us/
curl -I 'https://rdic.us/test/path?probe=1&two=2'
curl -I https://www.rdic.us/
```

Expected apex responses: 301 to the matching HTTPS www URL with path and query preserved. The Render hostname can test redirect rules before DNS cutover.
