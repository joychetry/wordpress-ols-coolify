# WordPress OpenLiteSpeed Super Optimized Docker Image

> ⚠️ **TESTING/BETA**: This image is currently in testing phase. Use in production at your own risk.

## Overview

This Docker image provides a highly optimized WordPress environment based on OpenLiteSpeed, specifically designed for use with Coolify 4 and other systems using Traefik/Caddy as reverse proxies.

### Tech Stack

- **OpenLiteSpeed** (1.8.2): High-performance web server, optimized alternative to Apache
- **PHP 8.2**: Latest stable version with WordPress optimizations
- **WordPress** (6.4.3+): Latest stable version
- **MariaDB** (11.4): Database optimized for WordPress
- **Relay**: In-memory cache optimized for PHP 8.2
- **WP-CLI**: Command-line tool for managing WordPress

### Key Features

- 🚀 Optimized performance for WordPress with builders (Elementor, etc.)
- 🔒 Hardened security configurations
- 💾 Advanced caching with Relay
- ⚡ OpenLiteSpeed pre-configured for WordPress
- 🛠️ PHP configuration optimized for modern CMS
- 🔄 Guaranteed compatibility with Traefik/Caddy

## Usage with Coolify 4

1. In Coolify dashboard, create a new application
2. Select "Docker Image" as deployment type
3. Use image: `anideaforbusiness/wordpress-ols-super-optimized-eg:latest`
4. Configure required environment variables (see Environment Variables section)

5. ### Detailed Coolify Deployment Guide

#### Prerequisites
- Coolify instance running (v4.0+)
- Docker and Docker Compose installed on your server
- Domain names ready for WordPress and PhpMyAdmin (optional)

#### Step-by-Step Deployment

**Step 1: Prepare Your Environment File**

```bash
# Clone or pull the repository
git clone https://github.com/joychetry/wordpress-ols-coolify.git
cd wordpress-ols-coolify

# Copy the example environment file
cp .env.example .env

# Edit .env with your specific settings
nano .env
```

**Step 2: Configure Environment Variables**

Key variables to set in your `.env` file:

```bash
# Coolify-specific (REQUIRED)
SERVICE_FQDN_WORDPRESS=yourdomain.com
SERVICE_FQDN_PHPMYADMIN=phpmyadmin.yourdomain.com

# Database (REQUIRED - Change these!)
WORDPRESS_DB_PASSWORD=your-secure-password
MYSQL_ROOT_PASSWORD=your-mysql-root-password

# WordPress Admin (REQUIRED - Change these!)
ADMIN_PASSWORD=your-admin-password
ADMIN_EMAIL=your-email@example.com

# Coolify Mount Path (Update with your app ID)
COOLIFY_MOUNT_PATH=/data/coolify/applications/YOUR_APP_ID
```

**Step 3: Deploy via Coolify UI**

1. **Login to Coolify Dashboard**
   - Go to `https://your-coolify-instance.com`
   - Sign in to your account

2. **Create New Application**
   - Click "New Project" → "New Application"
   - Name it (e.g., "WordPress Blog")
   - Select your server/destination

3. **Add Docker Compose Deployment**
   - Choose deployment type: "Docker Compose"
   - For source, select one of:
     - **GitHub**: Point to your fork (automated updates)
     - **Upload**: Paste the contents of `docker-compose.yml`

4. **Configure Container Settings**
   - **Docker Image**: `anideaforbusiness/wordpress-ols-super-optimized-eg:latest`
   - **Ports**: Leave as-is (Traefik will handle routing)
   - **Volumes**: Already configured in docker-compose.yml

5. **Add Environment Variables**
   - Click "Environment Variables"
   - Add all variables from your `.env` file:
     - `WORDPRESS_VERSION=6.4`
     - `WORDPRESS_DB_PASSWORD=your-secure-password`
     - `ADMIN_PASSWORD=your-admin-password`
     - `SERVICE_FQDN_WORDPRESS=yourdomain.com`
     - etc.
   - **Important**: Coolify will auto-escape these values

6. **Configure Domain & SSL**
   - **WordPress Service**:
     - Domain: Set to your `SERVICE_FQDN_WORDPRESS`
     - Coolify auto-generates SSL certificates via Let's Encrypt
     - HTTP/HTTPS redirects handled automatically
   
   - **PhpMyAdmin Service**:
     - Domain: Set to your `SERVICE_FQDN_PHPMYADMIN`
     - SSL auto-configured

7. **Deploy**
   - Click "Deploy" button
   - Coolify will:
     - Pull the Docker image
     - Create volumes and networks
     - Start containers
     - Configure Traefik reverse proxy
     - Generate SSL certificates
     - Make services accessible on your domains

8. **Verify Deployment**
   - Check service status in Coolify dashboard (should be "Running")
   - Visit `https://yourdomain.com` → WordPress installation
   - Visit `https://phpmyadmin.yourdomain.com` → Database management

#### Post-Deployment Steps

**1. Complete WordPress Installation**
```
- Visit https://yourdomain.com
- Complete the 5-minute WordPress setup
- Use credentials from your .env file
```

**2. Access PhpMyAdmin (Optional)**
```
- Visit https://phpmyadmin.yourdomain.com
- Login with WordPress DB credentials
```

**3. Configure WordPress**
- Go to Settings → General → Site Address
- Ensure it matches your domain
- Set Permalink structure to "Post name" for SEO
- Install essential plugins (Yoast SEO, Wordfence, etc.)

**4. Enable HTTPS Redirect (Recommended)**
- Coolify handles this automatically
- Verify in Traefik dashboard

#### Troubleshooting

**Container won't start?**
- Check environment variables are properly set
- Ensure passwords don't contain special characters that need escaping
- Check Coolify logs: Application → Logs

**Can't reach WordPress?**
- Verify domain DNS points to your server
- Check Coolify Traefik configuration
- Ensure ports 80/443 are open on firewall

**PhpMyAdmin not accessible?**
- Verify SERVICE_FQDN_PHPMYADMIN is set
- Check Traefik labels in docker-compose.yml
- Ensure DNS for phpmyadmin subdomain is configured

**Database connection failed?**
- Verify WORDPRESS_DB_PASSWORD matches MYSQL_PASSWORD in .env
- Check MySQL container is running: `docker ps`
- Verify network connectivity between WordPress and MySQL containers

#### Performance Optimization

This image includes:
- ✅ OpenLiteSpeed (2-3x faster than Apache)
- ✅ Relay cache for PHP sessions
- ✅ MariaDB optimization
- ✅ Gzip compression
- ✅ HTTP/2 support
- ✅ Automatic SSL via Let's Encrypt

No additional optimization needed!

#### Scaling & Maintenance

**Backup Database**
```bash
docker exec wordpress-mysql mysqldump -u wordpress -p${WORDPRESS_DB_PASSWORD} wordpress > backup.sql
```

**Update WordPress in Coolify**
- Go to Application → Redeploy
- Coolify pulls latest image and restarts
- Zero-downtime updates due to Traefik routing

**Update Environment Variables**
- Edit in Coolify dashboard
- Redeploy for changes to take effect

#### Support & Issues

- **GitHub Issues**: Submit on this repository
- **Coolify Issues**: Check Coolify documentation
- **WordPress Issues**: Visit wordpress.org/support

---

## Database Credentials

See `.env.example` for a complete list of configuration options.

## License

This is a fork of [fActorius/wordpress-superoptimized-v2](https://github.com/fActorius/wordpress-superoptimized-v2) with Coolify-specific optimizations.

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request
