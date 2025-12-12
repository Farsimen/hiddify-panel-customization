# Hiddify Panel UI Customization

> Remove Hiddify branding, footer links, and social media icons without rebuilding the entire panel.

## Problem

The Hiddify Panel displays:
- Hiddify branding and version number
- Footer links (GitHub, Telegram, Help, etc.)
- Social media icons

These elements cannot be removed through the admin panel settings, and rebuilding the entire panel is impractical for small customizations.

## Solutions Provided

This repository contains two approaches to hide/remove these elements:

### Method 1: Nginx Reverse Proxy with CSS Injection
**Best for**: Direct server control, no external dependencies
- Uses Nginx `sub_filter` to inject CSS
- Hides Hiddify branding and footer elements via `display: none`
- No rebuilding required
- Works with any Hiddify version

### Method 2: Cloudflare Worker
**Best for**: Using Cloudflare DNS/CDN
- Intercepts HTML responses
- Injects CSS rules to hide unwanted elements
- No server-side changes needed
- Works with Cloudflare Free tier

## Quick Start

### Using Nginx (Recommended for Ubuntu)

1. **Backup current config**:
   ```bash
   sudo cp /etc/nginx/sites-available/hiddify /etc/nginx/sites-available/hiddify.bak
   ```

2. **Copy customization config**:
   ```bash
   curl -O https://raw.githubusercontent.com/Farsimen/hiddify-panel-customization/main/nginx/hiddify-customized.conf
   sudo cp hiddify-customized.conf /etc/nginx/sites-available/hiddify
   ```

3. **Test and apply**:
   ```bash
   sudo nginx -t
   sudo systemctl restart nginx
   ```

### Using Cloudflare Worker

1. Go to Cloudflare Dashboard → Workers → Create Worker
2. Copy code from `cloudflare-worker.js`
3. Deploy and enjoy!

## File Structure

```
.
├── nginx/
│   ├── hiddify-customized.conf     # Complete Nginx config with CSS injection
│   └── css-injection-snippet.conf  # Just the CSS injection part
├── cloudflare/
│   ├── worker.js                   # Cloudflare Worker script
│   └── deployment-guide.md         # Step-by-step deployment
├── styles/
│   └── hide-branding.css           # CSS rules to hide elements
└── README.md
```

## What Gets Hidden

- ✅ Hiddify logo/branding
- ✅ Version number (11.0.13, etc.)
- ✅ Footer links (GitHub, Telegram, etc.)
- ✅ Social media icons
- ✅ Help/Support menu items

## Safety & Compatibility

- ✅ Non-destructive (CSS-based, reversible)
- ✅ No panel modifications needed
- ✅ No additional dependencies
- ✅ Works with Hiddify Manager v11.x
- ✅ No performance impact

## Support

For issues or suggestions, open an issue on GitHub.

## License

MIT License - Use freely in your projects
