DEVOPS CI/CD PIPELINE PROJECT
Your task is to build a complete automation pipeline for a simple web application.
Difficulty: Beginner-friendly
what you'd learn: Docker, Jenkins, Git, Automation.


Goal: 
- a working web application containerized with docker
- A jenkins server running in a container
- an automated CI/CD pipeline that deploys code changes


prerequisites:
Docker, Git, code editor, Github Account


STEP 1. Fork the repository

STEP 2. Understand the Application by checking it out

STEP 3. Build and test the docker image

STEP 4. Run Jenkins in Docker
# Create a network for Jenkins and your app
docker network create devops-net

# Run Jenkins container
docker run -d \
  --name jenkins \
  --network devops-net \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $(which docker):/usr/bin/docker \
  --group-add $(stat -c '%g' /var/run/docker.sock) \
  jenkins/jenkins:lts-jdk17

# Wait 30 seconds for Jenkins to start
sleep 30

# Get the initial admin password
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword


STEP 5. Configure Jenkins
- Open your browser and go to: http://localhost:8080
- Enter the initial admin password from above
- Click "Install suggested plugins" (wait for installation)
- Create your admin user:

Username: admin (or your choice)
Password: [create a password]
Full name: Your Name
Email: your@email.com
Save and finish

STEP 6. Add Github credentials to jenkins
- Go to: https://github.com/settings/tokens
- Click "Generate new token (classic)"
- Give it a name: jenkins-token
- Select scope: repo (full control of private repos)
- Click Generate token
- COPY THE TOKEN NOW (you won't see it again!)
- Add Token to Jenkins

In Jenkins, go to Dashboard → Manage Jenkins → Credentials
Click System → Global credentials (unrestricted) → Add Credentials
Fill in:

Kind: Username with password
Username: Your GitHub username
Password: Paste the GitHub token
ID: github-credentials
Description: GitHub access token
Click Create


STEP 7: Update the Jenkinsfile
- on your code editor, find the jenkinsfile for this project and edit the url to match your forked repo ( change this :url: 'https://github.com/YOUR_USERNAME/simple-devops-project.git')
- save and push to Github


STEP 8: Create the pipeline in jenkins
- In Jenkins, click "New Item"
- Enter name: simple-devops-pipeline
- Select Pipeline and click OK
- Scroll to Pipeline section
Configure:
Definition: Pipeline script from SCM
SCM: Git
Repository URL: https://github.com/YOUR_GITHUB_USERNAME/simple-devops-project.git
Credentials: Select github-credentials
Branch Specifier: */main (please change this on jenkins from master to main)
Script Path: Jenkinsfile
Click Save


STEP 9: Run your pipeline
- click on "Build Now" and wait for it to run
- click on the build number and check console output
(if your pipeline fails feel free to use an AI tool to correct)


STEP 10: Verify your deployed App
# Check if the app is running
curl http://localhost:5005

# Check health endpoint
curl http://localhost:5005/health

# Check version
curl http://localhost:5005/version


STEP 11: Test Auto-deployment
# Update the version in app.py
sed -i '' 's/1.0.0/1.0.1/g' app/app.py

# Update Dockerfile version
sed -i '' 's/APP_VERSION=1.0.0/APP_VERSION=1.0.1/g' Dockerfile

# Commit and push
git add app/app.py Dockerfile
git commit -m "Bump version to 1.0.1"
git push origin main

# watch jenkins: The pipeline should trigger automatically within 30 seconds and after it completes: # Check the new version 
curl http://localhost:5005/version


STEP 12: Document Your Work
Take screenshoots of
- jenkins pipeline success page
- console output showing all stages passed
- browser showing your app at localhost:5005
