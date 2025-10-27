# 🧪 API Test Automation with Postman, Newman & Jenkins

![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Jenkins](https://img.shields.io/badge/Jenkins-Automation-blue)
![Postman](https://img.shields.io/badge/Postman-API%20Testing-orange)
![Newman](https://img.shields.io/badge/Newman-CLI%20Runner-yellow)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

> Continuous API testing pipeline using **Postman**, **Newman**, and **Jenkins**, integrated with **GitHub Webhooks** for automatic test execution and HTML reporting.

---

## 📘 Overview

This project automates API testing using:
- **Postman Collection** — defines the API test cases  
- **Newman** — CLI runner to execute Postman collections  
- **Jenkins** — CI/CD pipeline automation  
- **GitHub Webhook** — triggers tests automatically on every push  

Every code push automatically runs Postman API tests through Jenkins and generates a detailed HTML report.

---

## 📂 Project Structure

API_Test_Automation/
│
├── UserAPITests.postman_collection.json # Postman API test cases
├── package.json # Dependencies
├── package-lock.json
├── reports/
│ └── report.html # Generated HTML report
└── Jenkinsfile or Freestyle Job Config



---

## ⚙️ Step 1 — Local Setup

### 1️⃣ Install Node.js and Newman
```bash
npm install -g newman
npm install -g newman-reporter-html
2️⃣ Verify installation

newman -v
💻 Step 2 — Setup Project Dependencies
Initialize your Node.js project:

npm init -y
Install dependencies locally:

npm install newman newman-reporter-html --save-dev

🧩 Step 3 — Run API Tests Locally

npx newman run UserAPITests.postman_collection.json -r cli,html --reporter-html-export reports/report.html
✅ Output:

Test results printed in terminal

HTML report generated at reports/report.html

⚡ Step 4 — Jenkins Setup (on Localhost)
🧱 Required Plugins:
NodeJS Plugin

Git Plugin

HTML Publisher Plugin

🧰 Freestyle Job Configuration
Job Name: API_Test_Automation

Build Step:

copy "C:\Users\Nandkishor\Documents\UserAPITests.postman_collection.json" "%cd%"
mkdir reports
npx newman run UserAPITests.postman_collection.json -r cli,html --reporter-html-export reports\report.html
Post-Build Action (Choose one):

✅ Archive the Artifacts

reports/*.html
or

✅ Publish HTML Reports

HTML directory to archive: reports

Index page: report.html

🌐 Step 5 — GitHub Webhook Integration
In Jenkins:
Go to Manage Jenkins → Configure System → GitHub

Add your GitHub credentials (PAT token)

In Your Job Configuration:
Enable:


GitHub hook trigger for GITScm polling
In GitHub Repository:
Go to Settings → Webhooks → Add Webhook

Payload URL:

http://<your-local-ip>:8080/github-webhook/
(If running locally, expose Jenkins via ngrok)

Content type: application/json

Trigger: “Just the push event”

Expose your Jenkins using:

ngrok http 8080
Then copy the generated link:

https://<your-ngrok-url>/github-webhook/
and use it in GitHub webhook.

🧾 Step 6 — Validate Your Pipeline
Click Build Now manually, or

Push changes to GitHub (to trigger automatically)

Jenkins will:

Run Postman API tests via Newman

Generate the HTML report

Archive or publish the report automatically

🧠 Step 7 — View the HTML Test Report
Go to your Jenkins job

Open your latest build (#1, #2, etc.)

Under Artifacts, click report.html

or, if using Publish HTML:
👉 Click “Postman HTML Report” link in the left sidebar

✅ Example Build Output

Build: #12
Status: SUCCESS
Duration: 35s


Artifacts:
📄 reports/report.html
🧾 Troubleshooting
Issue	Cause	Solution
❌ newman: command not found	Newman not installed globally	Run npm install -g newman
❌ Report not generated	Wrong output path	Ensure reports/ folder exists
❌ Jenkins can’t find files	Wrong workspace path	Use relative paths
❌ Webhook not triggering	Jenkins running locally	Use ngrok to expose port 8080
