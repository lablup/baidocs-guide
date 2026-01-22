# Hosting Options

BaiDocs supports multiple hosting strategies for documentation deployment, from static site hosting to containerized environments. This section covers hosting platform configuration, deployment automation, and performance optimization for production environments.

## Hosting Platform Overview

### Static Site Hosting

Static hosting platforms offer optimal performance and cost-effectiveness for documentation sites:

- **Vercel**: Zero-configuration deployment with global CDN
- **Netlify**: Continuous deployment with built-in form handling
- **GitHub Pages**: Direct integration with GitHub repositories
- **Cloudflare Pages**: Global edge network with advanced caching
- **AWS S3 + CloudFront**: Scalable enterprise hosting solution
- **Azure Static Web Apps**: Microsoft cloud platform integration

### Container-based Hosting

For dynamic features and server-side functionality:

- **Docker + Nginx**: Self-hosted container deployment
- **Kubernetes**: Orchestrated container management
- **Google Cloud Run**: Serverless container platform
- **AWS Fargate**: Managed container hosting
- **Azure Container Instances**: Simplified container deployment

## Static Site Deployment

### Vercel Deployment

#### Basic Vercel Configuration

```json
// vercel.json
{
  "version": 2,
  "name": "baidocs-documentation",
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/next"
    }
  ],
  "routes": [
    {
      "src": "/sitemap.xml",
      "headers": {
        "Content-Type": "application/xml"
      }
    },
    {
      "src": "/robots.txt",
      "headers": {
        "Content-Type": "text/plain"
      }
    },
    {
      "src": "/(.*)",
      "dest": "/$1"
    }
  ],
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "X-Content-Type-Options",
          "value": "nosniff"
        },
        {
          "key": "X-Frame-Options",
          "value": "DENY"
        },
        {
          "key": "X-XSS-Protection",
          "value": "1; mode=block"
        }
      ]
    },
    {
      "source": "/static/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    }
  ],
  "redirects": [
    {
      "source": "/docs/:path*",
      "destination": "/:path*",
      "permanent": true
    }
  ],
  "cleanUrls": true,
  "trailingSlash": false
}
```

#### Vercel Environment Configuration

```bash
# .env.production
NEXT_PUBLIC_SITE_URL=https://docs.example.com
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX
NEXT_PUBLIC_ALGOLIA_APP_ID=YOUR_APP_ID
NEXT_PUBLIC_ALGOLIA_SEARCH_KEY=YOUR_SEARCH_KEY

# Build-time environment variables
BUILD_TIME=2026-01-20T00:00:00Z
BUILD_VERSION=1.0.0
ENABLE_ANALYTICS=true
ENABLE_SEARCH=true
```

### Netlify Deployment

#### Netlify Configuration

```toml
# netlify.toml
[build]
  publish = "out"
  command = "npm run build:static"

[build.environment]
  NODE_VERSION = "18"
  NPM_VERSION = "9"

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-Content-Type-Options = "nosniff"
    Referrer-Policy = "strict-origin-when-cross-origin"

[[headers]]
  for = "/static/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/*.js"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/*.css"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[redirects]]
  from = "/docs/*"
  to = "/:splat"
  status = 301

[[redirects]]
  from = "/api/*"
  to = "/.netlify/functions/:splat"
  status = 200

[functions]
  directory = "netlify/functions"
  node_bundler = "esbuild"

[dev]
  command = "npm run dev"
  port = 3000
  publish = "out"
```

#### Netlify Build Plugins

```javascript
// netlify/plugins/manifest.js
module.exports = {
  onPreBuild: async ({ inputs, utils }) => {
    console.log('Generating build manifest...');

    const manifest = {
      buildTime: new Date().toISOString(),
      version: process.env.npm_package_version,
      commit: process.env.COMMIT_REF || 'unknown',
      branch: process.env.BRANCH || 'unknown',
    };

    await utils.build.failBuild('Build manifest created successfully');
  },

  onPostBuild: async ({ inputs, utils }) => {
    console.log('Optimizing build output...');

    // Compress HTML files
    await utils.run.command('find out -name "*.html" -exec gzip -k {} \\;');

    // Generate service worker
    await utils.run.command('npm run generate:sw');

    console.log('Post-build optimization completed');
  },
};
```

### GitHub Pages Deployment

#### GitHub Actions Workflow

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    name: Build Documentation
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: npm

      - name: Setup Pages
        uses: actions/configure-pages@v3
        with:
          static_site_generator: next

      - name: Install dependencies
        run: npm ci

      - name: Build with Next.js
        run: npm run build:static

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./out

  deploy:
    name: Deploy to GitHub Pages
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

