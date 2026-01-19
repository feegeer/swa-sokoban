# swa-sokoban

The architecture documentation of group Sokoban. It roughly follows the [arc42](https://arc42.org/overview) template. Furthermore, all content of the documentation is written in [reStructuredText](https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html) (more powerful than Markdown) and rendered to HTML pages with [Sphinx](https://www.sphinx-doc.org/en/master/)

## Setup & Installation

You need to have Python >= 3.13 and pip/[uv](https://docs.astral.sh/uv/) installed on your system (if you use uv, you do not necessarily need Python >= 3.13). Run

```bash
git clone git@github.com:feegeer/swa-sokoban.git # assuming you have set up an SSH key
cd swa-sokoban.git
uv sync # if you still use pip: python -m venv .venv && .venv/bin/activate  &&  pip install -e .
```

## Build the documentation locally

To build the documentation locally, it is sufficient to run from the repository root

```bash
uv run make html
```

The HTML `index.html` of the built documentation can be found under the directory `build/html`. Just open it in some browser to view it.

## Online version of the architecture documentation

The online version of our architecture documentation is hosted on GitHub Pages and can be accessed [by clicking here](https://feegeer.github.io/swa-sokoban/) It is built and deployed from the `main` branch via GitHub Actions. This is the [corresponding workflow file](https://github.com/feegeer/swa-sokoban/blob/main/.github/workflows/doc.yml). The workflow is triggered whenever commits are pushed to the `main` branch and in as a check in pull requests. [Manual triggering](https://github.com/feegeer/swa-sokoban/actions/workflows/doc.yml) is also possible.

## Contributing

To prevent frequent merge conflicts and ensure that the latest documentation always builds, **the main branch is protected**, which means that you have to:

- create a branch for your stuff, ideally named like the task you are currently working on, e.g. `add-domain-model`
- add your contribution as [reStructuredText](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html) in the according `.rst` file under the `doc` directory
- open a Pull Request, that you can then approve yourself. Note that you can only rebase your commits to the `main` branch if the **documentation build workflow passes** and if there are **no merge conflicts with the main branch**

## Help

If you have any suggestions, comments, questions or feedback regarding the repository and documentation structure, the deployment workflow, reStructuredText and Sphinx (like "how can I add an SVG image to my page?"), please do not hesitate and send me a message on WhatsApp.
