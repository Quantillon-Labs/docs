# Récapitulatif Architecture Serveur Quantillon

## Système d'exploitation

| Paramètre | Valeur |
|-----------|--------|
| OS | Ubuntu 25.04 (Plucky) |
| Kernel | Linux 6.14.0-37-generic |
| Architecture | x86_64 |
| RAM | 62 GB |
| Disque | 467 GB (30 GB utilisés) |

---

## Serveur Web - Nginx 1.26.3

**Configuration principale**: `/etc/nginx/nginx.conf`
- Worker: `www-data`
- Protocoles SSL: TLSv1.2, TLSv1.3
- Compression Gzip: Activée
- Certificats: Let's Encrypt (renouvellement auto toutes les 12h)

### Environnements et Domaines

| Domaine | Type | Backend/Root | Port |
|---------|------|--------------|------|
| `quantillon.money` | SPA statique | `/var/www/prod/quantillon-landing/dist` | 443 |
| `app.quantillon.money` | SPA statique | `/var/www/prod/quantillon-dapp/dist` | 443 |
| `api.quantillon.money` | Reverse proxy | `localhost:4000` | 443 |
| `dev.quantillon.money` | SPA + Basic Auth | `/var/www/dev/quantillon-landing/dist` | 443 |
| `dev.app.quantillon.money` | SPA + IP restriction | `/var/www/dev/quantillon-dapp/dist` | 443 |
| `dev.api.quantillon.money` | Reverse proxy | `localhost:4001` | 443 |
| `privatedemo.quantillon.money` | SPA + Basic Auth + RPC | `/var/www/privatedemo/quantillon-dapp/dist` | 443 |
| `stats.quantillon.money` | Reverse proxy (Umami) | `localhost:3000` | 443 |
| `docs.quantillon.money` | Redirect | `quantillon.gitbook.io` | 443 |

### Authentification
- **Basic Auth**: Fichier `/etc/nginx/.htpasswd` (user: `quantillon`)
- **IPs autorisées**: 176.135.25.75, 176.129.130.230

---
## Firewall - UFW (Uncomplicated Firewall)

**Status**: Actif, politiques par défaut:
- INPUT: **DROP** (deny)
- OUTPUT: **ACCEPT**
- FORWARD: **DROP**

### Règles ouvertes

| Port | Protocole | Service |
|------|-----------|---------|
| 80, 443 | TCP | Nginx (HTTP/HTTPS) |
| 54321 | TCP | SSH (rate-limited) |
| 4000 | TCP/UDP | API prod |
| 4001 | TCP/UDP | API dev |
| 4101 | TCP/UDP | Funding monitor (prod) |
| 4102 | TCP/UDP | Indexer service (prod) |
| 5101 | TCP/UDP | Funding monitor (dev) |
| 5102 | TCP/UDP | Indexer service (dev) |
| 6101 | TCP/UDP | Funding monitor (privatedemo) |
| 6102 | TCP/UDP | Indexer service (privatedemo) |

**Note**: Fail2ban n'est **pas installé**.

---

## Base de données - PostgreSQL 17.7

**Configuration**: `/etc/postgresql/17/main/`
- **Port**: 5432 (localhost uniquement)
- **SSL**: Activé
- **Max connections**: 100

### Bases de données

| Base | Propriétaire | Taille | Tables |
|------|--------------|--------|--------|
| `umami` | umamiuser | 22 MB | 12 (analytics) |
| `quantillon_profiles` | quantillondapp | 8.4 MB | 3 (user_profiles, funding_metadata, funding_points) |

### Utilisateurs
- `postgres` (superuser)
- `umamiuser` (umami analytics)
- `quantillondapp` (application Quantillon)

---

## Langages et Frameworks

### Frontend (dApp & Landing)

| Technologie | Version |
|-------------|---------|
| **TypeScript** | 5.2.2 |
| **React** | 18.2.0 |
| **Vite** | 5.1.4 |
| **Chakra UI** | 2.10.9 |
| **Wagmi** | Latest (Web3) |
| **RainbowKit** | Latest (wallets) |

### Backend API

| Technologie | Version |
|-------------|---------|
| **Node.js** | 20.18.1 |
| **Express.js** | 5.1.0 |
| **PostgreSQL (pg)** | 8.16.3 |
| **SIWE** | Latest (auth Web3) |

### Smart Contracts

| Technologie | Version |
|-------------|---------|
| **Solidity** | 0.8.24 |
| **Foundry** | Latest |
| **EVM** | Paris |

### Analytics (Umami)

| Technologie | Version |
|-------------|---------|
| **Next.js** | 15.3.3 |
| **Prisma** | ORM |

---

## Process Manager - PM2 6.0.8

