# Initialize the repository

git init

# Create the repository on GitHub

gh repo create HandyBeeApp/jamie-agent --public --source=. --remote=origin --push

# First, add all files and create the initial commit

git add .
git commit -m "Initial commit: Generic multi-agent system"

# Set the main branch to main

git branch -M main

# Push to remote

git push -u origin main
