# Next Steps for the Doula Landing Page

Here is a guide to getting this landing page live on the internet while maintaining separation from your personal GitHub account.

## 1. Keeping it Anonymous (Separate from your personal GitHub)

If you host this on your personal GitHub (e.g., `github.com/your-personal-username`), people can easily inspect the site's source code or network requests and trace it back to your profile. To avoid this:

**Option A: Create a New GitHub Account (Recommended for maximum separation)**
1. Create a brand new email address (or use the doula's email).
2. Sign up for a new GitHub account using that email (e.g., `[doula-brand]-web`).
3. Host the repository there.

**Option B: Create a GitHub Organization**
1. From your current GitHub account, create a new Organization (e.g., `[doula-brand]`).
2. You can create repositories under this Org. While your personal account is an admin, the repository URL will be `github.com/[doula-brand]/...` and commits can be made without directly exposing your personal username if configured carefully, but Option A is much safer for complete anonymity.

## 2. Setting up the Repository

Once you have decided on the account/organization:
1. Create a new repository on GitHub named `[doula-brand].github.io` (or just `landing-page`).
2. We have already initialized the code locally. Push it to the new remote:
   ```bash
   # Make sure you are in the directory
   cd /home/hassan/doula-landing-page
   
   # Add and commit the files
   git add .
   git commit -m "Initial commit of landing page"
   
   # Link to the new GitHub repo and push
   git branch -M main
   git remote add origin https://github.com/YOUR_NEW_ACCOUNT/YOUR_REPO_NAME.git
   git push -u origin main
   ```

## 3. Enabling GitHub Pages

1. Go to the repository settings on GitHub.
2. Find the "Pages" section in the left sidebar.
3. Under "Build and deployment", set the source to "Deploy from a branch".
4. Select the `main` branch and the `/ (root)` folder, then save.
5. GitHub will provide a URL (like `https://new-account.github.io/repo-name/`).

## 4. Setting up a Custom Domain via Cloudflare

Cloudflare provides a great layer of performance, security, and DNS management.

1. **Purchase a domain name** (if not already done) via Namecheap, Google Domains, Cloudflare Registrar, etc.
2. **Create a Cloudflare account** and click "Add Site". Enter your custom domain name.
3. Cloudflare will scan for existing DNS records. Follow the prompts to update your domain's Nameservers at your registrar to point to Cloudflare's provided nameservers.
4. **Configure DNS for GitHub Pages in Cloudflare:**
   - Go to the DNS tab in Cloudflare.
   - Delete any existing `A` or `CNAME` records for the root (`@`) and `www`.
   - Add four `A` records for the root (`@`) pointing to GitHub's IPs:
     - `185.199.108.153`
     - `185.199.109.153`
     - `185.199.110.153`
     - `185.199.111.153`
   - Add a `CNAME` record for `www` pointing to `your-github-account.github.io`.
   - **Crucial:** Make sure the Cloudflare "Proxy status" is active (the orange cloud icon) for these records. This hides GitHub's IPs behind Cloudflare and handles SSL.
5. **Configure GitHub Pages Custom Domain:**
   - Go back to your GitHub repository Settings > Pages.
   - In the "Custom domain" field, enter your purchased domain (e.g., `www.doulabrand.com`).
   - GitHub will create a `CNAME` file in your repository and attempt to verify the domain.

## 5. Editing the Content

The files are simple HTML and CSS. You can open `index.html` in any text editor to change the text, replace `[Placeholders]`, and add actual images. 

To see changes locally, just double-click `index.html` in your file browser to open it in Chrome/Firefox.
