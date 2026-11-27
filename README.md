------------------------------------------------------------------------

# DSCI-522-Group-25

## Predicting Top 10 Billboard Hits Using Audio Features and Metadata

Authors: Nitpreet Bamra, Eshadhi Bogoda, Jack Liu, James Wang

This project was developed by Team 25 as part of DSCI 522: Data Science Workflows in the University of British Columbia's MDS program (2026–2027).

## About

What makes a song reach the top of the Billboard charts? While music success is influenced by various factors, characteristics such as a song's audio features, genre, and release era may help explain differences in chart performance.

Our project investigates whether these characteristics can predict if a song appearing on the Billboard Hot 100 will reach the Top 10. Using historical Billboard chart data and song-level audio information from public datasets on Kaggle, we explore relationships between musical characteristics and chart success and develop a machine learning classification model to identify patterns associated with Top 10 songs.

Our primary research question is: **Can a song's audio features, genre, and release era predict whether a Billboard Hot 100 song reaches the Top 10?**

Our analysis used 24074 songs, with only 2,081 out of 16,851 songs in the training set reaching the Top 10. Our tuned logistic regression model achieved an accuracy of 0.586, recall of 0.650, precision of 0.169, and F1-score of 0.268 on the held-out test set. Although the model correctly identified approximately 65% of songs that reached the Billboard Hot 100 Top 10, its low precision indicates a high number of false positives. These findings suggest that audio features, genre, and release era have limited predictive power for identifying Top 10 hits, and further improvements are needed before the model can reliably predict chart success.

### Dataset

Our project uses the Billboard Hot 100 Audio Features public dataset, published on Kaggle, which can be found at <https://www.kaggle.com/datasets/thedevastator/billboard-hot-100-audio-features>

Our analysis combines two files from this dataset:

- `Hot Stuff.csv`: Contains historical Billboard Hot 100 chart information, including song titles, performers, and chart performance.
- `Hot 100 Audio Features.csv`: Contains audio characteristics and metadata associated with songs appearing on the Billboard Hot 100.

The two files are merged using song titles and performers as matching identifiers. The combined dataset is then processed to investigate whether audio features, genre, and release era can predict Top 10 chart performance. The response variable is a binary indicator representing whether a song reached the Billboard Hot 100 Top 10.

The data is downloaded programmatically using the Python kagglehub package. Original data files are stored in data/raw/, while cleaned and processed data is stored in data/processed/.

Dataset Source: [Billboard Hot 100 Audio Features](https://www.kaggle.com/datasets/thedevastator/billboard-hot-100-audio-features), published by `thedevastator` on Kaggle.

Dataset license: Dataset copyright by authors, as listed on the [Kaggle dataset page](https://www.kaggle.com/datasets/thedevastator/billboard-hot-100-audio-features). The data was originally compiled by Sean Miller; please credit the authors when reusing it.

### Report

The final report can be found [here](https://ubc-mds-2026-27.github.io/DSCI-522-predicting-top-10-billboard-hits-using-audio-features-and-metadata/analysis/billboard-top-10-prediction.html) (HTML).

## Usage

We run the analysis in a Docker container, so everyone uses the same software and package versions. The image is published on Docker Hub at [nitpreetb/dsci-522-predicting-top-10-billboard-hits-using-audio-features-and-metadata](https://hub.docker.com/r/nitpreetb/dsci-522-predicting-top-10-billboard-hits-using-audio-features-and-metadata).

#### 1: Prerequisites

Ensure that [Git](https://git-scm.com/downloads) and [Docker Desktop](https://www.docker.com/products/docker-desktop/) are installed on your computer.

Open Docker Desktop and ensure that it is running before proceeding with the following steps.

#### 2: Clone the Repository

Open a Git Bash terminal and run the following commands to clone the repository and navigate to the project directory:

``` bash
git clone https://github.com/UBC-MDS-2026-27/DSCI-522-predicting-top-10-billboard-hits-using-audio-features-and-metadata.git
cd DSCI-522-predicting-top-10-billboard-hits-using-audio-features-and-metadata
```

#### 3: Generate an SSH Key

If you do not already have an SSH key named `id_ed25519`, generate one using the following command:

``` bash
ssh-keygen -t ed25519
```

Press Enter at each prompt to accept the default settings. Ensure that the public key exists at `~/.ssh/id_ed25519.pub` before proceeding.

#### 4: Download the Docker Image

From the root directory of the repository, download the pre-built Docker image using the following command:

``` bash
docker compose pull
```

This command downloads the version of the Docker image specified in `docker-compose.yml`, ensuring that users run the analysis in the same computational environment.

#### 5: Prepare the Data Directories

The analysis uses the Python `kagglehub` package to download the dataset directly from Kaggle. The download process is included in the analysis code, so users do not need to download the dataset manually.

Before rendering the report, create the required data directories if they do not already exist:

``` bash
mkdir -p data/raw data/processed
```

#### 6: Run the Analysis

From the root directory of the repository, run the following command:

``` bash
docker compose run --rm analysis-env uv run quarto render analysis/billboard-top-10-prediction.qmd
```

This command runs the analysis inside a temporary Docker container. The analysis performs data loading, cleaning, exploratory data analysis, model training, and evaluation.

The `--rm` flag automatically removes the temporary container once the rendering process finishes. No additional commands are required to stop or remove the container.

#### 7: View the Report

After rendering is complete, open the generated HTML report located at `analysis/billboard-top-10-prediction.html` in a web browser.

## Dependencies

Git and Docker Desktop are required to reproduce this analysis.

All other computational dependencies, including Python, uv, Quarto, and the required Python packages, are provided within the Docker environment.

The main Python libraries used in this project include pandas and numpy for data manipulation and numerical operations, kagglehub for downloading the dataset, and scikit-learn for preprocessing, classification, and model evaluation.

The complete list of Python package dependencies and their versions can be found in [pyproject.toml](pyproject.toml) and [uv.lock](uv.lock). The computational environment is defined in the [Dockerfile](Dockerfile) and [docker-compose.yml](docker-compose.yml).

## Contributing

If you would like to contribute to this project or make changes to the analysis, please refer to our [Contributing Guidelines](CONTRIBUTING.md) for instructions on setting up and working within the development environment.

## License

The following licenses are proposed for this project:

Code: MIT License

Report: Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)

The original Kaggle dataset: Dataset copyright by authors (see Dataset license above)

For the complete license terms, see LICENSE.md.

## Generative AI Usage

Generative AI tools were used to assist with various aspects of the project's documentation and code development.

- ChatGPT (OpenAI) was used to help draft and organize project documentation, including the README.

- Claude (Anthropic) was used to assist with reviewing and designing portions of the data download code.
