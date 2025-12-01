# Docker Build Instructions:

---
## 0. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Have WSL Debian ready
- Check `☑ Use WSL 2 instead of Hyper-V`

---
## 1. Configure Docker Desktop
Tell Docker Desktop to "talk" to my specific Debian distro.
1. Open **Docker Desktop Dashboard** on Windows.
2. Go to **Settings (⚙ Gear Icon)** -> **Resources** -> **WSL Integration**.
3. Flip the switch for **Debian** (or whatever the distro is named) to **ON*.
4. Click **Apply & Restart**.

---
## 2. Verify in Debian
Open Debian terminal and type:
```bash
docker buildx version
```
If this outputs a version number, Docker Desktop is successfully bridged into WSL2 instance.

---
## 3. The Build Workflow (Correct Way)
Run these commands *inside* Debian terminal:
```bash
# 1. Create a directory in the LINUX file system (Home)
mkdir -p ~/baremetal-c
cd ~/baremetal-c

# 2. Put Dockerfile and Makefile here
# (Can use 'code .' to open VS Code from here if VS Code is installed)

# 3. Authenticate
docker login
```

---
## 4. Create the builder (only needs to be done once) 
One-time setup that upgrades Docker environment from "standard mode" to "multi-architecture factory mode."

run:
```bash
docker buildx ls
```
- If you see `class_builder` with a star (`*`) next to it, you are good. You don't need to run the command again.

- If you only see `default`, you need to run the command.

The command is:
```bash
docker buildx create --name class_builder --use --bootstrap
```

---
## 5. Add `Dockerfile`
Put this in `Dockerfile` inside `~/baremetal-c`
```dockerfile
# =================================================================
# Dockerfile for BareMetal C (Z80)
# =================================================================

# 1. Base Image: Official Debian stable slim version
FROM debian:trixie-slim

# 2. Metadata: So people know who maintains this
LABEL maintainer="BareMetal C Instructor"
LABEL description="Z80 Build Environment (SDCC, z80dasm, Make)"

# 3. Environment Setup
# Prevent apt from asking interactive questions during build
ENV DEBIAN_FRONTEND=noninteractive

# 4. Install Tools
# We combine update + install + cleanup in one RUN command to keep the image small.
RUN apt-get update && apt-get install -y \
    git sdcc z80dasm make perl less man-db manpages bash-completion \
    nano vim \
    && rm -rf /var/lib/apt/lists/*

# 5. Fix Man Pages
# Docker slim images often exclude man pages. We re-enable them so students
# can type 'man sdcc' or 'man z80dasm'.
RUN sed -i '/path-exclude=\/usr\/share\/man/d' /etc/dpkg/dpkg.cfg.d/docker && \
    mandb

# 6. Customize the Prompt
# This changes the prompt color and text so students know they are
# inside the container (and not on their Mac/Windows host).
RUN echo "export PS1='\[\033[01;33m\][baremetal-c]\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '" >> /root/.bashrc

# 7. Set Working Directory
# This is where the volume will be mounted by default.
WORKDIR /code

# 8. Default Command
# Drop the user into a bash shell when they run the container.
CMD ["/bin/bash"]
```
then
```
# 5. Build and Push for both platforms
# (Note: Using the Linux CLI syntax)
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t kongkrit/baremetal-c:latest \
  --push .
```

---
## 6. Updating images
- Edit `Dockerfile`
- rebuild: **Update version number (shown as 1.1 below)**
```bash
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t kongkrit/baremetal-c:v1.1 \
  -t kongkrit/baremetal-c:latest \
  --push .
```

---
## 7. Undoing stuff - delete a dud push to docker hub
You generally cannot delete pushed images from the command line. You must use the Docker Hub Website.

Here is the step-by-step process:

1. Log in to [hub.docker.com](https://hub.docker.com).
2. Click on **Repositories** in the top menu.
3. Click on your repository name (e.g., `yourusername/baremetal-c`).
4. Click on the **"Tags"** tab (this is the important part; do not look for a delete button on the main page).
5. Find the specific version tag you want to remove (e.g., `v1` or `oops-broken`).
6. Check the box next to that tag.
7. Click the **Delete** button that appears.

**Critical Warning for your Class**

**Do not delete the `latest` tag** unless you immediately plan to push a new one.

If a student tries to run `docker run ... yourname/baremetal-c` and the `latest` tag is missing, the command will fail with a "manifest not found" error, and you will get emails from confused freshmen.

- **To fix a bad version:** Just rebuild and push again. It will automatically overwrite the old `latest`.
- **To remove a specific bad tag (e.g., `v2`):** Delete it via the website using the steps above.