## Container-based Deployment

### Docker Configuration

#### Multi-stage Dockerfile

```dockerfile
# Dockerfile
FROM node:18-alpine AS base

# Install dependencies only when needed
FROM base AS deps
WORKDIR /app

# Copy package files
COPY package*.json ./
COPY turbo.json ./

# Install dependencies
RUN npm ci --only=production && npm cache clean --force

# Rebuild the source code only when needed
FROM base AS builder
WORKDIR /app

COPY --from=deps /app/node_modules ./node_modules
COPY . .

# Build the application
ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

RUN npm run build:static

# Production image, copy all the files and run nginx
FROM nginx:alpine AS runner

# Copy built application
COPY --from=builder /app/out /usr/share/nginx/html

# Copy nginx configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Add health check
COPY healthcheck.sh /healthcheck.sh
RUN chmod +x /healthcheck.sh

# Expose port
EXPOSE 80

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD /healthcheck.sh

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
```

#### Nginx Configuration

```nginx
# nginx.conf
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log notice;
pid /var/run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    # Logging
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                   '$status $body_bytes_sent "$http_referer" '
                   '"$http_user_agent" "$http_x_forwarded_for"';
    access_log /var/log/nginx/access.log main;

    # Performance
    sendfile on;
    tcp_nopush on;
    keepalive_timeout 65;
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_types text/plain text/css text/xml text/javascript
               application/javascript application/xml+rss
               application/json image/svg+xml;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;

    server {
        listen 80;
        listen [::]:80;
        server_name _;

        root /usr/share/nginx/html;
        index index.html;

        # Static assets caching
        location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|webp)$ {
            expires 1y;
            add_header Cache-Control "public, immutable";
        }

        # HTML files - no cache
        location ~* \.html$ {
            expires -1;
            add_header Cache-Control "no-cache, no-store, must-revalidate";
        }

        # Main routing
        location / {
            try_files $uri $uri.html $uri/ /index.html;
        }

        # API routes (if using hybrid deployment)
        location /api/ {
            proxy_pass http://api-server:3001;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_cache_bypass $http_upgrade;
        }

        # Health check endpoint
        location /health {
            access_log off;
            return 200 "healthy\n";
            add_header Content-Type text/plain;
        }

        # Error pages
        error_page 404 /404.html;
        error_page 500 502 503 504 /50x.html;
    }
}
```

### Kubernetes Deployment

#### Kubernetes Manifests

```yaml
# k8s-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: baidocs
  namespace: documentation
  labels:
    app: baidocs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: baidocs
  template:
    metadata:
      labels:
        app: baidocs
    spec:
      containers:
      - name: baidocs
        image: your-registry/baidocs:latest
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "64Mi"
            cpu: "50m"
          limits:
            memory: "128Mi"
            cpu: "100m"
        livenessProbe:
          httpGet:
            path: /health
            port: 80
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /health
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
        env:
        - name: NODE_ENV
          value: "production"

---
apiVersion: v1
kind: Service
metadata:
  name: baidocs-service
  namespace: documentation
spec:
  selector:
    app: baidocs
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP

---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: baidocs-ingress
  namespace: documentation
  annotations:
    kubernetes.io/ingress.class: "nginx"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
spec:
  tls:
  - hosts:
    - docs.example.com
    secretName: baidocs-tls
  rules:
  - host: docs.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: baidocs-service
            port:
              number: 80
```

## CDN and Performance Optimization

### CloudFront Configuration

