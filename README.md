# Deploy Pelican to Github Pages

Automated deployment of Pelican SSG generated static websites to GitHub Pages.

GitHub Pages can serve webpages from three predefined places

1. **main/root**: The root folder of the main branch
2. **main/docs**: Doc folder in the main branch
2. **gh-pages/root**: The root folder of any branch named "gh-pages"

## Prerequisites

1. Your working directory should be in the `main` branch of your repository.
2. Ensure you have captured your dependencies in `requirements.txt`. If not you can run the below command

```bash
pip freeze > requirements.txt
```

## Steps to use

First create a file named at the path `.github/.workflows/pelican.yml`
The conents of the file should be

```yaml
name: Deploy

on:
  # Trigger the workflow on push on main branch,
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
      with: 
        submodules: 'true'
    - uses: diegoquintanav/pelican-to-github-pages@v1.0.3
      env:
        GITHUB_TOKEN: ${{secrets.GITHUB_TOKEN}}
        GH_PAGES_CNAME: ${{secrets.DOMAIN_CNAME}}
```

Then, setup a secrets in your repository named `DOMAIN_CNAME` that should contain the URL of your custom domain without the protocol, e.g. `example.com`. This is only required if you have a custom domain, if you want to use the `*.github.io` subdomain, then you don't need this.

## Default configuration and overrides

This GitHub action will generate the static website using the following defaults

1. Configuration: Using `publishconf.py` as the default configuration file
2. Content: Uses `content` as the default content directory

You can override them by adding the following in the pelican.yml file under env variables


- `PELICAN_CONFIG_FILE`: path to the `publishconf.py` configuration file
- `PELICAN_CONTENT_FOLDER`: path to the `content` folder
- `PYTHON_VERSION`: docker tag of python image used to run pelican. Check available tags at <https://hub.docker.com/_/python/>.

