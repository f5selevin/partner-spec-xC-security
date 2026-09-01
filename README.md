# Partner Specialisation: XC (Security) Lab Guide

This lab guide combines and adapts material, structure, and ideas from classes in the following F5 repositories:

- [`f5devcentral/f5-agility-labs-xc`](https://github.com/f5devcentral/f5-agility-labs-xc) — Classes 2, 4, and 5 from that repository.
- [`f5devcentral/f5xc-emea-workshop`](https://github.com/f5devcentral/f5xc-emea-workshop) — Classes 1, 3, and 4 from that repository.

These class numbers identify the source material within each original repository; they do not map to the class or section numbers in this lab guide. The content has been assembled and adapted rather than directly reproduced.

## Install the lab guide server without cloning the repository

Download both scripts from GitHub into the same temporary directory, then run the registration script:

```bash README.md
SCRIPT_DIR="$(mktemp -d)"
BASE_URL="https://raw.githubusercontent.com/f5selevin/partner-spec-xC-security/main/udf/labguide"

curl -fsSL "${BASE_URL}/create-labguide-server.sh" \
  -o "${SCRIPT_DIR}/create-labguide-server.sh" && \
curl -fsSL "${BASE_URL}/register-labguide-server.sh" \
  -o "${SCRIPT_DIR}/register-labguide-server.sh" && \
chmod +x "${SCRIPT_DIR}"/*.sh && \
sudo "${SCRIPT_DIR}/register-labguide-server.sh"

rm -rf "${SCRIPT_DIR}"
```

The registration script installs and starts a systemd service as the `ubuntu` user. To use another existing account, set `SERVICE_USER` on the `sudo` command before removing the temporary directory:

```bash README.md
sudo SERVICE_USER="$USER" "${SCRIPT_DIR}/register-labguide-server.sh"
rm -rf "${SCRIPT_DIR}"
```
