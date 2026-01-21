# homelab-portainer

This repo contains a Portainer Stack for:
- PostgreSQL (bitnami)
- pgAdmin
- AdGuard Home
- Vault (bootstrap image)

Run this one command on the microk8s machine — it will create both files exactly where your Vault container expects them:
```
sudo bash -c 'set -e; mkdir -p /home/kalpak44/vault/file; microk8s kubectl config view --raw --minify -o jsonpath="{.clusters[0].cluster.certificate-authority-data}" | base64 -d > /home/kalpak44/vault/file/k8s-ca.crt; SECRET_NAME=$(microk8s kubectl -n kube-system get sa vault-tokenreview -o jsonpath="{.secrets[0].name}"); microk8s kubectl -n kube-system get secret "$SECRET_NAME" -o jsonpath="{.data.token}" | base64 -d > /home/kalpak44/vault/file/tokenreview.jwt; ls -l /home/kalpak44/vault/file/k8s-ca.crt /home/kalpak44/vault/file/tokenreview.jwt'

```
---

## Deploy with Portainer using **Git Repository**

This is the recommended approach because Portainer will automatically pull the stack definition from this repo:

Repository: `https://github.com/kalpak44/homelab-portainer.git`

---

## Create the Stack in Portainer (Git method)

1. Open **Portainer**
2. Go to: **Stacks → Add stack**
3. Select **Git repository**
4. Fill in:

### Repository URL
```
https://github.com/kalpak44/homelab-portainer.git
```

## Load the environment variables

This stack uses variables, so credentials are not hardcoded in the compose file.

1. In the stack creation page, scroll to: **Environment variables**
2. Click: **Load variables from .env file**
3. Upload a file based on `.env.example`

Example `.env` file content:

```env
POSTGRES_USER=admin
POSTGRES_PASSWORD=change_me_strong_password
POSTGRES_DB=root
POSTGRESQL_POSTGRES_PASSWORD: change_me_strong_password

PGADMIN_DEFAULT_EMAIL=admin@admin.com
PGADMIN_DEFAULT_PASSWORD=change_me_strong_password

# Vault (Userpass UI admin login)
VAULT_UI_ADMIN_USERNAME=admin
VAULT_UI_ADMIN_PASSWORD=change_me_strong_password
```

Portainer will inject these values when the stack is deployed.

---

## 3) Deploy the stack
Click **Deploy the stack**.