```javascript
// aws-cloudfront.js
const AWS = require('aws-sdk');

const cloudfrontConfig = {
  CallerReference: 'baidocs-distribution-' + Date.now(),
  Comment: 'BaiDocs Documentation Distribution',
  DefaultCacheBehavior: {
    TargetOriginId: 'S3-baidocs-docs',
    ViewerProtocolPolicy: 'redirect-to-https',
    AllowedMethods: {
      Quantity: 7,
      Items: ['GET', 'HEAD', 'OPTIONS', 'PUT', 'POST', 'PATCH', 'DELETE'],
      CachedMethods: {
        Quantity: 2,
        Items: ['GET', 'HEAD']
      }
    },
    ForwardedValues: {
      QueryString: false,
      Cookies: {
        Forward: 'none'
      },
      Headers: {
        Quantity: 0
      }
    },
    MinTTL: 0,
    DefaultTTL: 86400,
    MaxTTL: 31536000,
    Compress: true
  },
  CacheBehaviors: {
    Quantity: 3,
    Items: [
      {
        PathPattern: '/static/*',
        TargetOriginId: 'S3-baidocs-docs',
        ViewerProtocolPolicy: 'https-only',
        MinTTL: 31536000,
        DefaultTTL: 31536000,
        MaxTTL: 31536000,
        ForwardedValues: {
          QueryString: false,
          Cookies: { Forward: 'none' }
        },
        Compress: true
      },
      {
        PathPattern: '/_next/static/*',
        TargetOriginId: 'S3-baidocs-docs',
        ViewerProtocolPolicy: 'https-only',
        MinTTL: 31536000,
        DefaultTTL: 31536000,
        MaxTTL: 31536000,
        ForwardedValues: {
          QueryString: false,
          Cookies: { Forward: 'none' }
        },
        Compress: true
      },
      {
        PathPattern: '/api/*',
        TargetOriginId: 'API-Gateway',
        ViewerProtocolPolicy: 'https-only',
        MinTTL: 0,
        DefaultTTL: 0,
        MaxTTL: 0,
        ForwardedValues: {
          QueryString: true,
          Cookies: { Forward: 'all' },
          Headers: {
            Quantity: 3,
            Items: ['Authorization', 'Content-Type', 'Accept']
          }
        }
      }
    ]
  },
  Origins: {
    Quantity: 2,
    Items: [
      {
        Id: 'S3-baidocs-docs',
        DomainName: 'baidocs-docs.s3.amazonaws.com',
        S3OriginConfig: {
          OriginAccessIdentity: 'origin-access-identity/cloudfront/ABCDEFGHIJKLMN'
        }
      },
      {
        Id: 'API-Gateway',
        DomainName: 'api.example.com',
        CustomOriginConfig: {
          HTTPPort: 443,
          HTTPSPort: 443,
          OriginProtocolPolicy: 'https-only',
          OriginSSLProtocols: {
            Quantity: 3,
            Items: ['TLSv1', 'TLSv1.1', 'TLSv1.2']
          }
        }
      }
    ]
  },
  Enabled: true,
  PriceClass: 'PriceClass_All',
  HttpVersion: 'http2',
  IsIPV6Enabled: true
};
```

## Monitoring and Analytics

### Performance Monitoring Setup

```javascript
// monitoring.js
const { RealUserMonitoring } = require('@aws-observability/rum-web');
const { Analytics } = require('@vercel/analytics');

// Initialize monitoring
const rum = new RealUserMonitoring({
  applicationId: 'your-app-id',
  applicationVersion: '1.0.0',
  applicationRegion: 'us-east-1',
  identityPoolId: 'your-identity-pool-id',

  // Session configuration
  sessionSampleRate: 1.0,
  guestRoleArn: 'arn:aws:iam::123456789012:role/RUM-Monitor-us-east-1-123456789012-Unauth',

  // Page load monitoring
  enableXRay: true,
  enableRumClient: true,

  // Custom metrics
  telemetries: [
    'performance',
    'errors',
    'http'
  ]
});

// Custom performance metrics
export const trackDocumentationMetrics = () => {
  // Track time to first meaningful paint
  const observer = new PerformanceObserver((list) => {
    for (const entry of list.getEntries()) {
      if (entry.name === 'first-meaningful-paint') {
        rum.recordCustomEvent('documentation_load_time', {
          value: entry.startTime,
          unit: 'milliseconds'
        });
      }
    }
  });

  observer.observe({ entryTypes: ['paint', 'navigation'] });

  // Track search performance
  const trackSearch = (query, resultCount, duration) => {
    rum.recordCustomEvent('search_performance', {
      query_length: query.length,
      result_count: resultCount,
      search_duration: duration,
      unit: 'milliseconds'
    });
  };

  // Track page transitions
  const trackPageTransition = (from, to, duration) => {
    rum.recordCustomEvent('page_transition', {
      from_page: from,
      to_page: to,
      transition_duration: duration,
      unit: 'milliseconds'
    });
  };

  return { trackSearch, trackPageTransition };
};

// Error tracking
export const setupErrorTracking = () => {
  window.addEventListener('error', (event) => {
    rum.recordError({
      name: 'JavaScript Error',
      message: event.error.message,
      stack: event.error.stack,
      filename: event.filename,
      lineno: event.lineno,
      colno: event.colno
    });
  });

  window.addEventListener('unhandledrejection', (event) => {
    rum.recordError({
      name: 'Unhandled Promise Rejection',
      message: event.reason.message || 'Unhandled promise rejection',
      stack: event.reason.stack
    });
  });
};
```

### Health Check Implementation

