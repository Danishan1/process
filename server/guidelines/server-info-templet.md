# **itsRIGHTtime Server Information Template**

# Server Overview

**Server Name / Hostname:**  
**IP Address / Domain:**  
**Environment:** (Production / Staging / Development)  
**OS & Version:** (e.g., Ubuntu 22.04 LTS)  
**Date Deployed:**  
**Owner / Responsible Employee(s):**  



## 1. Hardware & Resources

- CPU:  
- RAM:  
- Disk Storage:  
- Disk Usage:  
- Network Bandwidth / Limits:  


## 2. Access & Accounts

**SSH Access:**  
- Admin user(s):  
- DEV users:  
- WorkSpace users:  
- Notes on key-based authentication / restrictions:  

**Sudo Access:** (List users)  

**Password Policies / Key Rotation:**  


## 3. Installed Services & Packages

| Service / Software | Version | Purpose | Notes |
|------------------|---------|--------|-------|
| Nginx / Apache   |         | Web server |       |
| MySQL / PostgreSQL |       | Database |       |
| Docker           |         | Containerization |       
| Node.js          |         | Runtime for DEV |       
| Python / Django  |         | Backend |       |
| Any others       |         |        |       |


## 4. Folder Structure

```
/opt/irt-dev/
├─ projects/
│  ├─ web-development/
│  ├─ mobile-application-development/
│  ├─ backend-infrastructure/
│  ├─ frontend-engineering/
│  ├─ mvp-startup-acceleration/
│  ├─ maintenance-support/
│  └─ emerging-technology/
├─ scripts/          # Deployment, CI/CD, automation scripts for DEV
└─ docs/             # DEV SOPs, guidelines, templates

/opt/irt-ws/products/
├─ letsSecure/
├─ letsIAM/
├─ letsDiscuss/
├─ letsNotify/
├─ letsNoteThis/
├─ letsWrite/
├─ letsSchedule/
└─ upcoming_products/
   ├─ letsCreate/
   ├─ letsCollaborate/
   └─ etc.
Each product has:
│  ├─ code/
│  ├─ config/
│  ├─ logs/
│  └─ docs/

/shared/
├─ backups/          # Daily, weekly backups for DEV and WS
├─ scripts/          # Server-wide automation, monitoring, updates
├─ secrets/          # API keys, environment variables (restricted)
├─ logs/             # Centralized logs for server monitoring
└─ docs/             # Server overview, maintenance, access control, changelog

```


## 5. DEV Projects

| Category | Project Name | Path | Owner | Notes |
|----------|--------------|------|-------|-------|
| Web Development | Project1 | /opt/irt-dev/projects/web-development/corporate-websites/project1 | John | Live / Staging |
| Backend | Project2 | /opt/irt-dev/projects/backend-infrastructure/custom-backend/nodejs/project2 | Jane | Testing |


## 6. WorkSpace Products

| Product | Path | Version / Deployment | Owner | Notes |
|---------|------|--------------------|-------|-------|
| letsSecure | /opt/irt-ws/products/letsSecure | v1.0 | John | Live |
| letsIAM | /opt/irt-ws/products/letsIAM | v1.2 | Jane | Under Maintenance |



## 7. Backups

- Backup Location: `/shared/backups/`  
- Frequency: Daily / Weekly / Monthly  
- Backup Retention Policy:  
- Responsible Person:  



## 8. Scripts & Automation

| Script | Path | Purpose | Owner | Notes |
|--------|------|--------|-------|-------|
| deploy_project.sh | /opt/irt-dev/scripts/deploy_project.sh | Deploy DEV projects | John | Use after Git pull |
| health_check.sh | /shared/scripts/health_check.sh | Server & service health monitoring | Jane | Cron every 30 min |



## 9. Secrets & Environment Variables

- Location: `/shared/secrets/`  
- Access: Restricted (list users)  
- Notes on sensitive keys / DB credentials:  



## 10. Logs & Monitoring

- Central log folder: `/shared/logs/`  
- Monitoring setup: (e.g., Netdata, Prometheus)  
- Log rotation schedule:  



## 11. Maintenance Schedule

| Task | Frequency | Responsible | Notes |
|------|----------|------------|-------|
| OS Updates | Weekly | John | `sudo apt update && sudo apt upgrade -y` |
| Backup Verification | Daily | Jane | Check backup integrity |
| Log Cleanup | Monthly | John | Rotate old logs |
| Security Patches | As needed | Admin | Apply critical updates immediately |


## 12. Notes & Additional Information

- Any special configuration, dependencies, or quirks of the server  
- Contacts for emergency support  
- Important URLs / endpoints / dashboards  

