# Build a WordPress Job Dashboard

**Updated:** 7:45 PM 7/16/2026

---

## Contents

- [Introduction](https://github.com/moorcode/WordPress-Job-Dashboard/blob/main/README.md#introduction)
  - [Process & Requirements](https://github.com/moorcode/WordPress-Job-Dashboard#process--requirements)
  - [Architecture](https://github.com/moorcode/WordPress-Job-Dashboard#architecture)
  - [Repository Structure](https://github.com/moorcode/WordPress-Job-Dashboard#repository-structure)
- [Create the custom plugin](https://github.com/moorcode/WordPress-Job-Dashboard#i-create-plugin)
- Build the database and API framework
- Connect to one API (e.g., Ashby)
- Display job listings
- Add search and filters
- Add company pages and job details
- Support multiple ATS providers (Ashby, Workday, Greenhouse, Lever)
- Add user features like favorites and alerts
- Polish it with a professional interface

---

## Introduction

This manual lists the steps to build a job dashboard in WordPress. The steps recorded here were executed using an established environment, thus some essential steps could be missing for you, if you are establishing your dev environment for the first time. This job dashboard serves to show WordPress / Automattic how I build plugins using WordPress features & to offer a native WordPress job search experience to [moorcoders](https://moorcode.wordpress.com/).

---

### Process & Requirements

- Environment Setup (one-time)
- WordPress & Plugin Setup (one-time)
- Feature Development (iterative)

---

### Architecture

```text
Development Environement
│
├── Git
├── PHP
├── Composer
├── MySQL / MariaDB
├── Apache or Nginx
├── WP-CLI
├── Node.js
│   └── npm
└── VS Code
        │
        ▼
Local WordPress Project
│
├── WordPress Core
│
├── wp-content/
│   │
│   ├── plugins/
│   │   │
│   │   └── job-api-manager/
│   │       │
│   │       ├── Plugin Bootstrap
│   │       │
│   │       ├── Admin Settings
│   │       │   ├── API Keys
│   │       │   ├── Default Companies
│   │       │   └── Default Roles
│   │       │
│   │       ├── Database
│   │       │   ├── Companies
│   │       │   ├── Roles
│   │       │   ├── Cached Jobs
│   │       │   ├── Favorites
│   │       │   └── Alerts
│   │       │
│   │       ├── API Layer
│   │       │   ├── Interface
│   │       │   ├── Ashby
│   │       │   ├── Greenhouse
│   │       │   ├── Lever
│   │       │   └── Workday
│   │       │
│   │       ├── Admin UI
│   │       │   ├── Manage Companies
│   │       │   ├── Manage Roles
│   │       │   ├── Edit Job Details
│   │       │   └── Sync Jobs
│   │       │
│   │       ├── Frontend
│   │       │   ├── Shortcode
│   │       │   ├── Gutenberg Block
│   │       │   ├── Job Listings
│   │       │   ├── Search & Filters
│   │       │   ├── Company Pages
│   │       │   ├── Job Detail Pages
│   │       │   └── Favorites
│   │       │
│   │       └── Assets
│   │           ├── CSS
│   │           └── JavaScript
│   │
│   ├── themes/
│   └── uploads/
│
├── composer.json
├── package.json
└── .gitignore
        │
        ▼
Development Workflow
│
├── WP-CLI
│   ├── Download WordPress
│   ├── Configure WordPress
│   ├── Install WordPress
│   ├── Activate Plugin
│   └── Run Cron
│
├── Composer
│   ├── Install Dependencies
│   └── Autoload Classes
│
├── npm
│   ├── Build CSS/JS
│   └── Watch Assets
│
├── Git
│   ├── Feature Branches
│   ├── Commits
│   └── Releases
│
└── Browser Testing
        │
        ▼
Production WordPress Site
```

# Repository Structure

```text
wordpress-job-dashboard/
│
├── README.md
├── .gitignore
├── composer.json
├── package.json
├── wp-config-sample.php
│
├── wordpress/
│   └── (WordPress core - usually ignored in Git)
│
├── wp-content/
│   │
│   ├── plugins/
│   │   │
│   │   └── job-api-manager/
│   │       │
│   │       ├── job-api-manager.php
│   │       ├── uninstall.php
│   │       ├── includes/
│   │       │   ├── class-plugin-bootstrap.php
│   │       │   ├── class-database.php
│   │       │   ├── class-rest-api.php
│   │       │   └── class-settings.php
│   │       │
│   │       ├── admin/
│   │       │   ├── class-admin-ui.php
│   │       │   └── settings-pages.php
│   │       │
│   │       ├── api/
│   │       │   ├── interface-ats-provider.php
│   │       │   ├── class-ashby.php
│   │       │   ├── class-greenhouse.php
│   │       │   ├── class-lever.php
│   │       │   └── class-workday.php
│   │       │
│   │       ├── database/
│   │       │   ├── migrations.php
│   │       │   └── schema.php
│   │       │
│   │       ├── frontend/
│   │       │   ├── shortcodes.php
│   │       │   ├── blocks/
│   │       │   └── templates/
│   │       │
│   │       ├── assets/
│   │       │   ├── css/
│   │       │   └── js/
│   │       │
│   │       └── tests/
│   │
│   ├── themes/
│   │
│   └── uploads/
│
├── docs/
│   ├── architecture.md
│   ├── api-integrations.md
│   └── database-schema.md
│
└── .github/
    └── workflows/
        └── tests.yml
```

## I. Create Plugin
