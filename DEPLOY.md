# NLA Website — Deploy & Go Public

## Folder
```
nla-website/
├── index.html
├── DEPLOY.md
└── assets/
    ├── logo.png
    ├── showreel.mp4          (compressed 75 MB → 8.5 MB, 1080p, faststart)
    ├── showreel-poster.jpg   (poster frame, shows instantly before video loads)
    ├── project-render.jpg    (optimized from 4.1 MB PNG → 0.6 MB JPG)
    └── fonts/                (self-hosted Archivo + Cormorant Garamond)
```

## 1. Deploy to Vercel (2 minutes)
Option A — dashboard: vercel.com → Add New → Project → drag the `nla-website` folder in (or `vercel.com/new` → "Deploy without Git" is gone, so use CLI or Git).

Option B — CLI:
```bash
cd nla-website
npx vercel --prod
```

Option C — Git (recommended, same as the Timesheet app):
```bash
cd nla-website
git init && git add -A && git commit -m "NLA website v1"
gh repo create nla-website --private --source=. --push
```
Then import the repo in Vercel. Framework preset: **Other** (static). No build command, output dir = root.

## 2. Point the GoDaddy domain
In Vercel → Project → Settings → Domains → add `yourdomain.com` and `www.yourdomain.com`.
Give the IT admin these DNS records in GoDaddy:

| Type  | Name | Value                  |
|-------|------|------------------------|
| A     | @    | 76.76.21.21            |
| CNAME | www  | cname.vercel-dns.com   |

(Vercel shows the exact values on the Domains page — use those if different.)
HTTPS certificate is issued automatically once DNS propagates (5 min – 48 h).

## 3. Before going public — checklist
- [ ] Replace `info@example.com` and phone number in the CTA + footer (search `example.com` in index.html)
- [ ] Swap the 3 project cards' images when real renders are ready (`assets/project-render.jpg` is reused 3× with filters right now)
- [ ] Update `<meta name="description">` / OG tags if the tagline changes
- [ ] Add Google Search Console → verify domain → submit sitemap (single page: just submit the URL)
- [ ] Optional: add analytics — Vercel Analytics (Settings → Analytics → Enable) or a GA4 snippet

## Notes on the video
- Muted autoplay + loop + `playsinline` → plays on iPhone/Android without user tap
- Speaker button bottom-right toggles sound
- Pauses automatically when scrolled off-screen (saves battery/data)
- If you replace the video later, re-encode with:
  `ffmpeg -i in.mp4 -an -c:v libx264 -preset slow -crf 26 -vf scale=1920:-2 -pix_fmt yuv420p -movflags +faststart showreel.mp4`
