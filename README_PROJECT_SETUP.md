# Setup this ADK Example project from

https://github.com/google/adk-samples/tree/main/python/agents/jamie-agent

## Getting Started

Python Agent Development Kit. See the ADK Quickstart Guide.
https://google.github.io/adk-docs/get-started/quickstart/

## Python 3.9+ and Poetry Requirements

### install pipx with brew

https://pipx.pypa.io/stable/installation/

```
brew install pipx
pipx ensurepath
sudo pipx ensurepath --global
```

### install poetry

https://python-poetry.org/docs/#installing-with-pipx

```
pipx install poetry
```

## Install dependencies using Poetry

```
poetry config keyring.enabled false
```

```
poetry install
```

## Set up Poetry in IDE

Run this to be able to see poetry virtual environment in IDE

```
poetry config virtualenvs.in-project true
```
To see your Poetry virtual environment path, run:
```
poetry env info --path
```
This will output the full path to your Poetry virtual environment, something like:


Then add the virtual environment to the IDE using Control+Shift+P and select "Python: Select Interpreter"

## Setup .env file

```
cp .env.example .env
```

Then fill in the values for the environment variables.

## Authenticate your GCloud account.

```
gcloud auth application-default login
gcloud auth application-default set-quota-project handybee-ai
```
Result: PROJECT was added to ADC which can be used by Google client libraries for billing and quota. Note that some services may still bill the project owning the resource.

## Activate the virtual environment set up by Poetry, run:

```
eval $(poetry env activate)
```

(jamie-agent-py3.12) $ # Virtualenv entered

## Install Google ADK

```
pip install google-adk
```

## Run agent on the web

```
adk web
```
