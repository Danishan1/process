
# **itsRIGHTtime VPS Structure & Management Guide**

## **1. Purpose**

This document defines the standardized structure and management practices for the Ubuntu VPS hosting the itsRIGHTtime Tech Ecosystem. The goal is to:

* Enable collaboration without creating bottlenecks
* Keep server organization flat, modular, and scalable
* Ensure consistent documentation, backups, and monitoring
* Support both DEV projects and WorkSpace products efficiently

---

## **2. High-Level Principles**

1. **Division-Based Organization:** Separate DEV projects and WorkSpace products.
2. **Modular Design:** Each project/product has its own code, config, logs, and documentation.
3. **Shared Resources:** Centralized folder for backups, scripts, secrets, and server-wide documentation.
4. **Security & Access Control:** Sensitive configs, secrets, and credentials stored in restricted locations.
5. **Documentation First:** Every folder contains a `docs/` subfolder explaining deployment, troubleshooting, and best practices.

---

## **3. VPS Directory Structure**

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

---

## **4. Folder Contents**

### **4.1 Projects**

* `code/` → Source code of the project/product
* `config/` → Environment and application configurations (sensitive info in `/shared/secrets/`)
* `logs/` → Runtime logs
* `docs/` → Deployment instructions, troubleshooting, SOPs

### **4.2 Scripts**

* `/opt/irt-dev/scripts` → DEV-specific automation (deployment, CI/CD, health checks)
* `/opt/irt-ws/scripts` → Product-specific deployment scripts
* `/shared/scripts` → Server-wide scripts (monitoring, updates, backups)

### **4.3 Documentation**

* `/opt/irt-dev/docs` → DEV guidelines, project templates, best practices
* `/opt/irt-ws/products/<product>/docs` → Product deployment and maintenance guides
* `/shared/docs` → Server overview, maintenance schedule, access control, changelog

### **4.4 Backups**

* `/shared/backups` → Projects, products, database dumps
* Maintain daily, weekly, and monthly backups

### **4.5 Secrets**

* `/shared/secrets` → Restricted access only
* Store credentials, API keys, environment variables

### **4.6 Logs**

* `/shared/logs` → Aggregated server logs for auditing and monitoring

---

## **5. Access & Security**

* Individual accounts per employee
* SSH key-based access only (no passwords)
* Role-based permissions for DEV vs WorkSpace folders
* Restrict access to `/shared/secrets` (`chmod 600`)

---

## **6. Collaboration Guidelines**

1. **Role-Based Ownership:** DEV projects managed by DEV team, WS products by WorkSpace team.
2. **Version Control:** All code, configs, and scripts should be Git-tracked.
3. **Templates:** Each project/product should start with a template for `docs/`, `config/`, and `logs/`.
4. **Change Logging:** Use `/shared/docs/changelog.md` for server-wide changes.
5. **Reviews:** Major script or config changes require peer review.

---

## **7. Maintenance & Monitoring**

* Keep `/shared/scripts/` for automated health checks and monitoring
* Regular updates: `sudo apt update && sudo apt upgrade -y`
* Backup schedule: daily for projects, weekly for full server snapshot
* Monitor logs in `/shared/logs/`

---

## **8. Key Benefits of This Structure**

* Flat and modular for easy navigation
* Clear separation between DEV and WorkSpace products
* Scalable for future projects/products
* Centralized backups, logs, and secrets
* Minimal dependency on a single person
* Faster onboarding for new employees