```javascript
// health-check.js
const express = require('express');
const app = express();

// Health check endpoint
app.get('/health', async (req, res) => {
  const healthcheck = {
    uptime: process.uptime(),
    message: 'OK',
    timestamp: new Date().toISOString(),
    checks: {}
  };

  try {
    // Database connectivity check
    healthcheck.checks.database = await checkDatabase();

    // External service checks
    healthcheck.checks.search = await checkSearchService();

    // Memory usage check
    const memUsage = process.memoryUsage();
    healthcheck.checks.memory = {
      used: Math.round(memUsage.heapUsed / 1024 / 1024 * 100) / 100,
      free: Math.round(memUsage.heapTotal / 1024 / 1024 * 100) / 100,
      status: memUsage.heapUsed < memUsage.heapTotal * 0.9 ? 'OK' : 'WARNING'
    };

    // Disk space check
    healthcheck.checks.disk = await checkDiskSpace();

    res.status(200).json(healthcheck);

  } catch (error) {
    healthcheck.message = 'Service Unavailable';
    healthcheck.error = error.message;
    res.status(503).json(healthcheck);
  }
});

async function checkDatabase() {
  // Database health check logic
  return { status: 'OK', responseTime: '< 100ms' };
}

async function checkSearchService() {
  // Search service health check
  return { status: 'OK', responseTime: '< 50ms' };
}

async function checkDiskSpace() {
  const fs = require('fs').promises;
  try {
    const stats = await fs.statfs('./');
    const free = (stats.bavail * stats.bsize) / 1024 / 1024 / 1024; // GB
    return {
      status: free > 1 ? 'OK' : 'WARNING',
      freeSpace: `${Math.round(free * 100) / 100}GB`
    };
  } catch (error) {
    return { status: 'ERROR', error: error.message };
  }
}

module.exports = app;
```

## Best Practices and Security

### Security Headers Implementation

```javascript
// security-headers.js
const securityHeaders = {
  // Prevent clickjacking
  'X-Frame-Options': 'DENY',

  // Prevent MIME type sniffing
  'X-Content-Type-Options': 'nosniff',

  // Enable XSS protection
  'X-XSS-Protection': '1; mode=block',

  // Referrer policy
  'Referrer-Policy': 'strict-origin-when-cross-origin',

  // Content Security Policy
  'Content-Security-Policy': [
    "default-src 'self'",
    "script-src 'self' 'unsafe-eval' 'unsafe-inline' *.googletagmanager.com",
    "style-src 'self' 'unsafe-inline' fonts.googleapis.com",
    "font-src 'self' fonts.gstatic.com",
    "img-src 'self' data: https:",
    "connect-src 'self' *.algolia.net *.algolianet.com",
    "frame-src 'none'",
    "object-src 'none'",
    "base-uri 'self'"
  ].join('; '),

  // Strict Transport Security
  'Strict-Transport-Security': 'max-age=31536000; includeSubDomains; preload',

  // Permissions Policy
  'Permissions-Policy': [
    'geolocation=()',
    'microphone=()',
    'camera=()',
    'midi=()',
    'encrypted-media=()',
    'payment=()',
    'usb=()'
  ].join(', ')
};

module.exports = securityHeaders;
```

### Deployment Checklist

```markdown
## Pre-deployment Checklist

### Performance
- [ ] Build size within budget (< 350KB initial bundle)
- [ ] Images optimized and compressed
- [ ] Critical CSS inlined
- [ ] Service worker implemented for offline support
- [ ] CDN configured for static assets

### SEO
- [ ] All pages have proper meta tags
- [ ] Sitemap.xml generated and submitted
- [ ] Robots.txt configured
- [ ] Open Graph meta tags implemented
- [ ] Schema.org structured data added

### Security
- [ ] Security headers configured
- [ ] HTTPS enforced
- [ ] Content Security Policy implemented
- [ ] No sensitive data exposed in client-side code
- [ ] Dependencies security audit passed

### Accessibility
- [ ] WCAG 2.1 AA compliance verified
- [ ] Screen reader testing completed
- [ ] Keyboard navigation tested
- [ ] Color contrast ratios meet standards
- [ ] Alt text provided for all images

### Monitoring
- [ ] Error tracking configured
- [ ] Performance monitoring enabled
- [ ] Health checks implemented
- [ ] Uptime monitoring configured
- [ ] Analytics tracking verified

### Documentation
- [ ] Deployment documentation updated
- [ ] Runbooks created for common issues
- [ ] Contact information for support team
- [ ] Rollback procedures documented
```

BaiDocs hosting options provide flexible deployment strategies that scale from simple static hosting to enterprise-grade container orchestration, ensuring optimal performance and reliability for documentation delivery worldwide.