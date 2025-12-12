# Installation Guide - Hiddify Panel Customization

## Method 1: Nginx Reverse Proxy (Recommended for Ubuntu)

This method uses Nginx's `sub_filter` directive to inject CSS that hides Hiddify branding elements.

### Prerequisites
- Root or sudo access on your Ubuntu server
- Nginx installed and running
- Hiddify Panel running on localhost (port 8000 or configured port)

### Step-by-Step Instructions

#### 1. Backup Your Current Nginx Configuration

```bash
sudo cp /etc/nginx/sites-available/hiddify /etc/nginx/sites-available/hiddify.bak
```

#### 2. Edit Your Nginx Configuration

Open your Hiddify Panel Nginx configuration:

```bash
sudo nano /etc/nginx/sites-available/hiddify
```

Find your `location / {` block (or the relevant location block for your panel).

#### 3. Add CSS Injection Configuration

Add these lines inside your location block:

```nginx
# Enable HTML filtering
sub_filter_once off;
sub_filter_types text/html;

# Inject CSS to hide Hiddify branding
sub_filter '</head>' '<style>
    footer, 
    [class*="Hiddify"], 
    [class*="hiddify"], 
    [class*="footer"],
    [class*="social"], 
    a[href*="github"], 
    a[href*="telegram"],
    a[href*="youtube"], 
    [class*="version"] {
        display: none !important;
    }
    body, main, .container, .main-content {
        margin-bottom: 0 !important;
        padding-bottom: 0 !important;
    }
</style></head>';
```

#### 4. Validate Nginx Configuration

Before applying changes, test your configuration:

```bash
sudo nginx -t
```

You should see: `nginx: configuration file test is successful`

If there are errors, you can restore your backup:

```bash
sudo cp /etc/nginx/sites-available/hiddify.bak /etc/nginx/sites-available/hiddify
```

#### 5. Reload Nginx

Once the test passes, reload Nginx:

```bash
sudo systemctl reload nginx
```

Or restart it:

```bash
sudo systemctl restart nginx
```

#### 6. Clear Browser Cache

Open your Hiddify Panel in a browser and do a hard refresh:
- **Chrome/Firefox**: `Ctrl+Shift+Delete` or `Ctrl+F5`
- **Safari**: `Cmd+Shift+Delete` or `Cmd+R`

### Expected Results

After applying these changes, the following should be hidden:
- ✅ Hiddify logo/branding at the bottom
- ✅ Version number (11.0.13, etc.)
- ✅ Footer links (GitHub, Telegram, Help, etc.)
- ✅ Social media icons
- ✅ Support menu items

### Troubleshooting

**Issue**: Changes don't appear after reload
- Solution: Clear browser cache completely (Ctrl+Shift+Delete)
- Solution: Open panel in incognito/private window

**Issue**: Nginx fails to reload
- Check error: `sudo journalctl -u nginx -n 20`
- Restore backup: `sudo cp /etc/nginx/sites-available/hiddify.bak /etc/nginx/sites-available/hiddify`
- Reload: `sudo systemctl reload nginx`

**Issue**: Some elements still visible
- The CSS selectors might need adjustment based on your Hiddify version
- Add more specific selectors to `hide-branding.css` file

### Reverting Changes

To remove the customization and restore original Hiddify branding:

```bash
# Restore backup
sudo cp /etc/nginx/sites-available/hiddify.bak /etc/nginx/sites-available/hiddify

# Reload Nginx
sudo systemctl reload nginx

# Clear browser cache
```

## Method 2: Cloudflare Worker

For those using Cloudflare DNS/CDN, see `cloudflare/deployment-guide.md`

## Support

If you encounter issues, check:
1. Your Nginx error log: `sudo tail -f /var/log/nginx/error.log`
2. Hiddify Panel logs: Check your installation directory
3. Browser console: Press F12 and check for JavaScript errors
