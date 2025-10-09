# MONICA Documentation

This repository hosts the documentation site for MONICA.

You can view the live site here: https://zalf-rpm.github.io/monica-documentation/

MONICA (Model for Nitrogen and Carbon Dynamics in Agro-Ecosystems) is a dynamic, process-based simulation model which describes transport and biochemical turnover of carbon, nitrogen, and water in agro-ecosystems on a daily time step.
The documentation is built using [MkDocs](https://www.mkdocs.org/) with the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme.

## Getting started (local development)

### Prerequisites
- Python 3.8+
- pip

### Clone the repository
```
git clone https://github.com/zalf-rpm/monica-documentation.git
```

### Install dependencies
```
pip install mkdocs-material
```

### Serve locally
Start a local development server with live reload:
```
mkdocs serve
```
Then open the URL printed in the terminal (typically `http://127.0.0.1:8000`).

### Build static site
Generate the static documentation:
```
mkdocs build
```
The static site will be created in the `site/` directory.