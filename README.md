# Nivi Atlas

Nivi Atlas is a Linux hosting control panel. Use it to manage websites, WordPress, mail, DNS, databases, SSL and backups from one panel. This repository holds the signed release bundles and the installer.

## Requirements

- A fresh **AlmaLinux 9** or **Rocky Linux 9** server, x86_64
- Root access over SSH
- At least 1 GB RAM (2 GB recommended) and 5 GB free disk
- Ports 7443 (panel), 80 and 443 (websites) reachable

## Install

Run this as root on the server:

```bash
curl -sSL https://github.com/GXHOST/nivi-atlas-releases/releases/latest/download/install.sh | bash
```

The installer checks the server, downloads the latest release and verifies its SHA-256 checksum. It then installs Nginx, PostgreSQL, PHP 8.3 and the panel, and opens the needed ports in firewalld. When it finishes, it prints the panel address and the admin password. It takes a few minutes.

Running it again is safe: keys, certificates and the admin password are kept.

### Options

Pass options after `bash -s --`:

```bash
curl -sSL https://github.com/GXHOST/nivi-atlas-releases/releases/latest/download/install.sh | bash -s -- --version v0.2.31 --yes
```

| Option | Meaning |
|---|---|
| `--version TAG` | Install a specific release instead of the latest |
| `--port N` | Panel HTTPS port (default 7443) |
| `--admin USER` | Admin username (default `admin`) |
| `--reset-admin` | Generate a new admin password |
| `--dry-run` | Only check the server; change nothing |
| `-y`, `--yes` | Do not ask for confirmation |

## First login

1. Open `https://<server-ip>:7443`. The panel starts with a self-signed certificate, so your browser shows a warning the first time.
2. Sign in as `admin` with the password the installer printed. It is also saved, readable by root only, in `/root/.nivi-atlas-admin`.
3. Under **Software**, add what you need: mail, DNS, databases, more PHP versions and so on.
4. Point your domains at the server's IP, then add sites and issue free SSL certificates from the panel.

Lost the admin password? Run the install command again with `--reset-admin`.

## Updates

Under **Tools & Settings → Updates**, the panel updates itself from the signed releases here, either manually or automatically at a chosen hour. If a new version fails to start, the update is rolled back automatically.

## Troubleshooting

```bash
systemctl status atlas-api atlas-executor nginx
journalctl -u atlas-api -n 50
```

## Releases

See [Releases](https://github.com/GXHOST/nivi-atlas-releases/releases) for every version and its changes. Each bundle comes with a SHA-256 checksum and a signature that the panel checks before updating.
