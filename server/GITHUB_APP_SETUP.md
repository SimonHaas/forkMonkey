# ForkMonkey GitHub App Setup Guide

## Overview

This guide walks you through creating and configuring a GitHub App for the ForkMonkey "Full Trust" adoption flow. The app will allow users to authorize ForkMonkey to automatically create and configure their monkey repository.

---

## Step 1: Create the GitHub App

1. Go to **GitHub Settings** → **Developer settings** → **GitHub Apps**
   
   Direct link: https://github.com/settings/apps/new

2. Fill in the following details:

### Basic Information

| Field | Value |
|-------|-------|
| **GitHub App name** | `ForkMonkey Adopter` |
| **Description** | `Automatically sets up your ForkMonkey repository with GitHub Actions and Pages enabled.` |
| **Homepage URL** | `https://roeiba.github.io/forkMonkey/` |

### Identifying and authorizing users

| Field | Value |
|-------|-------|
| **Callback URL** | `https://YOUR-BACKEND-URL/api/adopt/oauth/callback` |
| **Request user authorization (OAuth) during installation** | ✅ Checked |
| **Enable Device Flow** | ❌ Unchecked |

> ⚠️ **Replace `YOUR-BACKEND-URL`** with your actual backend URL once deployed (e.g., `https://forkmonkey-api.vercel.app`)

### Post installation

| Field | Value |
|-------|-------|
| **Setup URL (optional)** | Leave empty |
| **Redirect on update** | ❌ Unchecked |

### Webhook

| Field | Value |
|-------|-------|
| **Active** | ❌ Unchecked |
| **Webhook URL** | Leave empty |

---

## Step 2: Set Permissions

Under **Repository permissions**, set:

| Permission | Access Level |
|------------|--------------|
| **Actions** | Read and write |
| **Administration** | Read and write |
| **Contents** | Read and write |
| **Metadata** | Read-only |
| **Pages** | Read and write |
| **Workflows** | Read and write |

Under **Account permissions**, set:

| Permission | Access Level |
|------------|--------------|
| **Email addresses** | Read-only (optional, for contact) |

---

## Step 3: Where can this app be installed?

Select: **Any account**

This allows any GitHub user to install and use the app.

---

## Step 4: Create the App

Click **Create GitHub App** at the bottom.

---

## Step 5: Collect Your App Credentials

After creation, you'll be on the app's settings page. Collect these values:

### App ID
Found at the top of the page, looks like: `123456`

### Client ID  
Under "About" section, looks like: `Iv1.abc123...`

### Client Secret
Click **Generate a new client secret** and copy it immediately (shown only once).

### Private Key
Scroll down and click **Generate a private key**. A `.pem` file will download.

---

## Step 6: Configure Your Backend

Add these to your `server/.env` file:

```bash
# GitHub App Configuration
GITHUB_APP_ID=YOUR_APP_ID
GITHUB_APP_CLIENT_ID=Iv1.YOUR_CLIENT_ID
GITHUB_APP_CLIENT_SECRET=YOUR_CLIENT_SECRET

# Private key (paste the entire content, or path to .pem file)
GITHUB_PRIVATE_KEY="-----BEGIN RSA PRIVATE KEY-----
...your private key content...
-----END RSA PRIVATE KEY-----"
```

---

## Step 7: Upload App Logo (Optional)

1. Go to your app settings
2. Under "Display information", upload a logo
3. Recommended: Use the ForkMonkey logo (400x400 PNG)

---

## Step 8: Install the App on Your Account

1. Go to your app's public page:
   `https://github.com/apps/forkmonkey-adopter`
   
2. Click **Install**

3. Choose **All repositories** or select specific repos

4. Click **Install**

---

## Summary of Values Needed

After completing setup, you'll have:

| Value | Example | Where It Goes |
|-------|---------|---------------|
| App ID | `123456` | `GITHUB_APP_ID` |
| Client ID | `Iv1.abc123def456` | `GITHUB_APP_CLIENT_ID` |
| Client Secret | `secret123...` | `GITHUB_APP_CLIENT_SECRET` |
| Private Key | `.pem` file contents | `GITHUB_PRIVATE_KEY` |

---

## Testing the App

Once configured:

1. Start your backend server
2. Visit your ForkMonkey site
3. Click "Adopt Your Monkey!" 
4. Select "Full Trust"
5. Complete customization
6. Click Continue → You'll be redirected to GitHub authorization
7. Authorize the app
8. Your monkey repository will be created automatically!

---

## Troubleshooting

### "OAuth not configured" error
- Ensure `GITHUB_APP_CLIENT_ID` is set in your `.env`
- Restart your backend server after adding environment variables

### Callback URL mismatch
- The callback URL in your app settings must exactly match what your backend expects
- For local testing, you can temporarily set it to `http://localhost:5000/api/adopt/oauth/callback`

### Permission denied errors
- Ensure you've granted all required permissions in Step 2
- The user may need to re-authorize if permissions changed
