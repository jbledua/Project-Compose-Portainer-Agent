# Project-Compose-Portainer-Agent

## Overview
This project provides a **Docker Compose** stack that securely exposes the **Portainer Agent** via **Tailscale**, allowing remote management of Docker containers over a secure private network.

By using **Tailscale**, the Portainer Agent remains **unexposed to the public internet**, ensuring secure remote access.

---

## Features
- **Secure Access**: Portainer Agent is only accessible via Tailscale.
- **Environment Variables**: Uses `.env` to manage configuration securely.
- **Minimal Setup**: Runs with `docker compose` or via Portainer's Git deployment.
- **Persistence**: Tailscale authentication state is stored for reboots.

---

## Prerequisites
Ensure you have:
- **Docker** & **Docker Compose** installed.
- A **Tailscale account** ([sign up here](https://tailscale.com)).
- A **GitHub account** (if deploying via Portainer).

---

## Setup & Deployment
### 1. Clone the Repository
```bash
git clone https://github.com/jbledua/Project-Compose-Portainer-Agent.git
cd Project-Compose-Portainer-Agent
```

### 2. Configure Environment Variables
Create an `.env` file by copying the example:
```bash
cp .env.example .env
nano .env
```
Edit the `.env` file with your **Tailscale Authentication Key** and **desired hostname**:
```ini
TS_HOSTNAME=portainer-agent
TS_AUTHKEY=tskey-client-xxxxxxxxxxxxxxxxxxxxxx
```
💡 **Get your Tailscale Auth Key** from: [Tailscale Admin Console](https://login.tailscale.com/admin/settings/authkeys).

### 3. Deploy with Docker Compose
```bash
docker compose up -d
```

This will:
- Start a **Tailscale container** to handle networking.
- Start the **Portainer Agent**, exposing it only via Tailscale.

### 4. Verify Connectivity
1. Check if the containers are running:
   ```bash
   docker ps
   ```
2. Get the **Tailscale IP**:
   ```bash
   docker exec -it $(docker ps -qf "name=tailscale-portainer") tailscale ip -4
   ```
3. Use **Portainer** to connect to `http://<Tailscale_IP>:9001`.

---

## Deploying via Portainer (Using GitHub Personal Access Token)
If you want to deploy this stack using **Portainer's GitOps feature**, follow these steps:

### 1. Generate a GitHub Personal Access Token (PAT)
1. Go to **[GitHub Personal Access Tokens](https://github.com/settings/tokens)**.
2. Click **Generate New Token (Classic)**.
3. Enable scopes:
   - ✅ `repo` (for private repos, optional for public repos).
4. Click **Generate Token**, and **copy it**.

### 2. Add GitHub Repository to Portainer
1. Open **Portainer**.
2. Navigate to **Stacks** → **+ Add Stack**.
3. Select **Git repository**.
4. Fill in the details:
   - **Repository URL**: `https://github.com/jbledua/Project-Compose-Portainer-Agent.git`
   - **Branch**: `main`
   - **Authentication**: ✅ Enable and enter your GitHub **username** & **Personal Access Token (PAT)**.
5. Click **Deploy the Stack**.

### 3. Verify Deployment
Once deployed, Portainer should show the stack running under **Containers**.

---

## Troubleshooting
### Issue: Portainer Agent is unreachable
✅ **Check if Tailscale is running**:
```bash
docker logs tailscale-portainer
```
✅ **Check Tailscale IPs**:
```bash
docker exec -it tailscale-portainer tailscale status
```
✅ **Ensure the agent is running**:
```bash
docker logs portainer_agent
```

---

## Contributing
Feel free to fork, submit issues, or contribute via PRs!

---

## License
This project is licensed under the MIT License. See `LICENSE` for details.
