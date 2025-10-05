# REQUIREMENTS Setup UVX
```
pipx install uv
```
https://google.github.io/adk-docs/deploy/agent-engine/#prerequisites-ad



# Run the ASP enhance command to add the needed files required for deployment into your project.

``` 
uvx agent-starter-pack enhance --adk -d agent_engine
```

Follow the instructions from the ASP tool. In general, you can accept the default answers to all questions. However for the GCP region, option, make sure you select one of the supported regions for Agent Engine.

When you successfully complete this process, the tool shows the following message:

> Success! Your agent project is ready.