# Contributing Guidelines

Thank you for your interest in contributing to Group 25's DSCI 522 project, Predicting Top 10 Billboard Hits Using Audio Features and Metadata.

We welcome suggestions, bug reports, and contributions that improve the project's code, analysis, documentation, and reproducibility.

Before contributing, please review our Code of Conduct.

## Reporting Bugs

If you encounter a bug or an issue with the project:

1.  Check the repository's existing GitHub issues to avoid duplicates.

2.  Open a new issue in the GitHub Issues page.

3.  Provide a clear description of the problem, steps to reproduce it, the expected and actual behavior, and any relevant error messages.

## Suggesting Changes

We welcome suggestions for improvements to the analysis, code, visualizations, and documentation.

To suggest a change:

1.  Check existing issues for similar suggestions.

2.  Open a new GitHub issue describing your proposed improvement.

3.  Explain why the change would benefit the project.

## Submitting Contributions

To contribute code, analysis, or documentation:

1.  Fork the repository and clone your fork to your computer.

2.  Create a new branch for your changes.

3.  Make your changes, following the project's existing structure and conventions.

4.  Check that the report still renders without errors and that the analysis remains reproducible.

5.  Commit your changes with a clear, descriptive message.

6.  Push your branch to your fork and open a pull request against the repository's main branch.

7.  Describe your changes in the pull request and reference any related GitHub issues.

All pull requests must be reviewed by a Group 25 member before merging. Contributors may be asked to revise their work before it is accepted.

## Adding a Package and Updating the Docker Image

The Docker image is built from `Dockerfile`, `pyproject.toml`, and `uv.lock`, and is published automatically to [Docker Hub](https://hub.docker.com/r/<your-dockerhub-username>/dsci-522-predicting-top-10-billboard-hits-using-audio-features-and-metadata) by GitHub Actions. You never need to build or push it yourself.

To add or update a package:

1.  Create a branch from `main`:

    ``` bash
    git switch main && git pull
    git switch -c add-<package>
    ```

2.  Add the package with an exact version:

    ``` bash
    uv add "<package>==<version>"
    ```

3.  Commit and push:

    ``` bash
    git add pyproject.toml uv.lock
    git commit -m "Add <package>"
    git push -u origin add-<package>
    ```

4.  Wait for the **Publish image to Docker Hub** workflow in the Actions tab to finish. It publishes a new image and updates the tag in `docker-compose.yml` on your branch.

5.  Pull the update and check the report still renders:

    ``` bash
    git pull
    docker compose pull
    docker compose run --rm analysis-env uv run quarto render analysis/billboard-top-10-prediction.qmd
    ```

6.  Open a pull request. After it is merged, teammates run `git pull` and `docker compose pull` to get the new environment.

Do not edit the `image:` line in `docker-compose.yml` by hand. The workflow updates it.

## Republishing the Report

The rendered report is published on GitHub Pages from `docs/index.html`. Whenever you change the analysis or the report, you also need to republish it so the website matches the code in the repository.

First, make sure you are on your feature branch and have the latest Docker image:

```bash
docker compose pull
```

Next, render the report inside the container. This ensures it is built with the same packages as everyone else's:

```bash
docker compose run --rm analysis-env uv run quarto render analysis/billboard-top-10-prediction.qmd
```

Then copy the rendered report into the `docs/` folder, which is the folder GitHub Pages serves:

```bash
cp analysis/billboard-top-10-prediction.html docs/index.html
```

Finally, commit both files, push your branch, and open a pull request as usual:

```bash
git add analysis/billboard-top-10-prediction.html docs/index.html
git commit -m "Republish report"
git push
```

Once the pull request is merged into `main`, GitHub Pages updates the website automatically within a few minutes. Please do not edit `docs/index.html` by hand, and keep `embed-resources: true` in the report's YAML header so that all figures are included in the single HTML file.

## Generative AI Policy

Contributors may use generative AI tools, such as ChatGPT, GitHub Copilot, or Claude, to assist with coding, debugging, documentation, and brainstorming.

However, contributors must follow these guidelines:

**Permitted use**: AI tools may assist with generating ideas, explaining concepts, generating code, or improving documentation.

**Disclosure**: Contributors must disclose meaningful AI assistance in their pull requests, including the tool used and how it contributed to the work.

**Verification**: All AI-assisted content must be reviewed, tested where applicable, and checked for accuracy, originality, and compliance with project requirements.

**Responsibility**: Human contributors remain fully responsible for all submitted work, including any errors, inappropriate content, or licensing concerns.

**Human oversight**: GitHub issues and pull requests must be opened, reviewed, and merged by human contributors, not autonomous AI agents.

Contributions that do not follow these guidelines may be requested for revision or rejected.

## Questions

For questions about contributing, please open an issue in the project repository.
