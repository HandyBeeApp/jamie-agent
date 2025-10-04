# Make sure the libraries are there and updated

```
poetry install
```

## Set up Poetry in IDE

Run this to be able to see poetry virtual environment in IDE

```
poetry config virtualenvs.in-project true
```

### (Optional)

To see your Poetry virtual environment path, run:

```
poetry env info --path
```

This will output the full path to your Poetry virtual environment.

Then add the virtual environment to the IDE using Control+Shift+P and select "Python: Select Interpreter" and copy the contents

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

## Run agent on the web

```
adk web
```
