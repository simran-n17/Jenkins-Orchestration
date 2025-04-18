# Python Application with Jenkins CI/CD Pipeline

## 1. Project Overview

This project demonstrates a simple Python application with a complete CI/CD pipeline using Jenkins, Docker, and GitHub. The application provides a command-line tool that adds two numbers together, with automated testing and continuous integration/deployment.

> **Important**: Make sure Docker Desktop is running in the background before starting any Docker-related commands.

## 2. Components

### Project Structure
```
.
├── sources/
│   ├── add2vals.py      # Main application entry point
│   ├── calc.py          # Core calculation module
│   └── test_calc.py     # Unit tests
├── dist/               # Compiled executables
├── requirements.txt    # Python dependencies
├── Jenkinsfile        # Pipeline configuration
├── docker-compose.yml # Docker setup
└── README.md         # This documentation
```

### Python Application
- `add2vals.py`: Main application file that handles command-line arguments and displays results
- `calc.py`: Core calculation module containing the addition logic
- `test_calc.py`: Test suite with unit tests for the calculation module
- `requirements.txt`: Python dependencies with specific versions

### Jenkins Pipeline
- `Jenkinsfile`: Pipeline configuration defining the CI/CD workflow
- `docker-compose.yml`: Jenkins and agent setup with volume mappings and network configuration

### Docker Configuration
- Jenkins container: Main Jenkins server with persistent storage
- Jenkins agent container: For distributed builds
- Python build container: For building and testing the application

## 3. Steps to Perform This Project

### Step 1: Setup Jenkins

1. Pull Jenkins image and start containers:
 ```bash
   docker-compose up -d
 ```

<div align="center">
  <img src="images/img-1.png" alt="push and pull">
</div>

2. Get initial admin password:
 ```bash
   docker-compose exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
 ```

<div align="center">
  <img src="images/img-2.png" alt="Get initial admin password">
</div>

3. Access Jenkins:
   - Open browser and go to `http://localhost:8080`
   - Enter the initial admin password from step 2

<div align="center">
  <img src="images/img-3.png" alt="Access Jenkins">
</div>

4. Install plugins:
   - Click "Install suggested plugins"
   - Wait for installation to complete

<div align="center">
  <img src="images/img-4.png" alt="Install plugins 1">

  <img src="images/img-5.png" alt="Install plugins 2">
</div>

5. Create first admin user:
   - Enter your desired username
   - Enter your password
   - Enter your email
   - Click "Save and Continue"

<div align="center">
  <img src="images/img-6.png" alt="Create admin user">
</div>

6. Configure Jenkins instance:
   - Keep the default URL: `http://localhost:8080/`
   - Click "Save and Finish"

<div align="center">
  <img src="images/img-7.png" alt="Configure Jenkins 1">

  <img src="images/img-8.png" alt="Configure Jenkins 2">
</div>

7. Create Pipeline Project:
   - Click "New Item"
   - Enter project name: `simple-python-pyinstaller-app`
   - Select "Pipeline"
   - Click "OK"
   - In pipeline configuration:
     - Scroll to "Pipeline" section
     - Select "Pipeline script from SCM"
     - Select "Git" as SCM
     - Enter repository URL: `https://github.com/simran-n17/Jenkins-Orchestration.git`
     - Enter branch specifier: `*/master`
     - Click "Save"

<div align="center">
  <img src="images/img-9.png" alt="Create pipeline 1">

  <img src="images/img-10.png" alt="Create pipeline 2">
</div>

8. Install and Configure Docker in Jenkins Container:
 ```bash
   # Install Docker
   docker-compose exec jenkins bash -c "apt-get update && apt-get install -y docker.io"
   
   # Start Docker service
   docker-compose exec jenkins bash -c "service docker start"
   
   # Verify Docker installation
   docker-compose exec jenkins docker --version
 ```

<div align="center">
  <img src="images/img-11.png" alt="Install Docker 1">

  <img src="images/img-12.png" alt="Install Docker 2">

  <img src="images/img-13.png" alt="Install Docker 3">

  <img src="images/img-14.png" alt="Install Docker 4">
</div>

9. Install Docker Plugins:
    - Go to "Manage Jenkins" > "Manage Plugins"
    - Click "Available" tab
    - Search for and install:
      - Docker Pipeline
      - Docker plugin
      - docker-build-step

