
# Docker installation on Ubuntu (Amazon EC2)

This guide shows how to install Docker Engine and Docker Compose plugin on Ubuntu running in an Amazon EC2 instance. Copy and paste the commands (run as a user with `sudo` privileges).

**Prerequisites**
- An Ubuntu EC2 instance with a user that can run `sudo`.
- Allow outbound HTTPS in the instance's security group (required to download packages).

**1) Remove old/conflicting packages**
Run this to remove older Docker or container runtimes that might conflict:

```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
```

**2) Add Docker's official GPG key and apt keyring**

```bash
sudo apt update
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

**3) Add the Docker APT repository**

This uses your distribution codename (works on standard Ubuntu EC2 AMIs):

```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

**4) Install Docker Engine and Compose plugin**

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

**5) Enable and start Docker**

```bash
sudo systemctl enable --now docker
```

**6) Post-install: manage Docker as a non-root user (optional, be cautious)**
Add your user to the `docker` group so you can run Docker without `sudo`:

```bash
sudo usermod -aG docker $USER
# either log out and back in, or run:
newgrp docker
```

Security note: adding a user to the `docker` group grants effectively root access to the system; only add trusted users.

**7) Verify installation**

```bash
docker --version
docker compose version
sudo systemctl status docker --no-pager
docker run --rm hello-world
```

If `docker run hello-world` fails when running without `sudo`, try with `sudo` until group membership is refreshed:

```bash
sudo docker run --rm hello-world
```


**Troubleshooting**
- If apt complains about the repository signature, confirm `/etc/apt/keyrings/docker.asc` exists and is readable.
- On some Ubuntu kernels or older AMIs you may need additional kernel modules or a newer AMI. Consider upgrading the instance if you encounter low-level runtime errors.

**Cleanup / Uninstall**
To remove Docker packages later:

```bash
sudo apt remove -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo rm -rf /var/lib/docker /var/lib/containerd
```

**Notes for EC2**
- EC2 security groups do not need special inbound rules for Docker itself; containers expose ports you explicitly map (e.g., `8080` above) and you must open those ports in the security group.
- Be mindful of IAM roles and credentials; Docker operations run on the instance and are independent of AWS IAM (unless you configure credential helpers or cloud integrations).

