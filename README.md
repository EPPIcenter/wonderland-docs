# wonderland-docs

This repository contains the user-facing documentation for the MAD4HATTER amplicon sequencing pipeline. The documentation is built with MkDocs and automatically deployed to GitHub Pages.

## Documentation

The documentation provides comprehensive guides for using the MAD4HATTER pipeline, including installation instructions, execution parameters, pipeline outputs, and configuration options for custom amplicon pools. The live documentation is available at [https://eppicenter.github.io/wonderland-docs/](https://eppicenter.github.io/wonderland-docs/).

## For Developers

This documentation site is built with [MkDocs](https://www.mkdocs.org/) and the [Material theme](https://squidfunk.github.io/mkdocs-material/), and is automatically deployed to GitHub Pages via GitHub Actions.

### Prerequisites

- Python 3.x
- pip

### Local Development Setup

1. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

2. **Preview the site locally:**
   ```bash
   mkdocs serve
   ```
   The site will be available at `http://127.0.0.1:8000/`

3. **Build the site (without serving):**
   ```bash
   mkdocs build
   ```
   The built site will be in the `site/` directory (which is gitignored).

### Documentation Structure

- **Documentation source files**: Located in the `docs/` directory
  - Main pages: `docs/*.md`
  - Assets (images, CSS): `docs/assets/`
- **Configuration**: `mkdocs.yml` in the repository root
- **Custom styling**: `docs/assets/css/custom.css`

### Adding or Editing Documentation

1. **Add a new page:**
   - Create a new Markdown file in `docs/` or an appropriate subdirectory
   - Add it to the `nav` section in `mkdocs.yml`

2. **Edit an existing page:**
   - Edit the corresponding `.md` file in `docs/`
   - Use `mkdocs serve` to preview changes in real-time

3. **Update navigation:**
   - Edit the `nav` section in `mkdocs.yml`
   - Follow the existing structure and indentation

4. **Add images:**
   - Place images in `docs/assets/images/`
   - Reference them using relative paths: `![Alt text](assets/images/filename.png)`

### Writing Guidelines

- Use clear, concise language
- Include code examples where helpful
- Use MkDocs Material features:
  - Admonitions (notes, warnings, tips) using `!!! note`, `!!! warning`, etc.
  - Code blocks with syntax highlighting
  - Tables for structured information
  - Links to other documentation pages

### Deployment

The documentation is automatically deployed to GitHub Pages when changes are pushed to the `main` or `master` branches.

The deployment is handled by the GitHub Actions workflow in `.github/workflows/docs.yml`.

#### GitHub Pages Configuration

1. Go to repository Settings → Pages
2. Under "Source", select **"GitHub Actions"** (not "Deploy from a branch")
3. The workflow will automatically deploy on every push to the configured branches

### Testing Changes

Before committing documentation changes:

1. Run `mkdocs serve` and review all changes in the browser
2. Check that all links work correctly
3. Verify images display properly
4. Ensure the navigation structure makes sense
5. Test on different screen sizes if possible

### Dependencies

Documentation dependencies are listed in `requirements.txt`:
- `mkdocs>=1.5.0`
- `mkdocs-material>=9.0.0`
- `mkdocs-git-revision-date-localized-plugin>=1.2.0`
- `pymdown-extensions>=10.0`
- `mkdocs-minify-plugin>=0.7.0`
- `mike>=1.1.0`

## Contributing

When contributing to the documentation:

1. Make changes in a feature branch
2. Test locally with `mkdocs serve`
3. Submit a pull request
4. Documentation will be automatically deployed once merged to the main branch
