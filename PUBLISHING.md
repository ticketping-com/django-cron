# Publishing Guide

This guide explains how to publish `django-cron-django5` to PyPI.

## Prerequisites

1. Create accounts on:
   - [PyPI](https://pypi.org/) (production)
   - [TestPyPI](https://test.pypi.org/) (for testing)

2. Install build tools (already done):
   ```bash
   pip install --upgrade build twine
   ```

## Building the Package

1. Clean previous builds:
   ```bash
   rm -rf dist build *.egg-info django_cron_django5.egg-info
   ```

2. Build the package:
   ```bash
   python -m build
   ```

3. Verify the build:
   ```bash
   python -m twine check dist/*
   ```

## Publishing to TestPyPI (Recommended First)

Test your package on TestPyPI before publishing to the real PyPI:

```bash
python -m twine upload --repository testpypi dist/*
```

You'll be prompted for your TestPyPI username and password.

### Testing the TestPyPI Package

Install from TestPyPI to verify:

```bash
pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ django-cron-django5
```

Note: `--extra-index-url` is needed because dependencies (like Django) are on the main PyPI.

## Version Management

Before publishing a new version:

1. Update the version number in:
   - `pyproject.toml` (line 3)
   - `setup.py` (line 17)
   - `django_cron/__init__.py` (line 8)

2. Update the changelog/release notes

3. Commit the changes:
   ```bash
   git add .
   git commit -m "Release version X.Y.Z"
   git tag vX.Y.Z
   git push origin master --tags
   ```

4. Build and publish

## Verifying the Published Package

After publishing, verify the package page on PyPI:
- https://pypi.org/project/django-cron-django5/

Test installation:
```bash
pip install django-cron-django5
```

## Troubleshooting

### "File already exists" error
This happens when you try to upload a version that already exists on PyPI. You must increment the version number.

### Authentication errors
- Verify your credentials
- Check if 2FA is enabled (must use API tokens if it is)
- Ensure `~/.pypirc` file has correct permissions (600)

### Package not found after upload
- Wait a few minutes for PyPI to index the package
- Clear pip cache: `pip cache purge`

## Additional Resources

- [PyPI Publishing Guide](https://packaging.python.org/tutorials/packaging-projects/)
- [Twine Documentation](https://twine.readthedocs.io/)
- [Python Packaging User Guide](https://packaging.python.org/)
