# Readme for Todo App 📝

### Installation 🚀

1. **Install Node.js and NPM on Ubuntu:**
   - Make sure you have Node.js 16.x and NPM installed on your machine. If not, you can install them using the following commands:
     ```bash
     curl -s https://deb.nodesource.com/setup_16.x | sudo bash
     sudo apt install nodejs -y
     ```

### Configuration ⚙️

2. **Update Backend URL:**
   - Open the `src/TodoApp.js` file.
   - Locate the variable storing the backend URL and update it with the appropriate value. (* See Below for PrivateIp Configuration)

### Building and Running 🏗️

3. **Install Dependencies:**
   - Run the following command to install project dependencies:
     ```bash
     npm install
     ```

4. **Build the Project:**
   - Execute the following command to build the project:
     ```bash
     npm run build
     ```

### Deployment 🚀

5. **Deploy to Nginx Server:**
   - Copy the generated artifacts from the build process.
   - Deploy the artifacts to your Nginx server. Ensure that the server is properly configured to serve the application.

## * Using Private IP on the Backend VM 🌐

To use a Private IP on the Backend VM, follow the steps below:

### 1. Configure NGINX on the Backend VM ⚙️

Open the NGINX configuration file:

```bash
sudo nano /etc/nginx/sites-available/default
```

### 2. Insert Proxy Configuration 🔄

Copy and paste the following code just above the `root /var/www/html` line:

```nginx
location /api {
   proxy_pass http://<PrivateIP of BackendVM>:8000;
   proxy_set_header Host $host;
   proxy_set_header X-Real-IP $remote_addr;
   proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   proxy_set_header X-Forwarded-Proto $scheme;
}
```

Replace `<PrivateIP of BackendVM>` with the actual Private IP address of your Backend VM.

### 3. Update Frontend Configuration ⚙️

Open the `src/TodoApp.js` file in your frontend project.

Update the Backend URL by replacing the existing line with the following:

```javascript
const API_BASE_URL = 'http://<FrontendVM Public IP>:80/api';
```

Replace `<FrontendVM Public IP>` with the actual Public IP address of your Frontend VM.

## Important Note 📌

Make sure to restart NGINX on the VM after making the changes:

```bash
sudo service nginx restart
```

These configurations enable communication between the Frontend and Backend using Private IP on the Backend VM. Ensure that the IPs and ports are correctly set to match your environment.

#######################################################################################
# 🚀 React Todo UI CI/CD Pipeline

This repository demonstrates an end-to-end CI/CD pipeline for a React application using Azure DevOps. It includes automated build, artifact publishing, and deployment to a Linux Virtual Machine via SSH.

---

## 📁 Project Structure

```
ReactTodoUIMonolithic-surendra/
│
├── azure-pipelines1.yml         # CI/CD pipeline definition
├── src/                         # React application source code
├── public/                      # Static assets
├── package.json                 # Project dependencies
├── README.md                    # Documentation
```

---

## 📌 Overview

The pipeline is divided into two main stages:

1. **CI (Continuous Integration)** → Build the React application and publish artifacts  
2. **CD (Continuous Deployment)** → Deploy the application to a Linux VM  

---

## ⚙️ Pipeline Configuration

- **Pipeline Name:** ReactTodo Pipeline  
- **Trigger:** Manual (`trigger: none`)  
- **Agent Pool:** Simple  

---

## 🧱 CI Stage (Build)

### 🔹 Steps:

1. Install dependencies
```bash
npm install
```

2. Build application
```bash
npm run build
```

3. Publish artifacts
- Output folder: `build/`
- Artifact name: `Todo-reactUI-artifacts`

---

## 🚀 CD Stage (Deploy to VM)

### 🔹 Steps:

1. Download build artifacts from pipeline  
2. Copy files to VM using SSH  
3. Deploy files to web server directory  

---

## 🖥️ Deployment Flow

```
Code → Build → Artifact → VM → /var/www/html
```

---

## 🔐 Tools & Tasks Used

- PowerShell Task → Run build commands  
- PublishPipelineArtifact → Store build output  
- DownloadPipelineArtifact → Retrieve artifacts  
- CopyFilesOverSSH → Transfer files to VM  
- SSH Task → Execute deployment commands  

---

## 🎯 Use Case

- Automating React application deployment  
- Implementing CI/CD using Azure DevOps  
- Deploying static frontend to Linux VM  
- Demonstrating real-world DevOps workflow  

---

## 💡 Key Learnings

- CI/CD pipeline using YAML  
- Artifact-based deployment strategy  
- Secure deployment using SSH  
- Separation of CI and CD stages  

---

## 🚀 Future Enhancements

- Enable automatic triggers (on code push)  
- Add multi-environment support (Dev/Prod)  
- Configure Nginx for production  
- Implement rollback strategy  

---

🔥 *From code to deployment — complete DevOps pipeline in action*
