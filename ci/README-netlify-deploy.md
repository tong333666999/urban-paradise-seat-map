# Enable GitHub → Netlify auto-deploy

Secret `NETLIFY_AUTH_TOKEN` is already set on this repo.

The OAuth token used by automation lacks the `workflow` scope, so
`.github/workflows/netlify-deploy.yml` could not be pushed yet.

To enable pushes to `main` to deploy production:

1. Grant workflow scope locally: `gh auth refresh -h github.com -s repo,workflow`
2. Copy this file into place and push:
   ```bash
   mkdir -p .github/workflows
   cp ci/netlify-deploy.yml .github/workflows/netlify-deploy.yml
   git add .github/workflows/netlify-deploy.yml
   git commit -m "ci: Netlify production deploy on push to main"
   git push origin main
   ```

Or create the workflow in the GitHub UI (Actions → New workflow) using
`ci/netlify-deploy.yml` as the contents.

Until then, production can be updated with:
`npx netlify-cli deploy --dir=. --prod --site=4c0fb197-469b-459b-a163-0c31167b67da`
(with `NETLIFY_AUTH_TOKEN` set). Netlify build hooks alone fail because
the site has no GitHub App install / deploy key (`installation_id` null).
