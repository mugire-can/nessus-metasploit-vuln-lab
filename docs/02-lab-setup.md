# 02 — Lab Setup

## Environment Overview

| Component | Role | OS / Version |
|---|---|---|
| **Metasploitable2 VM** | Vulnerable target | Ubuntu 8.04 (intentionally outdated) |
| **Debian VM** | Nessus scanner host | Debian 12 (Bookworm) |
| **Network** | Isolated lab network | Host-only / NAT — no internet route to targets |

> Both VMs must be on the **same network segment** so that Nessus can reach Metasploitable2.

---

## Job 02 — Metasploitable2 VM Setup

### Download
Metasploitable2 is freely available from Rapid7:
- [https://docs.rapid7.com/metasploit/metasploitable-2/](https://docs.rapid7.com/metasploit/metasploitable-2/)

Download the `.zip`, extract the `.vmdk` image, and import it into your hypervisor (VMware / VirtualBox).

### Network Configuration
Set the VM's network adapter to **Host-Only** (or the same NAT network as your Nessus VM).

### First Boot
At login prompt:
```
Login: msfadmin
Password: msfadmin
```

Check the VM's IP address:
```bash
ifconfig
```

Note down this IP — it will be the **scan target** for Nessus.

---

## Job 03 — Debian VM + Nessus Installation

### Step 1 — Create a Debian VM

- Download Debian ISO: [https://www.debian.org/](https://www.debian.org/)
- Minimum recommended specs: 2 vCPU, 4 GB RAM, 20 GB disk
- Same network segment as Metasploitable2

### Step 2 — Create a Nessus Account

Go to [https://www.tenable.com/products/nessus](https://www.tenable.com/products/nessus) and register for a **free 7-day trial** (or Nessus Essentials for up to 16 IPs).  
You will receive an **activation code** by email.

### Step 3 — Download the Nessus Package

From [https://www.tenable.com/downloads/nessus](https://www.tenable.com/downloads/nessus):  
Select version and platform → **Linux — Debian — amd64** → Download the `.deb` file.

Or via `curl` (check the downloads page for the exact URL):
```bash
curl -L -o Nessus-<version>-debian10_amd64.deb \
  "https://www.tenable.com/downloads/api/v1/public/pages/nessus/downloads/<id>/download?i_agree_to_tenable_license_agreement=true"
```

### Step 4 — Install Nessus

```bash
# Install the package
sudo dpkg -i Nessus-<version>-debian10_amd64.deb

# Start the Nessus service
sudo systemctl start nessusd.service

# Enable Nessus to start on boot (optional)
sudo systemctl enable nessusd.service
```

### Step 5 — Initial Web UI Setup

Open a browser on the Debian VM (or from the host with port forwarding):
```
https://<debian-vm-ip>:8834
```

> Accept the self-signed certificate warning — this is expected.

Follow the setup wizard:
1. Choose **Nessus Professional / Trial** (or Essentials)
2. Enter your **activation code**
3. Create an admin account (username + password)
4. Wait for plugin compilation (~10–15 minutes on first launch)

---

## Network Verification

After both VMs are running, verify connectivity from the Nessus VM to the target:

```bash
ping <metasploitable2-ip>
nmap -sn <metasploitable2-ip>
```

Both should respond. If not, check the VM network adapter settings.

---

## Summary — IP Reference Table

| VM | Role | IP Address |
|---|---|---|
| Debian (Nessus) | Scanner | `<fill in your IP>` |
| Metasploitable2 | Target | `<fill in your IP>` |

> Replace the placeholders above with your actual lab IPs once the VMs are running.

---

*Previous: [01 — Overview](./01-nessus-overview.md) | Next: [03 — Scan & Findings](./03-scan-and-findings.md)*
