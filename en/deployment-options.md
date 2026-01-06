---
title: Deployment Options
description: Deploy your BaiDocs documentation to various platforms
---

# Deployment Options

BaiDocs can be deployed to many different platforms. This guide covers the most popular options.

## Deployment Methods

BaiDocs supports two deployment approaches:

1. **Static Export**: Pre-rendered static files
2. **Node.js Server**: Dynamic server with SSR

## Vercel (Recommended)

Vercel is the easiest option for deploying Next.js applications.

### Deploy with Vercel CLI

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel

# Production deployment
vercel --prod
```

### Deploy with GitHub Integration

1. Push code to GitHub
2. Go to [vercel.com](https://vercel.com)
3. Click "Import Project"
4. Select your repository
5. Configure build settings:
   - **Framework**: Next.js
   - **Root Directory**: `apps/viewer`
   - **Build Command**: `pnpm build`
   - **Output Directory**: `.next`

6. Add environment variables:
   ```
   ENABLE_PDF_GENERATION=true
   NODE_OPTIONS=--max-old-space-size=4096
   ```

7. Deploy!

:::tip
Vercel automatically redeploys when you push to your repository.
:::

## Netlify

Deploy to Netlify for static hosting.

### Using Netlify CLI

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Build for static export
pnpm build

# Deploy
netlify deploy

# Production deployment
netlify deploy --prod
```

### Configuration

Create `netlify.toml`:

```toml
[build]
  command = "cd apps/viewer && pnpm build"
  publish = "apps/viewer/.next"

[build.environment]
  NODE_VERSION = "18"
  ENABLE_PDF_GENERATION = "true"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

## Docker

Deploy using Docker containers.

### Dockerfile

Create `Dockerfile`:

```dockerfile
FROM node:18-alpine AS base

# Install pnpm
RUN npm install -g pnpm

# Install dependencies
FROM base AS deps
WORKDIR /app
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY apps/viewer/package.json ./apps/viewer/
COPY packages/*/package.json ./packages/*/
RUN pnpm install --frozen-lockfile

# Build application
FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

# Build
ENV ENABLE_PDF_GENERATION=true
ENV NODE_OPTIONS=--max-old-space-size=4096
RUN pnpm build:viewer

# Production image
FROM base AS runner
WORKDIR /app

ENV NODE_ENV production
ENV PORT 3000

# Copy built application
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/apps/viewer/.next/static ./apps/viewer/.next/static
COPY --from=builder /app/apps/viewer/public ./apps/viewer/public

# Expose port
EXPOSE 3000

# Start server
CMD ["node", "apps/viewer/server.js"]
```

### Build and Run

```bash
# Build image
docker build -t baidocs .

# Run container
docker run -p 3000:3000 baidocs
```

### Docker Compose

Create `docker-compose.yml`:

```yaml
version: '3.8'

services:
  baidocs:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - PORT=3000
    restart: unless-stopped
    volumes:
      - ./content:/app/content:ro
```

Run:

```bash
docker-compose up -d
```

## AWS

Deploy to Amazon Web Services.

### AWS Amplify

1. Push code to Git repository
2. Go to AWS Amplify Console
3. Click "New App" → "Host web app"
4. Connect repository
5. Configure build:
   - **Base directory**: `apps/viewer`
   - **Build command**: `pnpm install && pnpm build`
   - **Publish directory**: `.next`

### AWS S3 + CloudFront

For static hosting:

```bash
# Build static export
pnpm build

# Sync to S3
aws s3 sync .next/out s3://your-bucket-name

# Invalidate CloudFront cache
aws cloudfront create-invalidation \
  --distribution-id YOUR_DIST_ID \
  --paths "/*"
```

### AWS EC2

Deploy to EC2 instance:

```bash
# SSH into instance
ssh ubuntu@your-instance.amazonaws.com

# Install Node.js and pnpm
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
npm install -g pnpm

# Clone repository
git clone your-repo-url
cd your-repo

# Install and build
pnpm install
ENABLE_PDF_GENERATION=true pnpm build

# Start with PM2
npm install -g pm2
pm2 start .next/standalone/apps/viewer/server.js --name baidocs
pm2 startup
pm2 save
```

## Google Cloud Platform

Deploy to GCP.

### Cloud Run

Create `Dockerfile` (see Docker section), then:

```bash
# Build and push image
gcloud builds submit --tag gcr.io/PROJECT_ID/baidocs

# Deploy
gcloud run deploy baidocs \
  --image gcr.io/PROJECT_ID/baidocs \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated
```

### App Engine

Create `app.yaml`:

```yaml
runtime: nodejs18

instance_class: F4

env_variables:
  NODE_ENV: 'production'
  ENABLE_PDF_GENERATION: 'true'

automatic_scaling:
  min_instances: 1
  max_instances: 10
