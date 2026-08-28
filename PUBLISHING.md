# Publishing Guide

This guide explains how to publish `django-cron-django5` to PyPI using [uv](https://docs.astral.sh/uv/).

## Prerequisites

1. Install [uv](https://docs.astral.sh/uv/installation/).

2. Create accounts on:
   - [PyPI](https://pypi.org/) (production)
   - [TestPyPI](https://test.pypi.org/) (for testing)

3. Create an API token on PyPI (and TestPyPI if you will upload there). PyPI no longer accepts username/password uploads.

   Store the token in `.env` at the repo root (gitignored):

   ```bash
   UV_PUBLISH_TOKEN=pypi-...
   ```

   Load it into your shell before any `uv publish` command:

   ```bash
   set -a && source .env && set +a
   ```

   `uv publish` reads `UV_PUBLISH_TOKEN` automatically. Do not pass `--token` on the command line.

## Building the Package

1. Clean previous builds:

   ```bash
   rm -rf dist build *.egg-info django_cron_django5.egg-info
   ```

2. Build the sdist and wheel:

   ```bash
   uv build --no-sources
   ```

   Artifacts are written to `dist/`. `--no-sources` ensures the build does not depend on local `tool.uv.sources` overrides.

## Publishing to TestPyPI (Recommended First)

TestPyPI is configured as a named index in `pyproject.toml`. Upload with:

```bash
set -a && source .env && set +a
uv publish --index testpypi
```

Use a TestPyPI token in `.env` for this step (`UV_PUBLISH_TOKEN` from test.pypi.org), not the production PyPI token.

### Testing the TestPyPI Package

Install from TestPyPI to verify. `--extra-index-url` is needed because dependencies (like Django) are on the main PyPI:

```bash
uv pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ django-cron-django5
```

Or import-check without using the local project:

```bash
uv run --with django-cron-django5 --no-project --refresh-package django-cron-django5 -- python -c "import django_cron"
```

## Version Management

Before publishing a new version:

1. Bump the version in `pyproject.toml` with uv:

   ```bash
   uv version --bump patch    # 0.6.2 -> 0.6.3
   # or: uv version --bump minor
   # or: uv version 0.7.0
   ```

2. Keep `django_cron/__init__.py` (`__version__`) in sync with `pyproject.toml`.

3. Update the changelog/release notes.

4. Commit the changes:

   ```bash
   git add .
   git commit -m "Release version X.Y.Z"
   git tag vX.Y.Z
   git push origin master --tags
   ```

5. Build and publish.

## Publishing to PyPI

```bash
uv build --no-sources
set -a && source .env && set +a
uv publish
```

`uv publish` uploads `dist/` to PyPI by default, using `UV_PUBLISH_TOKEN` from `.env`.

## Verifying the Published Package

After publishing, verify the package page on PyPI:
- https://pypi.org/project/django-cron-django5/

Test installation:

```bash
uv add django-cron-django5
```

or:

```bash
pip install django-cron-django5
```

## Troubleshooting

### "File already exists" error
This happens when you try to upload a version that already exists on PyPI. You must increment the version number. `uv publish` will skip files that are identical to ones already on PyPI, so you can retry the same command after a partial upload.

### Authentication errors
- Confirm `.env` contains `UV_PUBLISH_TOKEN` and you ran `set -a && source .env && set +a` in the same shell
- Use a PyPI API token, not a password
- Check if 2FA is enabled (tokens are required)
- For GitHub Actions, prefer [Trusted Publishing](https://docs.pypi.org/trusted-publishers/) instead of storing a token in `.env` or secrets files

### Package not found after upload
- Wait a few minutes for PyPI to index the package
- Refresh uv's cache: `uv cache clean`

## Additional Resources

- [uv: Building and publishing a package](https://docs.astral.sh/uv/guides/package/)
- [uv: Using uv in GitHub Actions](https://docs.astral.sh/uv/guides/integration/github/)
- [Python Packaging User Guide](https://packaging.python.org/)