| App | Port | Env | Chemin |
|-----|------|-----|--------|
| `umami` | 3000 | - | /var/www/umami |
| `quantillon-api` | 4000 | prod | /var/www/prod/quantillon-dapp/backend |
| `quantillon-api-dev` | 4001 | dev | /var/www/dev/quantillon-dapp/backend |
| `funding-monitor` | 4101 | prod | /var/www/prod/quantillon-dapp/backend/funding-monitor |
| `funding-monitor-dev` | 5101 | dev | /var/www/dev/quantillon-dapp/backend/funding-monitor |
| `funding-monitor-privatedemo` | 6101 | privatedemo | /var/www/privatedemo/quantillon-dapp/backend/funding-monitor |
| `indexer-api` | 4102 | prod | /var/www/prod/quantillon-dapp/backend/indexer-service |
| `indexer-api-dev` | 5102 | dev | /var/www/dev/quantillon-dapp/backend/indexer-service |
| `indexer-api-privatedemo` | 6102 | privatedemo | /var/www/privatedemo/quantillon-dapp/backend/indexer-service |

### Port Allocation Strategy

| Service | Prod | Dev | Privatedemo |
|---------|------|-----|-------------|
| Main API | 4000 | 4001 | 4001 |
| Funding Monitor | 4101 | 5101 | 6101 |
| Indexer Service | 4102 | 5102 | 6102 |

---

## Blockchain - Anvil

**Service**: `anvil.service`
- **Port**: 8545
- **Type**: Fork Base Mainnet
- **Chain ID**: 31337
- **Comptes test**: 10 (10000 ETH chacun)
- **Accès**: Proxié via `privatedemo.quantillon.money/rpc`

---
## Structure des répertoires

```
/var/www/
├── prod/
│   ├── quantillon-landing/dist/    # Landing page prod
│   └── quantillon-dapp/dist/       # dApp prod
├── dev/
│   ├── quantillon-landing/dist/    # Landing page dev
│   ├── quantillon-dapp/
│   │   ├── dist/                   # dApp dev frontend
│   │   └── backend/                # API dev (index.js)
│   └── smart-contracts/
│       └── quantillon-protocol/    # Contrats Solidity
├── privatedemo/
│   └── quantillon-dapp/
│       ├── dist/                   # dApp privatedemo
│       └── backend/
│           └── funding-monitor/    # Service monitoring
└── umami/                          # Analytics platform
```

---

## Certificats SSL (Let's Encrypt)

| Domaine | Expiration |
|---------|------------|
| quantillon.money | 10 Avr 2026 |
| api.quantillon.money | 10 Avr 2026 |
| app.quantillon.money | 10 Avr 2026 |
| dev.quantillon.money | 10 Avr 2026 |
| **dev.api.quantillon.money** | **20 Mar 2026** (attention) |
| dev.app.quantillon.money | 10 Avr 2026 |
| privatedemo.quantillon.money | 12 Avr 2026 |
| stats.quantillon.money | 10 Avr 2026 |

---

## Tâches planifiées (Cron)

- **Certbot**: Toutes les 12h - renouvellement SSL
- **Sysstat**: Toutes les 10 min - collecte métriques
- **ZFS TRIM**: 1er dimanche du mois
- **ZFS SCRUB**: 2ème dimanche du mois

---

## Résumé Architecture

```
                              ┌─────────────────┐
                              │   Internet      │
                              └────────┬────────┘
                                       │
                              ┌────────▼────────┐
                              │  UFW Firewall   │
                              │  (80,443,54321) │
                              └────────┬────────┘
                                       │
                              ┌────────▼────────┐
                              │  Nginx 1.26.3   │
                              │  (SSL/Proxy)    │
                              └────────┬────────┘
          ┌────────────────────────────┼────────────────────────────┐
          │                            │                            │
 ┌────────▼────────┐          ┌────────▼────────┐          ┌───────▼────────┐
 │  Static Files   │          │   Backend API   │          │    Umami       │
 │  (React/Vite)   │          │  (Express.js)   │          │  (Next:3000)   │
 └─────────────────┘          └────────┬────────┘          └───────┬────────┘
                                       │                            │
                    ┌──────────────────┼──────────────────┐         │
                    │                  │                  │         │
           ┌────────▼────────┐┌────────▼────────┐┌───────▼────────┐│
           │   Main API      ││ Funding Monitor ││ Indexer Service ││
           │ (4000/4001)     ││ (4101/5101/6101)││ (4102/5102/6102)││
           └────────┬────────┘└────────┬────────┘└────────┬───────┘│
                    │                  │                  │        │
                    └──────────────────┼──────────────────┘        │
                                       │                           │
                              ┌────────▼───────────────────────────▼────────┐
                              │              PostgreSQL 17.7                │
                              │       (quantillon_profiles, umami)          │
                              └────────┬────────────────────────────────────┘
                                       │
                              ┌────────▼────────┐
                              │  Anvil (8545)   │
                              │  Base Fork      │
                              └─────────────────┘
```

---

## Points d'attention

1. **Certificat dev.api.quantillon.money** expire le 20 Mars 2026 (le plus tôt)
2. **Fail2ban** non installé - protection brute-force limitée au rate-limiting UFW
3. **SSH** sur port non-standard 54321 (bonne pratique)
4. **Tous les services Node.js** tournent en root (considération sécurité)
5. **PostgreSQL** accessible uniquement en localhost (sécurisé)

---

