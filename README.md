<p align="center">
  <img src="public/app/images/logo.png" height="128px">
</p>

<h1 align="center">Anchr - DevOps Internship Project</h1>
<h3 align="center">A production-ready deployment of Anchr with full monitoring, reverse proxy, and containerized infrastructure. Anchr is a powerful toolbox for common internet tasks including <strong>bookmarks</strong>, <strong>link shortening</strong>, and <strong>image uploads</strong>.</h3>

<p align="center">
    <img src="https://badges.fw-web.space/github/license/muety/anchr?style=flat-square">
    <img src="https://badges.fw-web.space/github/package-json/v/muety/anchr?style=flat-square">
    <img src="https://badges.fw-web.space/github/languages/code-size/muety/anchr">
    <a href="https://liberapay.com/muety/" target="_blank"><img src="http://badges.fw-web.space/liberapay/receives/muety.svg?logo=liberapay&style=flat-square"></a>
</p>

## 🎯 Project Overview

This repository contains a **production-ready DevOps implementation** of the Anchr application, featuring:

- **🐳 Complete Docker Compose orchestration** for all services
- **🔄 Nginx reverse proxy** for unified access and load distribution
- **📊 Prometheus monitoring** for metrics collection and alerting
- **📈 Grafana dashboards** for real-time visualization
- **🗄️ MongoDB database** with persistent storage
- **🔐 Secure configuration** with environment-based secrets management

## ⚡ Quick Start Guide

Get the entire stack running in minutes:

### Prerequisites
- Docker and Docker Compose installed
- At least 4GB of free RAM
- Ports 80, 443, 3000, and 9090 available

### Steps
1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd Part_DevOps_Internship_Project
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env file with your preferred text editor
   nano .env  # or vim, code, etc.
   ```
   
   **Required variables to configure:**
   - `ANCHR_DB_PASSWORD` - Set a strong MongoDB password
   - `ANCHR_SECRET` - Generate a strong random string for JWT
   - `ANCHR_PUBLIC_URL` - Set to your domain or `http://localhost`

3. **Start all services**
   ```bash
   source env.sh
   docker-compose up -d
   ```

4. **Verify deployment**
   ```bash
   # Check all containers are running
   docker-compose ps
   
   # View logs
   docker-compose logs -f
   ```

5. **Access the services**
   - **Anchr Application**: http://localhost/
   - **Prometheus Metrics**: http://localhost/prometheus
   - **Grafana Dashboards**: http://localhost/grafana (default: admin/admin)

### First Time Setup
1. Create your Anchr account through the web interface
2. Log into Grafana and add Prometheus as a data source
3. Configure alerts and dashboards as needed

### Stopping the Stack
```bash
# Stop all services
docker-compose down

# Stop and remove volumes (⚠️ deletes all data)
docker-compose down -v
```


