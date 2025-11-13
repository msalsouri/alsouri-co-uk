# DNS Configuration Guide for www.alsouri.org SSL Fix

## Current Problem
Your `www.alsouri.org` subdomain shows as "not secure" because the DNS creates a loop that prevents GitHub Pages from issuing an SSL certificate.

## Current DNS Configuration (INCORRECT)
```
www    CNAME    3600    alsouri.org.
```

This causes `www.alsouri.org` → `alsouri.org` → GitHub Pages IPs
GitHub Pages cannot issue a certificate for `www` in this configuration.

## Required DNS Change

### In Your Gandi DNS Panel:

**CHANGE THIS RECORD:**
```
Name: www
Type: CNAME
TTL: 3600
Value: alsouri.org.    ← CHANGE THIS
```

**TO:**
```
Name: www
Type: CNAME
TTL: 3600
Value: msalsouri.github.io.    ← NEW VALUE
```

## Step-by-Step Instructions

### 1. Update DNS Record in Gandi
1. Log in to Gandi
2. Navigate to: `alsouri.org` → DNS Records
3. Find the record: `www CNAME 3600 alsouri.org.`
4. Click Edit
5. Change the value from `alsouri.org.` to `msalsouri.github.io.`
6. Save the change

### 2. GitHub Pages Configuration (ALREADY DONE ✓)
- Updated `CNAME` file in `alsouri-org` repository to `www.alsouri.org`
- Pushed to GitHub: commit `8edb733`
- GitHub Pages will now recognize and serve `www.alsouri.org`

### 3. Enable HTTPS in GitHub (After DNS Propagation)
1. Wait 10-60 minutes for DNS propagation
2. Go to GitHub repository: `msalsouri/alsouri-org`
3. Settings → Pages
4. Once "HTTPS" shows a green checkmark, enable "Enforce HTTPS"

### 4. Verify the Fix
After DNS propagation (usually 10-60 minutes):
- Test: `https://www.alsouri.org` - should show lock icon
- Test: `https://alsouri.org` - should continue working
- Test: `https://shop.alsouri.org` - GoDaddy store (unchanged)

## What Each Subdomain Does Now

| Subdomain | Points To | Purpose |
|-----------|-----------|---------|
| `alsouri.org` | GitHub Pages IPs (A records) | Main website |
| `www.alsouri.org` | `msalsouri.github.io.` | Main website (with SSL) |
| `shop.alsouri.org` | `cdrapplication.secureserver.net.` | GoDaddy store (unchanged) |

## Why This Works

1. **Apex domain (`alsouri.org`)**: Uses A records pointing directly to GitHub Pages IPs
2. **WWW subdomain (`www.alsouri.org`)**: Uses CNAME pointing to your GitHub Pages custom domain
3. **Shop subdomain (`shop.alsouri.org`)**: Keeps its own GoDaddy infrastructure and SSL

This configuration allows GitHub Pages to:
- Recognize both `alsouri.org` and `www.alsouri.org` as valid custom domains
- Issue separate SSL certificates for each
- Serve the same content from both URLs with proper HTTPS

## Troubleshooting

### If SSL doesn't provision after 1 hour:
1. Check DNS propagation: `dig www.alsouri.org CNAME`
   - Should show: `www.alsouri.org. 3600 IN CNAME msalsouri.github.io.`
2. Check GitHub Pages settings → Custom domain
3. Try removing and re-adding the custom domain in GitHub Pages
4. Wait another 30 minutes

### If you see certificate errors:
- Clear browser cache
- Try incognito/private mode
- Check on different device/network

## Next Steps (ALREADY COMPLETED ✓)

- ✅ Updated `CNAME` file in `alsouri-org` repository
- ✅ Deployed bookings page to `alsouri.co.uk`
- ✅ Added `/Bookings/` navigation throughout site
- ⏳ **YOUR ACTION NEEDED**: Update DNS record in Gandi panel
- ⏳ **WAIT**: DNS propagation (10-60 minutes)
- ⏳ **FINAL STEP**: Enable "Enforce HTTPS" in GitHub Pages

## Support
If you encounter any issues after making the DNS change, check:
- GitHub Pages status: https://www.githubstatus.com
- DNS propagation: https://dnschecker.org/#CNAME/www.alsouri.org
