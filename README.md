# 🗡️ killersql — Tueur de requêtes SQL longues

![Bash](https://img.shields.io/badge/Bash-5%2B-informational?style=for-the-badge)
![MySQL](https://img.shields.io/badge/MySQL-MariaDB-4479A1?style=for-the-badge)
![Linux](https://img.shields.io/badge/Linux-Debian%2FUbuntu-orange?style=for-the-badge)
![Licence](https://img.shields.io/badge/Licence-MIT-green?style=for-the-badge)

Script de surveillance et d'élimination automatique des requêtes SQL trop longues (> 150 secondes). Évite les blocages de base de données causés par des requêtes SELECT bloquantes.

> Voir aussi le repo jumeau [`sqlkiller`](https://github.com/titithen00b/sqlkiller) qui propose une version avec service systemd.

---

## Fonctionnalités

- Surveillance continue du processlist MySQL/MariaDB
- Kill automatique des requêtes SELECT dépassant le seuil configuré
- Journalisation des requêtes tuées (utilisateur, durée, requête)
- Seuil de durée paramétrable

---

## Prérequis

- Bash 5+
- MySQL ou MariaDB
- Utilisateur MySQL avec droits `PROCESS` et `SUPER` (ou `KILL`)

---

## Installation

```bash
git clone https://github.com/titithen00b/killersql.git
cd killersql
chmod +x killersql
```

---

## Configuration

Ouvrir le script `killersql` et renseigner les variables de connexion :

```bash
DB_USER="root"
DB_PASS="mon_mot_de_passe"
DB_HOST="localhost"
MAX_QUERY_TIME=150    # Durée max en secondes avant kill
LOG_FILE="/var/log/killersql.log"
```

---

## Utilisation

### Lancement manuel

```bash
./killersql
```

### Comme service systemd

```bash
sudo cp killersql /usr/local/bin/killersql
sudo cp killersql.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable killersql.service
sudo systemctl start killersql.service
```

### Via cron (alternative)

Vérification toutes les minutes :

```
* * * * * /usr/local/bin/killersql >> /var/log/killersql.log 2>&1
```

---

## Exemple de log

```
[2024-01-15 14:32:01] KILL — ID:1542 | User: app_user | Durée: 187s | Query: SELECT * FROM big_table WHERE...
[2024-01-15 16:10:44] KILL — ID:1687 | User: reporting | Durée: 210s | Query: SELECT COUNT(*) FROM logs...
```

---

## Fichiers du projet

| Fichier | Description |
|---------|-------------|
| `killersql` | Script principal de surveillance et kill |

---

## Licence

MIT © Titithen00b