## 🚀 Features
* Link shortener
* Searchable bookmarks collections
* Encrypted images uploads, using [CryptoJS](https://www.npmjs.com/package/crypto-js)
* Malicious link checking, using [Safe Browsing API](https://developers.google.com/safe-browsing/)
* Self-hosted and open-source
* Hosted, GDPR-compliant service at [Anchr.io](https://anchr.io)
* Official [Android app](https://github.com/muety/anchr-android)
* Chrome and Firefox [browser extension](webext)
* [Prometheus](https://prometheus.io) metrics
* Integration with [ShareX](https://github.com/ShareX/ShareX)
* OAuth 2 authentication (Google, Facebook, ...)
* Telegram Bot ([@AnchrBot](https://t.me/AnchrBot))

**If you like this project, please [consider sponsoring it](https://muetsch.io/consider-sponsoring-open-source.html)!**

## 🗒 Description
The idea arose when someday I considered it useful to have a collection of web links or bookmarks – like those you have in Chrome or Firefox – accessible from anywhere without needing to synchronize your browser profile. Just like if you’re somewhere on another PC, find a useful article on the internet and want to save it quickly for later at home. This is what Anchr’s __collections__ feature does. It saves links – with an optional description for easier search and separated into categories / collections.

The second feature is to __upload images__. You can easily upload one or more photos from your computer or mobile device and send them to friends or include them into forum posts or the like. Special with Anchr’s image hosting is that users are given the opportunity to client-sided encrypt images with a password. As a result no one without the password will ever see their photos’ content.

The last feature are __shortlinks__ – actually not any different from those you know from goo.gl or bit.ly. They’re useful if you have a very long web link including many query parameters, access tokens, session ids, special characters and the like and want to share them. Often special characters break the linking or your chat application has a maximum length for hyperlinks. Or you just want to keep clarity in your document or emails. In this case it can be very helpful to make the links as short as any possible. Additionally, shortlinks are checked against [Google's Safe Browsing API](https://developers.google.com/safe-browsing/) to prevent your site to reference phishing sites or the like.

Anchr’s focus is on ease and quickness of use – short loading times, flat menu hierarchies, etc. There's also a Chrome extension out there, which you can use to save or shorten links directly from the website.

## 📡 How to run?
### Prerequisites
In order to host Anchr on your own, you need a few things.
* Node.js >= 21.x
* MongoDB >= 6.x
	* Alternative 1: [Mongo Atlas](https://mongodb.com/atlas) (hosted cloud MongoDB)
	* Alternative 2: [FerretDB](https://www.ferretdb.io/) (with Postgres or SQLite)

#### Database Setup
```bash
$ mongosh
$ > use anchr;  // choose 'anchr' as your database
$ > db.createUser({user: 'anchr', pwd: passwordPrompt(), roles: [{ role: 'dbOwner', db: 'anchr' }]});  // create user 'anchr'
$ > exit
```

### Configuration
1. `$ git clone https://github.com/muety/anchr`
2. Copy `.env.example` to `.env` and edit the contents to set environment variables:
    * `PORT`: TCP port to start the server on (default: `3000`)
    * `LISTEN_ADDR`: IPv4 address to make the server listen on (default: `127.0.0.1`)
    * `ANCHR_PUBLIC_URL`: Public base URL of your hosted instance (no trailing slash) (default: `http://localhost:3000`)
    * `ANCHR_DB_USER`: MongoDB user name (default: `anchr`)
    * `ANCHR_DB_PASSWORD`: MongoDB password (**required**)
    * `ANCHR_DB_HOST`: MongoDB host name (default: `localhost`)
    * `ANCHR_DB_PORT`: MongoDB port (default: `27017`)
    * `ANCHR_DB_NAME`: MongoDB database name (default: `anchr`)
    * `ANCHR_UPLOAD_DIR`: Absolute path to a file system directory (must exist!) to persist uploaded images to (default: `/var/data/anchr`)
    * `ANCHR_SECRET`: A (preferably long), random character sequence to be used for the JSON Web Token (default: `shhh`)
    * `ANCHR_LOG_PATH`: Absolute file path for access logs (directory must exist!) (default:  `/var/log/anchr/access.log`)
    * `ANCHR_ERROR_LOG_PATH`: Absolute file path for error logs (directory must exist!) (default: `/var/log/anchr/error.log`)
    * `ANCHR_GOOGLE_API_KEY`: Your API key for Google APIs (required for safe browse checking incoming shortlinks), which you get from the [Developers Console](https://console.developers.google.com/apis/) (default: `''`, leave blank to disable safe browse checking)
    * `ANCHR_FB_CLIENT_ID` and `ANCHR_FB_SECRET`: OAuth credentials for Facebook Login (default: `''`, leave blank to disable Facebook login)
    * `ANCHR_GOOGLE_CLIENT_ID` and `ANCHR_GOOGLE_SECRET`: OAuth credentials for Google Login (default: `''`, leave blank to disable Google login)
    * `ANCHR_ALLOW_SIGNUP`: Whether to allow sign up of new users (default: `true`)
    * `ANCHR_VERIFY_USERS`: Whether require new users to activate their accounts with an e-mail link (requires mailing) (default: `true`)
    * `ANCHR_BASIC_AUTH`: Whether to allow authenticating using [HTTP Basic Auth](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#authentication_schemes) (default: `true`)
    * `ANCHR_EXPOSE_METRICS`: Whether to expose [Prometheus](https://prometheus.io) metrics under the public `/api/metrics` endpoint (default: `false`)
    * `ANCHR_MAIL_SENDER`: Sender address in mails from Anchr.io (default: `Anchr.io <noreply@anchr.io>`)
    * `ANCHR_SMTP_HOST`: SMTP server host for sending mails (leave empty to disable mailing)
    * `ANCHR_SMTP_PORT`: SMTP server port (default: `587`)
    * `ANCHR_SMTP_TLS`: Whether to establish a TLS connection with the SMTP server (not to be confused with STARTTLS) (default: `false`)
    * `ANCHR_SMTP_USER`: SMTP server login username
    * `ANCHR_SMTP_PASS`: SMTP server login password
    * `ANCHR_MAILWHALE_URL`: Public URL of your [MailWhale](https://github.com/muety/mailwhale) instance when using it for mails instead of SMTP (default: `https://mailwhale.dev`)
    * `ANCHR_MAILWHALE_CLIENT_ID`: MailWhale client ID for authentication
    * `ANCHR_MAILWHALE_CLIENT_SECRET`: MailWhale client secret for authentication
    * `ANCHR_TELEGRAM_BOT_TOKEN`: Telegram bot token (from [@BotFather](https://t.me/BotFather)). Leave empty for disabling Telegram integration.
    * `ANCHR_TELEGRAM_URL_SECRET`: Secret to append to Telegram webhook path for security purposes. Can be any random string.

### ⚙️ Run
#### Setup
1. `$ source env.sh`
2. `$ corepack enable`
3. `$ yarn`
4. `$ cd public && ../node_modules/.bin/bower install && cd ..`
   
#### Option 1: Run Natively
##### For development
1. Run backend `$ yarn start`
2. Run frontend `$ yarn start:frontend`
3. Go to http://localhost:9000 and enjoy live reload

##### In production
1. `$ yarn run build` (to build frontend)
2. `$ yarn run production`

#### Option 2: Run with Docker
1. `source env.sh`
1. `docker-compose up`

## 🏗️ DevOps Infrastructure

This project includes a complete DevOps setup with the following architecture:

### 📦 Docker Compose Services

#### 1. **Anchr Application** (`anchr`)
- **Image**: Built from local Dockerfile
- **Port**: Configurable via `.env` (default: 3000)
- **Description**: Main application service running Node.js
- **Dependencies**: MongoDB database
- **Volumes**: Persistent storage for uploads and data
- **Environment**: Configured via `.env` file with database connection strings

#### 2. **MongoDB Database** (`mongo`)
- **Image**: `mongo:6.0`
- **Description**: Document database for storing bookmarks, links, and user data
- **Volumes**: 
  - `anchr_db_data:/data/db` - Persistent database storage
  - `./scripts/mongo-init.sh` - Database initialization script
- **Environment Variables**:
  - `MONGO_INITDB_ROOT_USERNAME` - Admin username
  - `MONGO_INITDB_ROOT_PASSWORD` - Admin password
  - `MONGO_INITDB_DATABASE` - Default database name

#### 3. **Prometheus** (`prometheus`)
- **Image**: `prom/prometheus:latest`
- **Port**: 9090
- **Description**: Time-series database for metrics collection and monitoring
- **Configuration**: `prometheus.yml` for scrape targets and intervals
- **Features**:
  - 15-second scrape interval
  - Self-monitoring enabled
  - Persistent storage for metrics history
  - Web UI accessible at `/prometheus`
- **Volumes**:
  - Configuration file mounted read-only
  - Persistent data storage for metrics

#### 4. **Grafana** (`grafana`)
- **Image**: `grafana/grafana:latest`
- **Port**: 3000
- **Description**: Visualization and analytics platform for metrics
- **Features**:
  - Real-time dashboards
  - Alert management
  - Accessible at `/grafana` sub-path
  - Pre-configured to work behind reverse proxy
- **Configuration**:
  - `GF_SERVER_ROOT_URL=/grafana` - Base URL configuration
  - `GF_SERVER_SERVE_FROM_SUB_PATH=true` - Sub-path serving

#### 5. **Nginx Reverse Proxy** (`nginx`)
- **Image**: `nginx:alpine`
- **Ports**: 80 (HTTP), 443 (HTTPS ready)
- **Description**: Front-facing reverse proxy for all services
- **Features**:
  - Single entry point for all services
  - Load balancing capabilities
  - SSL/TLS termination ready
  - Static file serving optimization
- **Configuration**:
  - Main config: `nginx/nginx.conf`
  - Site configs: `nginx/conf.d/`
  - Routes traffic to appropriate backend services

### 🔗 Service Architecture

```
┌─────────────────────────────────────────┐
│          Nginx (Port 80/443)            │
│         Reverse Proxy Layer             │
└────┬──────────┬──────────┬──────────────┘
     │          │          │
     ▼          ▼          ▼
┌─────────┐ ┌──────────┐ ┌──────────┐
│ Anchr   │ │Prometheus│ │ Grafana  │
│ :3000   │ │  :9090   │ │  :3000   │
└────┬────┘ └──────────┘ └──────────┘
     │
     ▼
┌─────────┐
│ MongoDB │
│ :27017  │
└─────────┘
```

### 📊 Monitoring Stack

The project includes a complete monitoring solution:

1. **Metrics Collection**: Prometheus scrapes metrics from:
   - Anchr application (if `ANCHR_EXPOSE_METRICS=true`)
   - Prometheus itself (self-monitoring)
   - Additional targets can be added to `prometheus.yml`

2. **Visualization**: Grafana provides:
   - Real-time dashboards
   - Historical data analysis
   - Custom alerting rules
   - Multiple data source support

3. **Access Points**:
   - Prometheus UI: `http://localhost/prometheus`
   - Grafana UI: `http://localhost/grafana`
   - Anchr Application: `http://localhost/`

### 💾 Persistent Volumes

The following Docker volumes ensure data persistence:

- `anchr_data` - Application uploads and user data
- `anchr_db_data` - MongoDB database files
- `prometheus_data` - Metrics storage
- `grafana_data` - Dashboard configurations and settings

### 🔧 Nginx Configuration

The Nginx setup provides:

- **Reverse Proxy**: Routes requests to appropriate backend services
- **Path-based Routing**: 
  - `/` → Anchr application
  - `/prometheus/` → Prometheus metrics
  - `/grafana/` → Grafana dashboards
- **Static File Optimization**: Efficient serving of CSS, JS, and images
- **WebSocket Support**: For real-time features
- **Security Headers**: Basic security configuration

### 📈 Monitoring Best Practices

1. **Enable Metrics**: Set `ANCHR_EXPOSE_METRICS=true` in `.env`
2. **Configure Prometheus**: Add application metrics endpoint to `prometheus.yml`:
   ```yaml
   - job_name: 'anchr'
     scrape_interval: 15s
     static_configs:
       - targets: ['anchr:3000']
     metrics_path: '/api/metrics'
   ```
3. **Set Up Grafana**:
   - Access Grafana at `http://localhost/grafana`
   - Default credentials: `admin/admin` (change on first login)
   - Add Prometheus as data source: `http://prometheus:9090`
   - Import or create dashboards for Anchr metrics

### 🔐 Security Considerations

- **Environment Variables**: Never commit `.env` file with secrets
- **Database Credentials**: Use strong passwords for MongoDB
- **JWT Secret**: Generate a strong `ANCHR_SECRET` for token signing
- **Nginx**: Configure SSL/TLS certificates for production use
- **Grafana**: Change default admin password immediately
- **Network Isolation**: Services communicate via Docker internal network

### 🚦 Health Checks & Dependencies

Service startup order is managed through `depends_on`:
```
nginx → anchr, prometheus, grafana
grafana → anchr
prometheus → anchr
anchr → mongo
```

This ensures proper initialization sequence during deployment.

### 🤖 Telegram Bot Setup
1. Create a new bot with [@BotFather](https://t.me/BotFather)
1. Configure `ANCHR_TELEGRAM_BOT_TOKEN` and `ANCHR_TELEGRAM_URL_SECRET` variables
1. Configure the webhook:
```bash
curl https://api.telegram.org/bot<BOT_TOKEN>/setWebhook?url=https://<ANCHR_URL>/api/telegram/updates/<URL_SECRET>
```

## 🧰 Tooling
### [ShareX](https://github.com/ShareX/ShareX) (Windows only)
You can integrate Anchr with [ShareX](https://github.com/ShareX/ShareX) on Windows and make it be used as a custom target for **image uploads** and **shortlinks**.
1. Generate an HTTP basic auth hash Base64 hash of `youremail@example.org:yourpassword`
    * **Option 1 (Linux):** `echo "youremail@example.org:yourpassword" | base64`
    * **Option 2:** Use an [online tool](https://www.base64encode.net/)
1. Insert your newly generated hash in
    * [`sharex-images.json`](scripts/sharex-images.json) and
    * [`sharex-shortlinks.json`](scripts/sharex-shortlinks.json)
1. Import both files as custom uploaders in ShareX

## 🧩 Project History
The project's origins lie in 2014, back when the [MEAN stack](https://www.mongodb.com/mean-stack) was the sh*t. It was the author's first real web project and a great opportunity to learn. The project is maintained ever since, however, considered mostly feature-complete. Dependencies are updated occasionally. Because the project started quite a couple of years ago, some parts are still based on old-fashioned JavaScript ES5 syntax, alongside vintage tools like [Grunt](https://gruntjs.com/) and [Bower](https://bower.io/). Certainly, this is not state-of-the-art in web dev anymore. However, to keep consistency with existing code, the original code style should still be followed in new contributions. **Update:** Just [recently](https://github.com/muety/anchr/issues/54), all backend-side code was refactored to modern JavaScript syntax to ease development. 

## 🧑‍💻 Developer Notes
### API Tests
```bash
npm install -g newman
```

```bash
newman run "Anchr.postman_collection.json" \
    -e "Anchr Environment.postman_environment.json" \
    --env-var "test_password=ssshhhh"
```

### Upgrade packges
```bash
# Backend
$ yarn plugin import interactive-tools
$ yarn upgrade-interactive

# Frontend
$ cat bower.json | jq   '.dependencies | keys[]' -r | xargs npx bower update
```

## � Acknowledgments

### Original Project Maintainer
A huge thank you to **[Ferdinand Mütsch](https://muetsch.io)** ([@muety](https://github.com/muety)) for creating and maintaining the original Anchr project. This incredible toolbox has proven to be an excellent foundation for learning DevOps practices and containerization. Your dedication to open-source software and the comprehensive documentation you've provided have been invaluable to this internship project.

**Original Repository**: [github.com/muety/anchr](https://github.com/muety/anchr)

### Special Thanks
Special thanks to **[Amir Hossein Fattahi](https://github.com/amixsty)** from **partsoftware** for their guidance, mentorship, and support throughout this DevOps internship project. Your expertise in containerization, monitoring, and infrastructure management has been instrumental in creating this production-ready deployment.

### Community
Thanks to the open-source community for amazing tools like:
- Docker & Docker Compose for containerization
- Nginx for robust reverse proxy capabilities
- Prometheus for powerful metrics collection
- Grafana for beautiful data visualization
- MongoDB for flexible data storage
- Node.js ecosystem for the application runtime

## �📓 License
GNU General Public License v3 (GPL-3) @ [Ferdinand Mütsch](https://muetsch.io)
