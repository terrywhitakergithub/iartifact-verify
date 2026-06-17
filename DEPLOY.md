# iArtifact Verifier — GitHub Pages Deploy

## What lives here

| File | Purpose |
|------|---------|
| `index.html` | Public verifier page — buyers paste an iA-ID to verify |
| `version.json` | Version check endpoint — app reads this on boot to detect updates |

## One-time setup

1. Make sure the repo `terrywhitakergithub/iartifact-verify` exists on GitHub.
2. Go to **Settings → Pages** on that repo.
3. Set **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`.
4. Push both files. GitHub Pages will publish at:
   `https://terrywhitakergithub.github.io/iartifact-verify/`

## Releasing a new version

When you ship a new installer, update `version.json` **before** publishing the installer:

```json
{
  "version": "2.0.1",
  "notes": "One-line summary of what changed",
  "download_url": "https://groundzerostudios.gumroad.com",
  "pub_date": "2026-06-17"
}
```

Then `git add version.json && git commit -m "bump to 2.0.1" && git push`.

Users with the app open will see the update banner within 4 seconds of next launch.

## Verifier URL format

The app generates verify links in the form:

```
https://terrywhitakergithub.github.io/iartifact-verify?ia=<ia_id>
```

The verifier page reads the `?ia=` param from the URL and looks up the iA-ID in
the public registry (if configured) or displays the raw ID for manual comparison.