```

Deploy:

```bash
gcloud app deploy
```

## DigitalOcean

Deploy to DigitalOcean.

### App Platform

1. Connect Git repository
2. Configure app:
   - **Type**: Web Service
   - **Run Command**: `node .next/standalone/apps/viewer/server.js`
   - **Build Command**: `pnpm install && pnpm build`

### Droplet

Similar to AWS EC2:

```bash
# Create droplet
# SSH into droplet
# Install Node.js and pnpm
# Clone, build, and run
```

## Self-Hosted

Host on your own server.

### Using PM2

```bash
# Install PM2
npm install -g pm2

# Start application
cd .next/standalone/apps/viewer
pm2 start server.js --name baidocs

# Save PM2 configuration
pm2 startup
pm2 save

# Monitor
pm2 monit
```

### Using systemd

Create `/etc/systemd/system/baidocs.service`:

```ini
[Unit]
Description=BaiDocs Documentation
After=network.target

[Service]
Type=simple
User=www-data
WorkingDirectory=/var/www/baidocs/.next/standalone/apps/viewer
ExecStart=/usr/bin/node server.js
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Enable and start:

```bash
sudo systemctl enable baidocs
sudo systemctl start baidocs
sudo systemctl status baidocs
```

### Nginx Reverse Proxy

Configure Nginx:

```nginx
server {
    listen 80;
    server_name docs.example.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## Environment Configuration

### Production Variables

Set for production:

```bash
NODE_ENV=production
PORT=3000
NEXT_PUBLIC_SITE_URL=https://docs.example.com
```

### Secrets Management

Use platform-specific secret management:

**Vercel:**
- Add secrets in project settings

**AWS:**
- Use AWS Secrets Manager or Parameter Store

**Docker:**
- Use Docker secrets or environment files

## SSL/HTTPS

### Let's Encrypt (Free)

Using Certbot:

```bash
# Install Certbot
sudo apt-get install certbot python3-certbot-nginx

# Obtain certificate
sudo certbot --nginx -d docs.example.com

# Auto-renewal
sudo certbot renew --dry-run
```

### CloudFlare

1. Add your domain to CloudFlare
2. Enable "Flexible" or "Full" SSL
3. Update DNS settings

## Custom Domain

### Vercel

1. Go to project settings
2. Click "Domains"
3. Add your custom domain
4. Update DNS records as instructed

### Netlify

1. Go to "Domain settings"
2. Add custom domain
3. Configure DNS

## Performance Optimization

### CDN

Use a CDN for static assets:

- CloudFront (AWS)
- Cloud CDN (GCP)
- Cloudflare
- Fastly

### Caching

Configure caching headers:

```nginx
# Nginx example
location /_next/static/ {
    expires 365d;
    add_header Cache-Control "public, immutable";
}

location /public/ {
    expires 7d;
    add_header Cache-Control "public";
}
```

### Compression

Enable gzip/brotli:

```nginx
gzip on;
gzip_types text/plain text/css application/json application/javascript;
gzip_min_length 1000;
```

## Monitoring

### Uptime Monitoring

Use services like:
- UptimeRobot
- Pingdom
- StatusCake

### Error Tracking

Integrate error tracking:

```javascript
// Sentry example
import * as Sentry from "@sentry/nextjs";

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.NODE_ENV,
});
```

### Analytics

Add analytics:

```javascript
// Google Analytics
import { Analytics } from '@vercel/analytics/react';

function MyApp({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
      <Analytics />
    </>
  );
}
```

## Deployment Checklist

Before deploying:

- [ ] Build completes successfully
- [ ] Environment variables configured
- [ ] Custom domain setup
- [ ] SSL certificate installed
- [ ] CDN configured
- [ ] Monitoring enabled
- [ ] Analytics integrated
- [ ] Error tracking setup
- [ ] Backup plan in place
- [ ] Rollback procedure documented

## Troubleshooting

### Deployment Fails

Check:
1. Build logs for errors
2. Environment variables
3. Memory limits
4. Build timeouts

### Site Not Loading

Verify:
1. DNS settings
2. SSL certificate
3. Server running
4. Firewall rules
5. Port accessibility

### Performance Issues

Optimize:
1. Enable caching
2. Use CDN
3. Compress assets
4. Optimize images
5. Review bundle size

## Updating Deployment

### Zero-Downtime Updates

Using PM2:

```bash
pm2 reload baidocs
```

Using Docker:

```bash
# Build new image
docker build -t baidocs:new .

# Update service
docker service update --image baidocs:new baidocs
```

## Backup and Recovery

### Backup Content

```bash
# Backup content directory
tar -czf content-backup-$(date +%Y%m%d).tar.gz content/

# Upload to S3
aws s3 cp content-backup-*.tar.gz s3://backups/
```

### Database Backups

If using a database:

```bash
# MongoDB example
mongodump --out=backup-$(date +%Y%m%d)

# PostgreSQL example
pg_dump dbname > backup-$(date +%Y%m%d).sql
```

## Conclusion

You now have all the knowledge needed to deploy BaiDocs! Choose the platform that best fits your needs and follow the corresponding guide.

:::success
Congratulations on completing the BaiDocs Guide! You're ready to create amazing documentation.
:::
