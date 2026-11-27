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

## Working in the container

To run and edit the project, we work inside the Docker container, using Positron over ssh.

**Before your first connection:**

1.  Download [Positron](https://positron.posit.co/download.html), if you do not already have it.

2.  Set up an ssh key, if you do not already have one. In a terminal on your computer, run the command below and press Enter at each question:

    ``` bash
    ssh-keygen -t ed25519
    ```

    Then check that `~/.ssh/id_ed25519.pub` exists.

3.  Check that Git has your correct email on your computer:

    ``` bash
    git config --global user.email
    ```

    It should print the email of your GitHub account. If it prints nothing, set your name and email. `docker-compose.yml` shares these with the container, so your commits there count as yours:

    ``` bash
    git config --global user.name "Your Name"
    git config --global user.email "you@example.com"
    ```
4. Open Docker Desktop on your computer and ensure it is running. 

5. Download the image, from the root of the repository:

    ``` bash
    docker compose pull
    ```

**Connecting with Positron:**

1.  Start the container from the root of the repository:

    ``` bash
    docker compose up -d
    ```

2.  In Positron, open the Command Palette (Cmd+Shift+P on Mac, Ctrl+Shift+P on windows)

3. Type and run **Remote SSH: Show Remote Menu** → select **Connect to Host** and  type `root@localhost:2222`. 
    
    **If the connection fails** (e.g. `Could not establish connection to "localhost"` or `ECONNREFUSED`), click **Close Remote**, then try again using the IP address instead of `localhost`: and type `root@127.0.0.1:2222`. 

4.  Open Folder in the Explorer. Type in `/workspace` at the path box that appears.

5.  If Positron shows **Restricted Mode**, click **Trust this folder**.

6.  Pick the console with the interpreter at `/opt/venv/bin/python`. If it isn't listed, run **Python: Select Interpreter** → **Enter interpreter path...** and type it in. Check it with:

    ``` python
    import sys; print(sys.executable)
    ```

7.  You should now be abe to use the terminal, console and render in positron. **If you are having issues**, check the section **Troubleshooting Help** found below. 

8.  When you're finished, close the Positron window and run:

    ``` bash
    docker compose down
    ```

**Troubleshooting Help**

-   **Restricted Mode / Python unavailable:** click **Trust this folder**.
-   **`/opt/venv/bin/python` not in the list:** enter the path by hand (Connecting with Positron: step 6).
-   **"Remote host identification has changed":** run `ssh-keygen -R "[localhost]:2222"` and connect again **or**
     `ssh-keygen -R "[127.0.0.1]:2222` if you used the IP address.
-   **GitHub asks you to sign in when you push:** sign in once in the browser window that opens. Positron will remember it.
-   **Docker is open**: Ensure you are signed in and Docker is already turned on.
-   **If the connection fails (for Windows)** instead of `localhost`: and type `root@127.0.0.1:2222` (Connecting with Positron: Step 3). 
-   **The ssh key shows up as a folder instead of a file:** this means Docker started before your key existed. 
    Stop the container with `docker compose down`, delete the `id_ed25519.pub` folder inside `~/.ssh`, 
    create your key again (Before your first connection: Step 2) and reconnect.
-   **Error saying port 2222 is taken:** another program on your computer is already using it.
     Open `docker-compose.yml`, change the number on the left of `"2222:22"` (for example to `"2223:22"`) and use that new number when you connect (e.g. `root@localhost:2223`).

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

