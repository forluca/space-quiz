# Orbit: Space exploration quiz

Orbit is a ten-question quiz about our solar system, space exploration, and
the universe. Each round shuffles the questions and answer choices, gives
immediate feedback with a space fact, and ends with a score and an option to
play again.

The app includes responsive layouts, light and dark themes, keyboard-accessible
buttons, and reduced-motion support. Everything is contained in `index.html`,
including styles, JavaScript, and illustrations; there are no external assets
or dependencies.

## Run locally

Clone or download this repository, then open `index.html` directly in a modern
web browser. No installation, build step, or local server is needed.

Choose an answer, select **Next question**, and select **See results** after
the tenth question. Use **Explore again** to start a new round.

## GitHub Pages

Site URL: <https://forluca.github.io/space-quiz/>

The workflow in `.github/workflows/pages.yml` prepares and uploads only
`index.html` as the site artifact. Pull requests targeting `main` run the
**Prepare static site** check without deploying. Pushes to `main` deploy the
site automatically. The workflow can also be run manually on `main` from
the repository's **Actions** tab.

In **Settings > Pages > Build and deployment**, the source must be set to
**GitHub Actions**. Deployment uses the `github-pages` environment and the
workflow's scoped `pages: write` and `id-token: write` permissions.
