# CI/CD Setup Guide - Manual Steps

This guide provides step-by-step instructions to connect your Express.js app with GitHub and Jenkins.

---

## Part 1: Create GitHub Repository

### Step 1: Create Repository on GitHub.com
1. Go to [https://github.com](https://github.com) and log in
2. Click the **+** icon in the top-right corner → **New repository**
3. Fill in:
   - Repository name: `jenkins-cicd-learn`
   - Description: "A small Express.js app to learn CI/CD with Jenkins and GitHub"
   - Select **Public** or **Private**
   - Check **Add a README file** (optional)
4. Click **Create repository**

### Step 2: Push Your Local Code to GitHub

Run these commands in your project directory:

```bash
# Initialize git (if not already initialized)
git init

# Add all files
git add .

# Create initial commit
git commit -m "Initial commit - Express.js app with Jenkins CI/CD"

# Add remote repository (replace YOUR_USERNAME with your GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/jenkins-cicd-learn.git

# Push to GitHub
git push -u origin main
```

---

## Part 2: Install Jenkins

### Option A: Install Jenkins on macOS

```bash
# Using Homebrew
brew install jenkins-lts

# Start Jenkins
brew services start jenkins-lts
```

Jkins will be available at: `http://localhost:8080`

### Option B: Install Jenkins using Docker

```bash
# Pull and run Jenkins
docker run -d -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  --name jenkins jenkins/jenkins:lts
```

### Option C: Install Jenkins on Ubuntu/Linux

```bash
# Add Jenkins repository
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

# Install Jenkins
sudo apt-get update
sudo apt-get install jenkins

# Start Jenkins
sudo systemctl start jenkins
```

---

## Part 3: Initial Jenkins Setup

1. Open Jenkins in browser: `http://localhost:8080`
2. Get the initial admin password:
   ```bash
   # For macOS/Linux
   cat /Users/navneet/.jenkins/secrets/initialAdminPassword
   # Or on Linux: sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
3. Enter the password and click **Continue**
4. Click **Install suggested plugins**
5. Create your admin user account
6. Click **Save and Finish**
7. Click **Start using Jenkins**

---

## Part 4: Install Node.js Plugin in Jenkins

1. Go to **Manage Jenkins** → **Manage Plugins**
2. Click on **Available** tab
3. Search for **NodeJS**
4. Check the **NodeJS** plugin
5. Click **Install without restart**
6. After installation, go to **Manage Jenkins** → **Global Tool Configuration**
7. Click **Add NodeJS**
8. Configure:
   - Name: `NodeJS-18` (or your preferred version)
   - Check **Install automatically**
   - Select version from dropdown
9. Click **Save**

---

## Part 5: Create Jenkins Pipeline Job

### Method 1: Using Jenkinsfile (Recommended)

1. In Jenkins, click **New Item**
2. Enter job name: `jenkins-cicd-learn`
3. Select **Pipeline** and click **OK**
4. In the pipeline configuration:
   - Scroll to **Pipeline**
   - Select **Definition**: "Pipeline script from SCM"
   - Select **SCM**: "Git"
   - Enter your repository URL:
     ```
     https://github.com/YOUR_USERNAME/jenkins-cicd-learn.git
     ```
   - Specify branch: `*/main` or `*/master`
5. Click **Save**
6. Click **Build Now** to run the pipeline

### Method 2: Direct Pipeline Script

1. In Jenkins, click **New Item**
2. Enter job name: `jenkins-cicd-learn`
3. Select **Pipeline** and click **OK**
4. In pipeline configuration, select **Definition**: "Pipeline script"
5. Copy and paste the content from the `Jenkinsfile` in this project
6. Click **Save**
7. Click **Build Now**

---

## Part 6: Set Up GitHub Webhook (Auto-trigger Builds)

### Step 1: Configure Jenkins to Receive Webhooks

1. Go to **Manage Jenkins** → **Configure System**
2. Find **GitHub** section
3. Check **Specify another hook URL for GitHub notifications**
4. Copy the webhook URL:
   ```
   http://YOUR_JENKINS_URL/github-webhook/
   ```
5. Click **Save**

### Step 2: Add Webhook in GitHub

1. Go to your GitHub repository
2. Click **Settings** → **Webhooks** → **Add webhook**
3. Fill in:
   - **Payload URL**: `http://YOUR_JENKINS_URL/github-webhook/`
     - Note: If testing locally, use ngrok: `https://your-ngrok-url.ngrok.io/github-webhook/`
   - **Content type**: `application/json`
   - **Which events would you like to trigger this webhook?**: Select "Just push the event"
4. Click **Add webhook**

### Step 3: Configure Job to Use Webhook

1. In your Jenkins job configuration
2. Under **Build Triggers**, check **GitHub hook trigger for GITScm polling**
3. Click **Save**

---

## Part 7: Test the CI/CD Pipeline

1. Make a small change to your code locally
2. Commit and push:
   ```bash
   git add .
   git commit -m "Test CI/CD trigger"
   git push origin main
   ```
3. Check Jenkins - a new build should start automatically!
4. Watch the pipeline execute each stage

---

## Troubleshooting

### Issue: Jenkins can't clone GitHub repository
- Go to **Manage Jenkins** → **Manage Credentials**
- Add GitHub credentials (username and personal access token)

### Issue: Build fails at npm install
- Make sure Node.js plugin is installed and configured
- Check the Node.js path in **Manage Jenkins** → **Global Tool Configuration**

### Issue: Webhook not working (local Jenkins)
- Use [ngrok](https://ngrok.com/) to expose local Jenkins to the internet
- Run: `ngrok http 8080`
- Use the ngrok URL in GitHub webhook

---

## Useful Commands

```bash
# Check Jenkins status (macOS)
brew services list | grep jenkins

# Restart Jenkins
brew services restart jenkins-lts

# View Jenkins logs
tail -f /Users/navneet/.jenkins/logs/jenkins.log
```

