# Build a WordPress Job Dashboard

**Recommended Expertise:** WordPress development, PHP, REST APIs, JavaScript, performance optimization, and plugin architecture

---

# Steps Overview

1. Create the custom plugin.
2. Connect to one API (e.g., Ashby).
3. Display job listings.
4. Add search and filters.
5. Add company pages and job details.
6. Support multiple ATS providers (Ashby, Workday, Greenhouse, Lever).
7. Add user features like favorites and alerts.
8. Polish it with a professional interface.

---

# Purpose

Demonstrate to WordPress / Automattic my aptitude for building in WordPress; Offer new WP job search experience to moorcoders.

---

# I. CREATE PLUGIN

## Here's what you need:

- WordPress (open source) — Free to download and develop with.
- PHP — The primary language for WordPress plugins. Free.
- A code editor — Free options include Visual Studio Code, Notepad++, or VSCodium.
- A local development environment — Free options include Local, XAMPP, Laragon (Windows), or MAMP's free version.
- A web browser for testing.

## System Requirements

A modern local development environment typically includes:

- PHP 8.x
- MySQL or MariaDB
- Apache or Nginx
- WordPress 6.x or later

## Basic workflow:

1. Install WordPress on your computer using a local development environment.
2. Create a new folder in the `wp-content/plugins` directory.
3. Add a PHP file with a plugin header.

Example:

```php
<?php
/*
 * Plugin Name: My First Plugin
 * Description: My first WordPress plugin.
 * Version: 1.0
 * Author: Your Name
 */

defined('ABSPATH') || exit;

// Your code goes here.
