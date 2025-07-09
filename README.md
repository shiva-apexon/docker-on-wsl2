# Installing Docker and Docker Compose on Ubuntu via WSL2 (Alternative to Docker Desktop)

Docker Desktop is a popular tool but can consume significant system resources and sometimes crash. This guide shows how to install Docker and Docker Compose on Ubuntu running inside WSL2, and optionally run Portainer for a lightweight, visual Docker management interface.

---

## Step 1: Set to WSL 2 and Install Ubuntu LTS from Microsoft Store

1. Open PowerShell as Administrator.
2. Set WSL 2 as the default version:

   ```powershell
   wsl --set-default-version 2
   ```
3. Restart your computer.
4. List out the Linux distributions
   ```powershell
   wsl --list --online
   ```
5. Install a version using a NAME from the output
   ```powershell
   wsl --install -d Ubuntu-24.04
   ```
6. List currently installed distros and the version of WSL
   ```powershell
   wsl -l -v 
   ```
---

## Step 2: Update Ubuntu, Install Docker and Docker Compose

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
5. Update package index and install Docker and Docker Compose:

   ```bash
   sudo apt update
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```
6. Check Docker service status:

   ```bash
   systemctl status docker
   ```
7. Add your user to the `docker` group to run Docker without `sudo`:

   ```bash
   sudo usermod -aG docker $USER
   ```
8. Close and reopen the Ubuntu terminal to apply group changes.

---

## Step 3 (Optional): Run Portainer for Visual Docker Management

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

## References

* https://documentation.ubuntu.com/wsl/stable/howto/install-ubuntu-wsl2/
* https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
* https://docs.docker.com/engine/install/linux-postinstall/
* https://docs.docker.com/compose/install/linux/#install-using-the-repository
* https://support.netfoundry.io/hc/en-us/articles/360057865692-Installing-Docker-and-docker-compose-for-Ubuntu-20-04
