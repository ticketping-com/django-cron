# django-cron

[![PyPI version](https://badge.fury.io/py/django-cron-django5.svg)](https://badge.fury.io/py/django-cron-django5)
[![Python Versions](https://img.shields.io/pypi/pyversions/django-cron-django5.svg)](https://pypi.org/project/django-cron-django5/)
[![Django Versions](https://img.shields.io/badge/django-4.2%20%7C%205.0%20%7C%205.1-blue.svg)](https://www.djangoproject.com/)

A Django app for running scheduled tasks (cron jobs) within your Django project. This is a fork of Tivix/django-cron with full Django 5 compatibility. Used by [Ticketping](https://ticketping.com/) in production.

## Features

- **Simple API**: Define cron jobs with a clean, Pythonic interface
- **Django 5 Compatible**: Fully tested and working with Django 5.x
- **Flexible Scheduling**: Support for various schedule types (daily, hourly, custom intervals)
- **Multiple Locking Backends**: File, cache, and database-based locking to prevent concurrent runs
- **Job Logging**: Track job execution history and debug issues easily
- **No System Cron Required**: Jobs run within your Django process

## Installation

Install from PyPI:

```bash
pip install django-cron-django5
```

Add `django_cron` to your `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    # ...
    'django_cron',
]
```

Run migrations:

```bash
python manage.py migrate django_cron
```

## Quick Start

Create a cron job by subclassing `CronJobBase`:

```python
from django_cron import CronJobBase, Schedule

class MyCronJob(CronJobBase):
    RUN_EVERY_MINS = 60  # Run every hour

    schedule = Schedule(run_every_mins=RUN_EVERY_MINS)
    code = 'myapp.my_cron_job'  # Unique code

    def do(self):
        # Your code here
        pass
```

Register your cron job in settings:

```python
CRON_CLASSES = [
    'myapp.cron.MyCronJob',
]
```

Run cron jobs:

```bash
python manage.py runcrons
```

For continuous execution, use:

```bash
python manage.py cronloop
```

## Requirements

- Python >= 3.9
- Django >= 4.2, < 6.0

## Documentation

For detailed documentation, please visit the [GitHub repository](https://github.com/ticketping-com/django-cron).

## Contributing

We welcome contributions! Please see our [GitHub repository](https://github.com/ticketping-com/django-cron) for more information.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Credits

- Original project: [Tivix/django-cron](https://github.com/Tivix/django-cron)
- Maintained for Django 5 by Ticketping

---

## Development Setup

### Pre-requisites

- Python >= 3.9

We suggest using [`pyenv`](https://github.com/pyenv/pyenv) to easily manage python versions.

### Configure local development setup

1. Install and activate python:

```bash
pyenv install 3.11.13
pyenv virtualenv 3.11.13 djcron
pyenv local djcron
```

2. Install project requirements:

```bash
pip install -r requirements-dev.txt
```

3. Install pre-commit hooks (if `pre-commit` is installed globally):

```bash
pre-commit install
```

## Running Tests

```bash
python testmanage.py test django_cron
```