<div align="center">
  <img src="images/img-15.png" alt="Install Docker plugins 1">

  <img src="images/img-16.png" alt="Install Docker plugins 2">

  <img src="images/img-17.png" alt="Install Docker plugins 3">

  <img src="images/img-18.png" alt="Install Docker plugins 4">
</div>

   - Restart Jenkins after installation:
     ```bash
      # Restart Jenkins container
      docker-compose restart jenkins
      
      # Wait for Jenkins to start (about 30 seconds)
      # Then verify Jenkins is running
      docker-compose ps
     ```

<div align="center">
  <img src="images/img-19.png" alt="Restart Jenkins 2">
</div>
    
10. Sign in to Jenkins:
    - Use the credentials you created in step 5

<div align="center">
  <img src="images/img-20.png" alt="Sign in to Jenkins">
</div>

11. Run the Pipeline:
    - Go to your pipeline project
    - Click "Build Now"

<div align="center">
 <img src="images/img-21.png" alt="Download executable 1">

  <img src="images/img-22.png" alt="Download executable 2">

  <img src="images/img-23.png" alt="Download executable 3">
</div>

### Step 2: Testing the Executable

1. Download the executable from Jenkins:
   - Go to Jenkins dashboard
   - Click on `simple-python-pyinstaller-app`
   - Click on the latest build number
   - Look for "Build Artifacts" section
   - Click on `add2vals` to download it to your local machine

<div align="center">
  <img src="images/img-24.png" alt="Download executable 4">
</div>

   Note: The executable downloaded from Jenkins will be a Linux version since Jenkins runs in a Linux container. 

2. To run the Linux executable on Windows using WSL:

```bash
   # Check if WSL is installed
   wsl --version
   
   # If WSL is not installed, you'll get an error
   # In that case, install WSL:
   wsl --install
   
   # After installation, restart your computer
   # Then open PowerShell and verify WSL is installed:
   wsl --version

   # To open WSL terminal, you have three options:
   # Option 1: Open PowerShell and type:
   wsl
   
   # Option 2: Press Windows + R, type 'wsl' and press Enter
   
   # Option 3: Click Start menu, type 'wsl' and click on 'Windows Subsystem for Linux'
   
   # Navigate to your directory where you downloaded `add2vals` in WSL
   # Note: In WSL, Windows paths are accessed through /mnt/
   cd /mnt/c/Users/asus/OneDrive/Downloads
   
   # Make the file executable
   chmod +x add2vals
   
   # Run the executable
   ./add2vals 5 3
```

<div align="center">
</div>

## 4. What Does PyInstaller Do?

PyInstaller is used to create a standalone executable from the Python application. Here's how it works:

1. **Analysis Phase**
   - Analyzes `add2vals.py` and its dependencies
   - Identifies required Python files, libraries, and resources
   - Creates a dependency graph of the application

2. **Bundling Phase**
   - Packages all identified dependencies
   - Includes Python interpreter
   - Bundles required libraries and resources
   - Creates a single executable file

3. **Output**
   - Executable is created in the `dist` directory
   - Named `add2vals` (or `add2vals.exe` on Windows)
   - Contains everything needed to run the application

### Benefits of PyInstaller
1. **Portability**
   - Single executable file
   - No Python installation required
   - Easy distribution

2. **Dependency Management**
   - All dependencies included
   - No external package installation needed
   - Consistent environment

3. **Cross-Platform Support**
   - Creates platform-specific executables
   - Works on Windows, Linux, and macOS
   - Native performance

## 5. Conclusion

This project demonstrates a complete CI/CD pipeline setup using Jenkins, Docker, and Python. By following these steps, you have:

1. Set up a Jenkins server with Docker support
2. Created a pipeline that automatically:
   - Builds your Python application
   - Runs tests
   - Creates a standalone executable
3. Generated a distributable application that can run on any system
 
### Thank You
Thank you for following this tutorial! We hope it has helped you understand:
- How to set up a CI/CD pipeline with Jenkins
- How to create standalone executables with PyInstaller
- How to use Docker for containerization
- How to automate Python application builds and tests

Feel free to:
- Star this repository if you found it helpful
- Share it with others who might benefit
- Contribute improvements or suggestions
- Report any issues you encounter

Happy coding! 🚀
