# OAuth Setup Guide for construyelo.cl

This guide will help you set up OAuth authentication for your deployed site at https://construyelo.cl/

## Prerequisites
- Access to your GitHub repository
- Google Cloud Console account
- GitHub account with admin access

---

## Part 1: Google OAuth Setup

### Step 1: Create Google OAuth App
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project or select existing one
3. Navigate to **APIs & Services** → **Credentials**
4. Click **+ CREATE CREDENTIALS** → **OAuth 2.0 Client ID**
5. Configure the OAuth consent screen if not already done
6. Select **Web application** as application type

### Step 2: Configure Authorized URLs
**Authorized JavaScript origins:**
```
https://construyelo.cl
```

**Authorized redirect URIs:**
```
https://construyelo.cl/api/auth/callback/google
```

### Step 3: Save Your Credentials
After creating, you'll receive:
- `GOOGLE_CLIENT_ID` (looks like: xxxxx.apps.googleusercontent.com)
- `GOOGLE_CLIENT_SECRET` (keep this secret!)

---

## Part 2: GitHub OAuth Setup (User Login)

### Step 1: Create GitHub OAuth App
1. Go to [GitHub Settings](https://github.com/settings/developers)
2. Click **OAuth Apps** → **New OAuth App**
3. Fill in the details:
   - **Application name:** `Construyelo - Login`
   - **Homepage URL:** `https://construyelo.cl`
   - **Authorization callback URL:** `https://construyelo.cl/api/auth/callback/github`
4. Click **Register application**

### Step 2: Save Your Credentials
- `GITHUB_CLIENT_ID`
- `GITHUB_CLIENT_SECRET` (generate one if needed)

---

## Part 3: GitHub Export OAuth Setup (Code Export Feature)

### Step 1: Create Separate GitHub OAuth App
1. Go to [GitHub Settings](https://github.com/settings/developers)
2. Click **OAuth Apps** → **New OAuth App**
3. Fill in the details:
   - **Application name:** `Construyelo - Export`
   - **Homepage URL:** `https://construyelo.cl`
   - **Authorization callback URL:** `https://construyelo.cl/api/export/github/callback`
4. Click **Register application**

### Step 2: Save Your Credentials
- `GITHUB_EXPORTER_CLIENT_ID`
- `GITHUB_EXPORTER_CLIENT_SECRET`

---

## Part 4: Configure Your Repository

### Step 1: Clone Your Repository Locally
```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME
```

### Step 2: Install Dependencies
```bash
bun install
# or
npm install --legacy-peer-deps
```

### Step 3: Create Environment Files

Create `.dev.vars` (for local testing):
```bash
# Google OAuth
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# GitHub OAuth (Login)
GITHUB_CLIENT_ID="your-github-client-id"
GITHUB_CLIENT_SECRET="your-github-client-secret"

# GitHub Export OAuth
GITHUB_EXPORTER_CLIENT_ID="your-github-exporter-client-id"
GITHUB_EXPORTER_CLIENT_SECRET="your-github-exporter-client-secret"

# Keep these from your existing .dev.vars
JWT_SECRET="your-existing-jwt-secret"
WEBHOOK_SECRET="your-existing-webhook-secret"
GOOGLE_AI_STUDIO_API_KEY="your-google-ai-api-key"
```

Create `.prod.vars` (for deployment):
```bash
# Copy the same OAuth credentials as .dev.vars
# This file is used for production deployment

# Google OAuth
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# GitHub OAuth (Login)
GITHUB_CLIENT_ID="your-github-client-id"
GITHUB_CLIENT_SECRET="your-github-client-secret"

# GitHub Export OAuth
GITHUB_EXPORTER_CLIENT_ID="your-github-exporter-client-id"
GITHUB_EXPORTER_CLIENT_SECRET="your-github-exporter-client-secret"

# Production secrets
JWT_SECRET="your-production-jwt-secret"
WEBHOOK_SECRET="your-production-webhook-secret"
GOOGLE_AI_STUDIO_API_KEY="your-google-ai-api-key"
```

---

## Part 5: Deploy to Cloudflare

### Step 1: Test Locally (Optional)
```bash
bun run dev
# or
npm run dev
```

Visit http://localhost:5000 and test the login functionality

### Step 2: Deploy to Production
```bash
bun run deploy
```

This will:
- Build your frontend
- Deploy to Cloudflare Workers
- Apply database migrations
- Update your live site at construyelo.cl

### Step 3: Verify Deployment
1. Visit https://construyelo.cl
2. Click "Sign In" button
3. Try logging in with Google or GitHub
4. Test the export functionality

---

## Troubleshooting

### "OAuth callback mismatch" Error
- Double-check that your callback URLs exactly match what you configured in Google/GitHub
- Make sure you're using `https://` not `http://`
- Clear your browser cache

### "Invalid client" Error
- Verify your CLIENT_ID and CLIENT_SECRET are correct
- Make sure you copied them correctly (no extra spaces)
- Check that `.prod.vars` has the production secrets

### Login Button Doesn't Appear
- Make sure the OAuth credentials are in `.prod.vars`
- Redeploy with `bun run deploy`
- Check browser console for errors

---

## Security Notes

⚠️ **Never commit these files to Git:**
- `.dev.vars`
- `.prod.vars`

They should already be in `.gitignore`, but double-check!

✅ **Keep your secrets safe:**
- Don't share CLIENT_SECRET values
- Rotate secrets if they're ever exposed
- Use different secrets for dev and production

---

## Summary Checklist

- [ ] Google OAuth app created
- [ ] GitHub Login OAuth app created
- [ ] GitHub Export OAuth app created
- [ ] `.dev.vars` file created with all credentials
- [ ] `.prod.vars` file created with all credentials
- [ ] Tested locally (optional)
- [ ] Deployed to Cloudflare with `bun run deploy`
- [ ] Verified login works on construyelo.cl
- [ ] Verified export works on construyelo.cl

---

## Next Steps

After OAuth is working:
1. Test creating apps on construyelo.cl
2. Test exporting apps to GitHub
3. Monitor for any authentication issues
4. Consider adding more OAuth providers if needed

Need help? Check the [Cloudflare Workers documentation](https://developers.cloudflare.com/workers/) or the [VibeSDK repository](https://github.com/cloudflare/vibesdk).
