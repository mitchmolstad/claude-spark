---
name: deploy-and-verify
description: Build and deploy the app to production, then verify it works
disable-model-invocation: true
---

Build and deploy the app to production, then verify it works.

## Steps

### Phase 1: Git Sync (Before Deploy)

1. Run `git status` to show uncommitted changes. If there are changes:
   - Stage all relevant files
   - Commit with a descriptive message using this format:

     ```
     git commit -m "$(cat <<'EOF'
     [Brief description of what changed]

     Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
     EOF
     )"
     ```

   - Push to GitHub: `git push`

2. If no changes to commit, proceed to Phase 2.

### Phase 2: Build & Deploy

> [CUSTOMIZE] Replace the commands below with your project's build and deploy steps.
> Examples for common platforms:

**Vercel:**
```bash
# Vercel auto-deploys from git push — just verify the deploy status
npx vercel --prod
```

**Netlify:**
```bash
npx netlify deploy --prod --dir=dist
```

**Cloudflare Pages:**
```bash
npm run build
npx wrangler pages deploy dist --project-name=[YOUR_PROJECT]
```

**Cloudflare Workers:**
```bash
npx wrangler deploy
```

**Railway:**
```bash
# Railway auto-deploys from git push — just verify the deploy status
railway status
```

**Custom / Docker:**
```bash
docker build -t [YOUR_APP] .
docker push [YOUR_REGISTRY]/[YOUR_APP]:latest
# Trigger deploy via your platform's CLI or API
```

3. Wait 10-15 seconds for deployment to propagate.

### Phase 3: Verify

> [CUSTOMIZE] Replace with your app's verification steps.
> At minimum, verify the deployed URL loads and core functionality works.

4. Use Playwright (or curl) to verify the production URL loads.

5. Verify core functionality:
   - Does the main page load without errors?
   - Can a user complete the primary action (login, submit form, etc.)?
   - Are API endpoints responding?

6. Take a screenshot if using Playwright for evidence.

### Phase 4: Report

7. Report results:
   - Git sync status (committed hash or "no changes")
   - Deploy status (success/failure for each service)
   - Verification status (what worked, what failed)
   - Screenshot if available
