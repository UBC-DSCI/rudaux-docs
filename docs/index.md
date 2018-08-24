# Rudaux

Welcome to the documentation for rudaux!

Rudaux helps you programmatically administer a course by integrating:

- [Canvas](https://www.canvaslms.com/) - a learning management system
- [JupyterHub](https://github.com/jupyterhub/jupyterhub) - a multi-user Jupyter notebook Server
- [nbgrader](https://github.com/jupyter/nbgrader) - a Jupyter notebook auto-grader
- [nbgitpuller](https://github.com/data-8/nbgitpuller) - a JupyterHub extension to pull Jupyter notebooks from git repositories

Rudaux was designed to simplify course management generally, but there are a few operations in particular that would be nearly impossible without rudaux.

- Syncing students and assignments between Canvas and nbgrader.
- Creating assignments in Canvas with JupyterHub/nbgitpuller links.
- Scheduled automated grading of Jupyter notebooks with nbgrader.

Rudaux is named after the French artist and astronomer Lucien Rudaux, a pioneer in space artistry and one of the first artists to paint Jupiter.

<div class="showcase">
  <p>
    For information on the motivation behind and development of rudaux, please read 
    <a href="https://samhinshaw.com/blog/designing-rudaux"><em>Designing Rudaux</em></a>.
    <br>
    For information on how to use Rudaux to integrate Canvas and JupyterHub, please read 
    <a href="https://samhinshaw.com/blog/using-rudaux"><em>Using Rudaux</em></a>.
  </p>
</div>

## Installation

```
pip install rudaux
```

## Setup

Before setting up rudaux, it is important to have the proper infrastructure in place. Please see the [DSCI 100 infrastructure repository](https://github.ubc.ca/UBC-DSCI/dsc100-infra) for our reproducible infrastructure provisioning workflow.

_Note_: rudaux currently requires a fork of nbgrader to work properly ([more information](https://github.com/samhinshaw/rudaux/issues/7)):

```sh
pip install git+git://github.com/samhinshaw/nbgrader.git
```

Once your servers are set up and your dependencies installed, rudaux needs a configuration file to operate. Please read the [configuration](config) documentation for more information and a sample config file.

1. Log in to the server you will be executing rudaux commands on.
2. Clone your instructors repository containing your config file and master (source) assignments.
3. Initialize rudaux.

## Usage

### Command-Line Interface

See [command-line interface](cli).

```sh
rudaux {init, grade, submit}
```

### Python API

See [API](api).

```py
from rudaux import Course, Assignment
```

## Contributing

1. Clone this repository.
2. Install [MkDocs](https://www.mkdocs.org/#installation).
3. In this repo's directory, start the MkDocs development server: `mkdocs serve`.
4. Edit source docs in [docs/](docs), and see the dev server hot reload your changes!
5. Build and deploy to GitHub Pages with `mkdocs gh-deploy`.
