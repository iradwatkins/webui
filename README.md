# Open WebUI Backup

This repository contains automated backups of Open WebUI configuration and data.

## What's Backed Up

- **Database**: WebUI SQLite database with users, chats, prompts, documents
- **Uploads**: User-uploaded files
- **Vector DB**: Embeddings and vector database
- **Configuration**: Docker compose snippet and settings
- **Metadata**: Backup timestamps and statistics

## What's NOT Backed Up

- Large model cache files (to save space)
- LLM models themselves (stored in Ollama separately)

## Restoration Instructions

### Quick Restore

```bash
# 1. Clone this repository
cd /root
git clone git@github.com:iradwatkins/webui.git

# 2. Stop WebUI container
cd /root/docker-containers
docker compose -f services-compose.yml stop webui

# 3. Restore database
cp /root/webui/database/webui.db /var/lib/docker/volumes/open-webui/_data/webui.db

# 4. Restore uploads
rsync -av /root/webui/uploads/ /var/lib/docker/volumes/open-webui/_data/uploads/

# 5. Restore vector database
rsync -av /root/webui/config/vector_db/ /var/lib/docker/volumes/open-webui/_data/vector_db/

# 6. Restart WebUI
docker compose -f services-compose.yml up webui -d
```

### Full Restore from Archive

```bash
# Extract a specific archive
cd /root/webui/archive
tar -xzf webui-backup-TIMESTAMP.tar.gz -C /var/lib/docker/volumes/open-webui/_data/
```

## Automated Backups

Backups run automatically every 6 hours via systemd timer.

Manual backup:
```bash
/root/scripts/backup-webui.sh
```

## Important Notes

⚠️ This repository is PRIVATE - contains user data and configurations
📦 Archives are kept for last 5 backups locally
🔄 Git history maintains all configuration changes
🔗 Ollama models must be backed up separately (see n8n-back-ups repo)
