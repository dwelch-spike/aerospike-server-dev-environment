# 🛠 Aerospike Server Dev Environment (Devcontainers + macOS ARM)

This repo contains a ready-to-use **devcontainer-based development environment** for building and testing the [Aerospike Database Server](https://github.com/citrusleaf/aerospike-server) on **macOS with Apple Silicon (M1/M2/M3)** using **Docker and VSCode/Cursor**.
The root of this repository is mounted on the dev container so that changes made to source code cloned in this directory take effect both on the host machine and container.

---

## 🚀 Prerequisites

- macOS (Sonoma or later)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Cursor](https://www.cursor.so/) or [VSCode](https://code.visualstudio.com/)
- `aerospike-server` source checked out:

  ```bash
  git clone https://github.com/citrusleaf/aerospike-server.git

## Folder Structure

```
your-project-root/
├── .devcontainer/
│   ├── Dockerfile           # Defines Ubuntu-based dev image
│   └── devcontainer.json    # Mounts and sets up workspace
├── aerospike-server/        # Your local source clone (you need to clone the server manually)
```

## ⚙️ What This Devcontainer Provides

- Ubuntu 24.04 with:
    - All build tools pre-installed (gcc, cmake, libssl-dev, etc.)
    - SSH server (optional)
    - File descriptor limits configured
- Support for:
    - Git operations via mounted .gitconfig and .ssh keys
    - Running on ARM Mac
- No automatic post-create build — you control when to build/run
- Easily customizable

## First-Time Setup

1. Open Cursor/VScode.
2. Open the root project folder (the one containing `.devcontainer/`).
3. When prompted, choose "Reopen in Container".
4. Once inside the container terminal, run:
   ```bash
   cd /workspace
   ./bin/install-dependencies.sh
   git submodule update --init --recursive
   make -j$(nproc)
   ```
5. You're ready to develop, test, and run the Aerospike Server.

## Host Config Mounting

This setup mounts your Git and SSH settings from the host:

```json
"mounts": [
  "source=${localWorkspaceFolder}/aerospike-server,target=/workspace,type=bind",
  "source=${env:HOME}/.gitconfig,target=/root/.gitconfig,type=bind",
  "source=${env:HOME}/.ssh,target=/root/.ssh,type=bind"
]
```

## SSH Key Permissions

To avoid SSH permission errors inside the container:

```bash
chmod 600 ~/.ssh/id_*
chmod 700 ~/.ssh
```

Also ensure GitHub is added to known hosts:

```bash
ssh-keyscan github.com >> ~/.ssh/known_hosts
```

## Verifying Access

Inside the container terminal:

```bash
git config --global user.name
git config --global user.email
ssh -T git@github.com
```

## Helpful Docker Commands (optional)

To rebuild the image manually:

```bash
devcontainer build --no-cache
```

Or with Docker directly:

```bash
docker build -t aerospike-dev -f .devcontainer/Dockerfile .
```
