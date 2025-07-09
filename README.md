# Installing Docker and Docker Compose on Ubuntu via WSL2 (Alternative to Docker Desktop)

Docker Desktop is a popular tool but can consume significant system resources and sometimes crash. This guide shows how to install Docker and Docker Compose on Ubuntu running inside WSL2, and optionally run Portainer for a lightweight, visual Docker management interface.

---

## Step 1: Enable WSL and Set to WSL 2

1. Open PowerShell as Administrator.
2. Enable WSL feature:

   ```powershell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   ```
3. Enable Virtual Machine Platform feature:

   ```powershell
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
4. Set WSL 2 as the default version:

   ```powershell
   wsl --set-default-version 2
   ```
5. Restart your computer.

---

## Step 2: Install Ubuntu LTS from Microsoft Store

1. Open Microsoft Store.
2. Search for "Ubuntu 22.04 LTS" (or latest LTS version).
3. Install it.
4. Launch Ubuntu from the Start Menu and complete the initial setup (create username and password).

---

## Step 3: Update Ubuntu and Install Docker

1. Inside the Ubuntu terminal, update packages:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Install required packages:

   ```bash
   sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
   ```
3. Add Docker’s official GPG key:

   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```
4. Add Docker repository:

   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```
5. Update package index and install Docker:

   ```bash
   sudo apt update
   sudo apt install docker-ce docker-ce-cli containerd.io -y
   ```
6. Start Docker service:

   ```bash
   sudo service docker start
   ```
7. Add your user to the `docker` group to run Docker without `sudo`:

   ```bash
   sudo usermod -aG docker $USER
   ```
8. Close and reopen the Ubuntu terminal to apply group changes.

---

## Step 4: Install Docker Compose

1. Download the latest Docker Compose binary:

   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```
2. Apply executable permissions:

   ```bash
   sudo chmod +x /usr/local/bin/docker-compose
   ```
3. Verify installation:

   ```bash
   docker-compose --version
   ```

---

## Step 5 (Optional): Run Portainer for Visual Docker Management

Portainer provides a lightweight GUI for managing Docker containers.

1. Pull the Portainer image:

   ```bash
   docker pull portainer/portainer-ce
   ```
2. Create and run the Portainer container:

   ```bash
   docker volume create portainer_data
   docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
   ```
3. Open your browser and navigate to `http://localhost:9000`.
4. Set up your admin user and connect to the local Docker environment.

---

## Summary

* Enabled and configured WSL 2 on Windows.
* Installed Ubuntu LTS from Microsoft Store.
* Installed Docker and Docker Compose inside Ubuntu.
* Optionally set up Portainer as a lightweight Docker GUI.
* This setup consumes fewer resources than Docker Desktop and avoids common crashes.

---

If you want, I can help you format this nicely for Word, or add screenshots and tips — just say! Would you like me to generate a formatted version or include commands in code blocks?
