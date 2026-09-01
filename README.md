# 🏴‍☠️ LuffyTV Player

A fast, dark-themed anime player frontend that uses the [LuffyTV Anime Stream API](https://github.com/fahadulalim93-cloud/luffytv-miruro-api) to fetch m3u8 stream URLs and play them with HLS.js.

Hosted at **[luffytv.live](https://luffytv.live)**.

## ✨ Features

- 🎬 **Single-page app** — instant navigation, no page reloads
- ⚡ **HLS.js playback** — plays m3u8 streams in any modern browser
- 🔄 **Multi-server fallback** — if Vidstream fails, auto-tries HD-1, HD-2, VidPlay-1
- 🌐 **Sub/Dub toggle** — switch audio language with one click
- 📋 **Episode browser** — quickly jump between episodes
- 🔍 **Search** — find anime by name
- 🎨 **Dark theme** matching modern streaming aggregators
- 🏷️ **Pretty server names** (Koto, Aegis, Rin, Neko, etc.) for clean UX

## 🎯 Server Name Mapping

| Raw API Name | Display Name | Provider |
|---|---|---|
| Vidstream-2 | Koto | megaplay.buzz (HLS) |
| HD-1 | Aegis | megaplay.buzz (HLS) |
| HD-2 | Rin | megaplay.buzz (HLS) |
| VidPlay-1 | Neko | vidtube.site (EMBED) |
| VidPlay-2 | Aris | vidtube.site (EMBED) |

Edit `SERVER_DISPLAY_NAMES` in `index.html` to customize.

## 🚀 Deploy to luffytv.live

### Option A: Nginx on your VPS

```bash
# Clone the repo
cd /var/www
git clone https://github.com/fahadulalim93-cloud/luffytv-player

# Nginx config
sudo tee /etc/nginx/sites-available/luffytv.live << 'EOF'
server {
    listen 80;
    server_name luffytv.live www.luffytv.live;
    root /var/www/luffytv-player;
    index index.html;
    
    location / {
        try_files $uri $uri/ /index.html;
    }
}
EOF

sudo ln -s /etc/nginx/sites-available/luffytv.live /etc/nginx/sites-enabled/
sudo certbot --nginx -d luffytv.live -d www.luffytv.live
sudo systemctl reload nginx
```

### Option B: Vercel (free, fast CDN)

1. Push this folder to GitHub
2. Go to https://vercel.com → New Project → Import the repo
3. Set custom domain to `luffytv.live`
4. Done — Vercel auto-deploys on every push

### Option C: Cloudflare Pages

1. Push to GitHub
2. Go to https://dash.cloudflare.com → Pages → Create project
3. Connect GitHub repo
4. Build command: (none — static HTML)
5. Output directory: `/`
6. Add custom domain `luffytv.live`

## 🔌 API Configuration

The player expects the API at `https://api.luffytv.live`. To change it:

1. Edit `index.html`
2. Find: `const API_BASE = window.LUFFYTV_API || 'https://api.luffytv.live';`
3. Replace with your API URL (e.g. `http://169.58.120.196:8080` for direct VPS access)

Or set it dynamically via JS before the script loads:
```html
<script>window.LUFFYTV_API = 'https://your-api.com';</script>
```

## 📋 Required API Endpoints

This player uses these endpoints from the LuffyTV anime-stream-api:

| Endpoint | Purpose |
|---|---|
| `GET /full/{slug}/{episode}` | One-shot: anime + episode + m3u8 URL |
| `GET /m3u8/{link_id}` | Direct m3u8 URL for a specific server |
| `GET /episodes/{anime_id}` | Episode list for the anime |
| `GET /search?q={query}` | Search anime |

## 🎨 Theme Customization

All colors are CSS variables — edit `:root` in `index.html`:

```css
:root {
  --bg: #000;
  --accent: #FF4D6A;   /* "YOU ARE WATCHING" + active states */
  --cyan: #00E5FF;     /* HLS section header */
  --purple: #A855F7;   /* EMBED section header */
}
```

## 📁 File Structure

```
luffytv-player/
├── index.html    # Single-file app (HTML + CSS + JS)
└── README.md     # This file
```

## 🔗 Related Repos

- **API**: https://github.com/fahadulalim93-cloud/luffytv-miruro-api
- **Player**: https://github.com/fahadulalim93-cloud/luffytv-player (this repo)

## 📝 URL Routes

| Path | Page |
|---|---|
| `/` | Home page with search |
| `/watch/{slug}/{episode}` | Watch page (e.g. `/watch/bleach-yaa9n/1`) |

## 💡 Usage Examples

Direct URLs you can visit:
- `https://luffytv.live/` — home
- `https://luffytv.live/watch/bleach-yaa9n/1` — Bleach Episode 1
- `https://luffytv.live/watch/demon-slayer-kimetsu-no-yaiba-rzepv/1` — Demon Slayer Episode 1
- `https://luffytv.live/watch/jujutsu-kaisen-tv-8ssye/1` — Jujutsu Kaisen Episode 1
