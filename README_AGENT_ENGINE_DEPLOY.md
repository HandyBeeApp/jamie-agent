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

### Answers

Continue with enhancement? [Y/n]: : y
--
Choose where your agent code is located:
  1. deployment
  2. eval
  3. rag (has agent.py)
  4. Enter custom directory name

Select agent directory (1): 3
--
> Please select a CI/CD runner:
1. Google Cloud Build - Fully managed CI/CD, deeply integrated with GCP for fast, 
consistent builds and deployments.
2. GitHub Actions - GitHub Actions: CI/CD with secure workload identity federation 
directly in GitHub.

Enter the number of your CI/CD runner choice (1): 2
--
Enter desired GCP region (Gemini uses global endpoint by default) (europe-west4): europe-west4
--
> You are logged in with account: 'team@handybee.au'
> You are using project: 'handybee-ai'
> Do you want to continue? (The CLI will check if Vertex AI is enabled in this project) 
[Y/skip/edit] (Y): Y

# Quick Start (Local Testing)

Install required packages and launch the local development environment:

```bash
make install && make playground
```

# Deploy agent to Agent Engine
```bash 
make backend
``` 
