# Build a WordPress Job Dashboard

**Updated:** 8:06 PM 7/30/2026

---

## Contents

- [Introduction](https://github.com/moorcode/WordPress-Job-Dashboard/blob/main/README.md#introduction)
  - [Purpose](https://github.com/moorcode/WordPress-Job-Dashboard#purpose)
  - [Process & Requirements](https://github.com/moorcode/WordPress-Job-Dashboard#process--requirements)
  - [Overall Architecture](https://github.com/moorcode/WordPress-Job-Dashboard#overall-architecture)
  - [System Architecture](https://github.com/moorcode/WordPress-Job-Dashboard#sytem-architecture)
  - [Repository Structure](https://github.com/moorcode/WordPress-Job-Dashboard#repository-structure)
- [Set Up the Development Environment](https://github.com/moorcode/WordPress-Job-Dashboard#environment)
- [Create the custom plugin](https://github.com/moorcode/WordPress-Job-Dashboard#i-create-plugin)


---

## Introduction

This manual lists the steps to build a job platform, with WordPress serving as the CMS, authentication layer, and content management system. Put another way, these steps build a WordPress-native job aggregation platform with a plugin architecture that can consume multiple ATS APIs, cache results locally, and expose them through WordPress pages, REST endpoints, Gutenberg blocks, and shortcodes. The steps recorded here were executed using an established environment, thus some essential steps could be missing for you, if you are establishing your dev environment for the first time.


### Purpose
To demonstrate to WordPress / Automattic that I'm a WordPress builder; To offer a native WordPress job search experience to  [moorcoders](https://moorcode.wordpress.com/).

### Process & Requirements

- Environment Setup (one-time)
	- [WSL2 Ubuntu](https://ubuntu.com/wsl), Git, Apache, MySQL, PHP, Composer, WP-CLI, Node/npm, VS Code 
- WordPress & Plugin Setup (one-time)
- Feature Development (iterative)

### Overall Architecture

```text

Development Environment (WSL2 Ubuntu)
│
├── Git
├── Apache
├── MySQL
├── PHP 8.x
├── Composer
├── WP-CLI
├── Node.js
│   └── npm
├── VS Code + WSL Extension
└── Browser
        │
        ▼
Local WordPress Project
│
├── WordPress Core
├── wp-config.php
├── wp-content/
│   │
│   ├── plugins/
│   │   │
│   │   └── job-api-manager/
│   │       │
│   │       ├── Bootstrap
│   │       │   ├── Plugin Loader
│   │       │   ├── Activation
│   │       │   ├── Deactivation
│   │       │   └── Uninstall
│   │       │
│   │       ├── Settings
│   │       │   ├── API Keys
│   │       │   ├── Default Companies
│   │       │   ├── Default Roles
│   │       │   ├── Sync Settings
│   │       │   └── Cache Settings
│   │       │
│   │       ├── Database
│   │       │   ├── Schema
│   │       │   ├── Migrations
│   │       │   ├── Companies
│   │       │   ├── Roles
│   │       │   ├── Cached Jobs
│   │       │   ├── Favorites
│   │       │   └── Alerts
│   │       │
│   │       ├── API
│   │       │   ├── Provider Interface
│   │       │   ├── HTTP Client
│   │       │   ├── Ashby
│   │       │   ├── Greenhouse
│   │       │   ├── Lever
│   │       │   ├── Workday
│   │       │   └── Response Normalizer
│   │       │
│   │       ├── Services
│   │       │   ├── Job Sync
│   │       │   ├── Search
│   │       │   ├── Favorites
│   │       │   ├── Alerts
│   │       │   ├── Company Service
│   │       │   └── Notification Service
│   │       │
│   │       ├── REST
│   │       │   ├── Jobs Endpoint
│   │       │   ├── Companies Endpoint
│   │       │   ├── Favorites Endpoint
│   │       │   └── Alerts Endpoint
│   │       │
│   │       ├── Admin
│   │       │   ├── Dashboard
│   │       │   ├── Manage Companies
│   │       │   ├── Manage Roles
│   │       │   ├── Manual Sync
│   │       │   ├── Job Editor
│   │       │   └── Settings
│   │       │
│   │       ├── Frontend
│   │       │   ├── Shortcodes
│   │       │   ├── Gutenberg Blocks
│   │       │   ├── Templates
│   │       │   ├── Search 
│   │       │   ├── Filters
│   │       │   ├── Company Pages
│   │       │   ├── Job Pages
│   │       │   └── Favorites
│   │       │
│   │       └── Assets
│   │           ├── CSS
│   │           ├── JavaScript
│   │           └── Images
│   │
│   ├── themes/
│   └── uploads/
│
├── Local Database
└── REST API
        │
        ▼
External ATS Providers
│
├── Ashby
├── Greenhouse
├── Lever
├── Workday
└── Future Providers
```

### System Architecture

```text
Browser
    │
    ▼
WordPress
    │
    ▼
Job API Manager Plugin
│
├── Admin
│
├── REST API
│
├── Database Layer
│
├── ATS Provider Interface
│   │
│   ├── Ashby
│   ├── Greenhouse
│   ├── Lever
│   └── Workday
│
├── Cache Layer
│
├── Search
│
├── Favorites
│
├── Alerts
│
└── Frontend
    │
    ▼
WordPress Pages
```

### Repository Structure

```text
wordpress-job-dashboard/
│
├── README.md
├── LICENSE
├── .gitignore
├── composer.json
├── composer.lock
├── package.json
├── package-lock.json
├── wp-config-sample.php
│
├── wordpress/
│   └── (WordPress Core)
│
├── wp-content/
│   ├── plugins/
│   │   └── job-api-manager/
│   │
│   ├── themes/
│   └── uploads/
│
├── docs/
│   ├── architecture.md
│   ├── api-integrations.md
│   ├── database-schema.md
│   ├── deployment.md
│   └── roadmap.md
│
├── scripts/
│   ├── setup.sh
│   ├── install-wordpress.sh
│   └── reset-db.sh
│
│
└── .github/
    └── workflows/
	├── php-tests.yml
	├── coding-standards.yml
        └── build.yml
```

---

## Steps

### 0. Set Up Environment
__Verify / Update Ubuntu & Linux Kernel, Install Basic Development Tools, Install Apache Web Server, MySQL, PHP, Secure Database, Create WordPress Database__


```bash
lsb_release -a
```
(expected: Ubuntu 22.04 LTS or Ubuntu 22.04 LTS)



```bash
uname -a
```
(expected: microsoft-standard-WSL2)



```bash
sudo apt update
```

```bash
sudo apt upgrade -y
```

```bash
sudo apt install -y curl wget unzip git software-properties-common
```
(installs several useful command-line tools on a Debian/Ubuntu-based Linux system)

```bash
git --version
```
(git version 2.x.x)

```bash
sudo apt install apache2 -y
```

```bash
sudo service apache2 start
```

```bash
sudo service apache2 status
```
 (expected: active (running))

**WSL2 note
Apache will not automatically start after reboot unless configured. For development, manually starting it is fine.
Test: When http://localhost is put inside Windows browser you should see "Apache2 Ubuntu Default Page"

10. sudo apt install mysql-server -y (install)
11. sudo service mysql start (start)
12. sudo service mysql status (check)
13. sudo mysql_secure_installation (secure)


## I. Create Plugin
