# DevOps Practicals 1–6 Complete Commands & Configs

---

## PRACTICAL 1: Git Commands

### Part A: Basic Commands
```bash
git config --global user.name "Pratik4875"
git config --global user.email "pratik.dk@somaiya.edu"
git init
cd desktop
mkdir 15_pratik
cd 15_pratik
nano app.py
git init
git add .
git commit -m "new mess"
git remote add origin "https://github.com/Pratik4875/15_pratik.git"
git push -u origin master
```

### Part B: Branching and Merging
```bash
git checkout -b login
git pull origin master
nano login.py
git status
git add login.py
git status
git commit -m "Added a mess"
git status
git checkout master
git merge login
git status
git log --oneline --all
git push origin master
```

---

## PRACTICAL 2: Jenkins Pipeline

1. **Git Executable Path (Manage Jenkins -> Tools -> Git):**
```text
C:\Program Files\Git\bin\git.exe
```

2. **`C:\Program Files\Jenkins\jenkins.xml` (Replace `<arguments>` line):**
```xml
<arguments>-Xrs -Xmx256m -Dhudson.plugins.git.GitSCM.ALLOW_LOCAL_CHECKOUT=true -Dhudson.lifecycle=hudson.lifecycle.WindowsServiceLifecycle -jar "C:\Program Files\Jenkins\jenkins.war" --httpPort=8080 --webroot="%ProgramData%\Jenkins\war"</arguments>
```
*(Restart Jenkins: `Win + R` -> `services.msc` -> Jenkins -> Restart)*

3. **Local Repo Init (in Git Bash):**
```bash
pwd
cd 15_pratik
git init
git add .
git commit -m "this is my first commit"
```

4. **Blue Ocean Repository URL & Step:**
```text
file:///C:/Users/Admin/15_pratik
```
- Step (**Windows Batch Script**):
```bat
echo "pratik"
```

---

## PRACTICAL 3: Docker

### Phase 1: Windows / VMware Setup (CMD as Administrator)
```cmd
.\bcdedit.exe /set hypervisorlaunchtype off
.\reg.exe add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v EnableVirtualizationBasedSecurity /t REG_DWORD /d 0 /f
.\reg.exe add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity" /v Enabled /t REG_DWORD /d 0 /f
```
*(Restart -> Windows Security -> Core isolation -> Memory integrity OFF -> VMware Processors = 4 + Virtualize Intel VT-x/EPT)*

```cmd
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

### Phase 2: Verify Docker (PowerShell as Admin)
```powershell
docker run hello-world
```

### Phase 3: Build & Run Website Container (Git Bash)
```bash
cd desktop
git clone https://github.com/Pratik4875/mini.git
cd mini
nano Dockerfile
```

**`Dockerfile` contents:**
```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
```

**Build & Run (`http://localhost:8081`):**
```bash
docker build -t mini .
docker run -d -p 8081:80 --name mini mini
docker ps
```

---

## PRACTICAL 4: CI/CD using Git, Jenkins, and Docker

### Phase 1: Clone & Create `Dockerfile` (Git Bash)
```bash
cd /c/Users/Admin/Downloads
git clone https://github.com/Pratik4875/mini.git
cd mini
ls
nano Dockerfile
cat Dockerfile
git add .
git commit -m "Add Dockerfile"
```

### Phase 2: Add Docker to Windows System PATH
1. In CMD: `where docker` (e.g. `C:\Users\Admin\AppData\Local\Programs\DockerDesktop\resources\bin`)
2. `Win + R` -> `sysdm.cpl` -> **Advanced -> Environment Variables -> System variables -> Path -> Edit -> New** -> paste path -> OK
3. `Win + R` -> `services.msc` -> **Jenkins** -> **Restart**

### Phase 3: Blue Ocean 3-Stage Pipeline (`file:///C:/Users/Admin/Downloads/mini`)
- **Stage 1: `Build`** (Windows Batch Script)
```bat
where docker
docker --version
docker build -t mini .
```
- **Stage 2: `Run Docker Container`** (Windows Batch Script)
```bat
docker rm -f mini1 >nul 2>&1 & docker run -d -p 8081:80 --name mini1 mini
```
- **Stage 3: `Verify the Docker Container`** (Windows Batch Script)
```bat
docker ps
```
*(Open `http://localhost:8081`)*

---

## PRACTICAL 5: Kubernetes

*(Enable Kubernetes in Docker Desktop first)*
```powershell
kubectl version --client
kubectl cluster-info
kubectl create deployment nginx --image=nginx
kubectl get nodes
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl get pods
kubectl get service nginx
kubectl port-forward svc/nginx 8082:80
```
*(Open `http://localhost:8082` in browser, then press `Ctrl+C`)*
```powershell
kubectl get all
kubectl scale deployment nginx --replicas=3
kubectl get pods
kubectl get deployment nginx
```

---

## PRACTICAL 6: Ansible

### Phase 1: Install in WSL Ubuntu
```bash
wsl --install
# Open Ubuntu:
sudo apt update
sudo apt upgrade -y
sudo apt install ansible -y
ansible --version
mkdir ansible-practical
cd ansible-practical
nano playbook.yml
```

### Phase 2: `playbook.yml` (Keep exact 2-space indentation!)
```yaml
- name: Basic Ansible Automation
  hosts: localhost
  connection: local
  tasks:
    - name: Create a text file
      file:
        path: /home/pratik/ansible_file.txt
        state: touch

    - name: Write content to a file
      copy:
        content: "This file is created using Ansible automation"
        dest: /home/pratik/ansible_message.txt
```

### Phase 3: Run & Verify
```bash
ansible-playbook playbook.yml
ls -l /home/pratik/
cat /home/pratik/ansible_message.txt
```
