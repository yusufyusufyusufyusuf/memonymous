# Deploying the Memonymous backend (makes memoirs shared across all browsers)

Run these on **your computer** (they need your Cloudflare login). Takes ~5 minutes.
You need Node.js 18+ installed: https://nodejs.org

```bash
# 1. from the repo, go into the backend folder
cd backend

# 2. install dependencies
npm install

# 3. log in to Cloudflare (opens your browser to authorize)
npx wrangler login

# 4. create the database, then paste the printed database_id into wrangler.toml
#    (replace REPLACE_WITH_YOUR_D1_DATABASE_ID)
npx wrangler d1 create memoirs_db

# 5. create the tables
npx wrangler d1 migrations apply memoirs_db --remote

# 6. set the two secrets
#    MOD_API_KEY  = a password moderators use (pick something strong)
#    GLOBAL_PEPPER = a long random string (used to hash reporter IPs)
npx wrangler secret put MOD_API_KEY
npx wrangler secret put GLOBAL_PEPPER

# 7. deploy
npx wrangler deploy
```

Step 7 prints a URL like:

```
https://anon-memoirs-backend.<your-subdomain>.workers.dev
```

**Send me that URL** and I'll connect the website to it, so a memoir published in
one browser shows up for everyone.
