# CICDPIPE-HTML

This project demonstrates a CI-CD pipeline using Bash, Python, and cron jobs. It automatically deploys code updates from a GitHub repository to an Nginx server.

Project Overview
This CI-CD pipeline pulls the latest code from a GitHub repository and deploys it to an Nginx server automatically. The pipeline is powered by:

Python script to check for new commits using GitHub API. Bash script to pull the latest code and restart Nginx. Cron job to automate the check for new commits.

Prerequisites
Ensure you have the following installed on the server:

Nginx for serving the HTML project. Python 3 with requests library. Git for version control. A GitHub account and a repository for your project.

Setup Instructions
1. HTML Project Setup
Clone or create a my_html_project:
 mkdir my-html-project
   cd my-html-project
Add an index.html file
<!DOCTYPE html>
<html>
<head>
 <title>CI-CD Test</title>
</head>
<body>
 <h1>Welcome to the CI-CD Test Page</h1>
</body>
</html>```

Initialize Git and push the project to GitHub
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/AbhayShukla1907/my-html-project.git
git push -u origin master

2. Nginx and Server Setup
set up a local Linux machine
Install Nginx
sudo apt update
sudo apt install nginx
sudo systemctl enable nginx
sudo systemctl start nginx

3. Write a Python Script to Check for New Commits
Install Required Python Packages: Install Python if not already installed
sudo apt install python3 python3-pip -y
Install the requests library for making HTTP requests "pip3 install requests" 3. Create the Python Script: Create a file named check_commits.py "nano check_commits.py" Add the following code to the file

 import requests

"REPO_OWNER = 'AbhayShukla1907'
REPO_NAME = 'my_html_project'
GITHUB_API_URL = f'https://api.github.com/repos/AbhayShukla/my_html_project/commits'

# Function to check for new commits
def check_for_new_commits():
 response = requests.get(GITHUB_API_URL)
 if response.status_code == 200:
     latest_commit = response.json()[0]['sha']
     with open('latest_commit.txt', 'r+') as f:
         last_commit = f.read().strip()
         if latest_commit != last_commit:
             f.seek(0)
             f.write(latest_commit)
             f.truncate()
             print("New commit found!")
             return True
         else:
             print("No new commits.")
             return False
 else:
     print("Failed to fetch commits.")
     return False

if __name__ == '__main__':
 check_for_new_commits()"

3. Create a File to Store the Latest Commit:
     echo "" > latest_commit.txt
4. Write a Bash Script to Deploy the Code
Create the Bash Script: Create a file named deploy.sh "nano deploy.sh" Add the following code to the file
   #!/bin/bash
# Navigate to the web root directory (adjust this path as needed)
cd /var/www/html

# Pull the latest changes from GitHub
git pull origin main

# Restart Nginx to apply the changes
sudo systemctl restart nginx

echo "Deployment completed!"
Make the Script Executable: "chmod +x deploy.sh"
5. Set Up a Cron Job to Run the Python Script
Edit the Crontab Open the crontab edito "crontab -e" Add the following line to run the Python script every minute "* * * * * /usr/bin/python3 /path/to/check_commits.py && /path/to/deploy.sh"
6. Test the Setup
Make a New Commit: Make a change to your index.html file, for example: "echo "
Updated content/!

" >> index.html" Add and commit the change
git add index.html
git commit -m "Updated content"
git push origin main

Verify Deployment Wait for a minute, then visit your server's public IP in a browser. You should see the updated content on your webpage.
Conclusion
This CI-CD pipeline continuously checks for updates in a GitHub repository and automatically deploys them to an Nginx server, ensuring efficient delivery of updates.
