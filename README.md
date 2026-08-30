# acekapila.com

Plain static HTML. No build step, no framework, no dependencies.
Everything can be edited directly in the GitHub web UI.

## Previewing locally

All asset paths are relative, so you can open `index.html` straight from the
folder in a browser and it will render correctly. The repo list needs an
internet connection; without one it falls back to a plain GitHub link.

## Structure

```
index.html              home — durable statement + dated "Working on" block
writing/index.html      the register: things published elsewhere + hosted here
writing/_template/      copy this folder to start a new hosted piece
assets/style.css        all styling, one file
CNAME                   acekapila.com
robots.txt
```

## Showing repositories

The **Code** block on `index.html` fills itself from the GitHub API at page
load. You never edit it.

To choose which repos appear: on GitHub, open a repo, click the gear next to
"About", and add the topic `showcase`. Any repo carrying that topic shows on
the site, newest first. If no repo carries it, the four most recently pushed
public repos appear instead. Forks, archived and private repos are always
excluded.

Change how many appear via `data-limit` on the `<ul id="repos">` element.
Change the topic via `data-topic`.

If GitHub is unreachable or rate-limited (60 requests/hour per visitor IP,
unauthenticated), the block degrades to a plain link to your GitHub profile.
No API key is used or needed.

## Adding something published elsewhere (the common case)

Open `writing/index.html`, find the **Elsewhere** section, copy the commented
`<li>` template, fill in URL / title / venue / date. Add newest at the top.

Then update the **Recent** block on `index.html` so the home page shows it.

## Adding something hosted here

1. Copy `writing/_template/` to `writing/your-slug/`
2. Edit the title, description, canonical URL, date and body in that file
3. Add an `<li>` in the **Here** section of `writing/index.html` pointing at
   `/writing/your-slug/`

URLs are permanent, so pick the slug carefully. Lowercase, hyphens, no dates:
`/writing/auditing-ai-systems-cpg-234/`

## Changing focus

The `<h1>` and the paragraph under it on `index.html` are meant to be durable —
they should survive a change of topic. When your focus moves, change only the
**Working on** block and its date. That block is marked with a comment.

## Deploying to GitHub Pages

1. Create a public repo named exactly `acekapila.github.io`
2. Push everything in this folder to the root of that repo
3. Settings -> Pages -> Source: "Deploy from a branch", branch `main`, folder `/ (root)`
4. Settings -> Pages -> Custom domain: `acekapila.com`, save.
   Do this BEFORE changing DNS — configuring DNS first can allow someone else
   to host on one of your subdomains.
5. At the registrar, four A records on the apex (`@`):
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```
   Optional AAAA records for IPv6:
   ```
   2606:50c0:8000::153
   2606:50c0:8001::153
   2606:50c0:8002::153
   2606:50c0:8003::153
   ```
   Remove any default or parking record the registrar created.
6. CNAME record for `www` -> `acekapila.github.io`
7. Settings -> Pages -> tick **Enforce HTTPS** once available (up to an hour;
   DNS propagation up to 24 hours)
8. Account Settings -> Pages -> **Verified domains** -> verify `acekapila.com`
   so no other GitHub account can attach it to a repo

No wildcard DNS records (`*.acekapila.com`) — takeover risk.

## When to outgrow this

Past about fifteen entries, maintaining the register by hand gets tedious.
That is the point to move to Jekyll, which GitHub Pages builds for free.
Not before — a build step you have to debug is a worse problem than a list
you have to edit.
