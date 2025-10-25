# django-cron

django-cron fork of Tivix/django-cron compatible with Django 5 - built for Ticketping

## Local development

### Pre-requisites

- Python >= 3.11.13

We suggest using [`pyenv`](https://github.com/pyenv/pyenv) to easily manage python versions. Some of the following commands use `pyenv`.
Use [pyenv-installer](https://github.com/pyenv/pyenv-installer) for easy installation.

### Configure local development setup

- Install and activate python 3.9.10 in the root directory

  - `pyenv install 3.11.13`
  - `pyenv virtualenv 3.11.13 djcron`
  - `pyenv local djcron`

- Install project requirements

  - `curl -sSL https://install.python-poetry.org | python -`
  - `poetry install`

- Install precommit hook (`pre-commit` should be installed globally on system first)

  - `pre-commit install`
