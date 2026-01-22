# 호스팅 옵션

BaiDocs는 정적 사이트 호스팅부터 컨테이너화된 환경까지 다양한 문서 배포 호스팅 전략을 지원합니다. 이 섹션에서는 호스팅 플랫폼 구성, 배포 자동화, 프로덕션 환경을 위한 성능 최적화를 다룹니다.

## 호스팅 플랫폼 개요

### 정적 사이트 호스팅

정적 호스팅 플랫폼은 문서 사이트에 최적의 성능과 비용 효율성을 제공합니다:

- **Vercel**: 글로벌 CDN이 있는 무구성 배포
- **Netlify**: 내장 폼 처리 기능이 있는 지속적 배포
- **GitHub Pages**: GitHub 저장소와의 직접 통합
- **Cloudflare Pages**: 고급 캐싱 기능이 있는 글로벌 엣지 네트워크
- **AWS S3 + CloudFront**: 확장 가능한 엔터프라이즈 호스팅 솔루션
- **Azure Static Web Apps**: Microsoft 클라우드 플랫폼 통합

### 컨테이너 기반 호스팅

동적 기능과 서버 사이드 기능을 위한 옵션:

- **Docker + Nginx**: 셀프 호스팅 컨테이너 배포
- **Kubernetes**: 오케스트레이션된 컨테이너 관리
- **Google Cloud Run**: 서버리스 컨테이너 플랫폼
- **AWS Fargate**: 관리형 컨테이너 호스팅
- **Azure Container Instances**: 간소화된 컨테이너 배포

## 정적 사이트 배포

### Vercel 배포

#### 기본 Vercel 구성

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

#### Vercel 환경 구성

```bash
# .env.production
NEXT_PUBLIC_SITE_URL=https://docs.example.com
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX
NEXT_PUBLIC_ALGOLIA_APP_ID=YOUR_APP_ID
NEXT_PUBLIC_ALGOLIA_SEARCH_KEY=YOUR_SEARCH_KEY

# 빌드 시점 환경 변수
BUILD_TIME=2026-01-20T00:00:00Z
BUILD_VERSION=1.0.0
ENABLE_ANALYTICS=true
ENABLE_SEARCH=true
```

### Netlify 배포

#### Netlify 구성

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

#### Netlify 빌드 플러그인

```javascript
// netlify/plugins/manifest.js
module.exports = {
  onPreBuild: async ({ inputs, utils }) => {
    console.log('빌드 매니페스트 생성 중...');

    const manifest = {
      buildTime: new Date().toISOString(),
      version: process.env.npm_package_version,
      commit: process.env.COMMIT_REF || 'unknown',
      branch: process.env.BRANCH || 'unknown',
    };

    await utils.build.failBuild('빌드 매니페스트가 성공적으로 생성되었습니다');
  },

  onPostBuild: async ({ inputs, utils }) => {
    console.log('빌드 출력 최적화 중...');

    // HTML 파일 압축
    await utils.run.command('find out -name "*.html" -exec gzip -k {} \\;');

    // 서비스 워커 생성
    await utils.run.command('npm run generate:sw');

    console.log('빌드 후 최적화 완료');
  },
};
```

### GitHub Pages 배포

#### GitHub Actions 워크플로우

```yaml
name: GitHub Pages에 배포
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
    name: 문서 빌드
    runs-on: ubuntu-latest
    steps:
      - name: 체크아웃
        uses: actions/checkout@v4

      - name: Node.js 설정
        uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: npm

      - name: Pages 설정
        uses: actions/configure-pages@v3
        with:
          static_site_generator: next

      - name: 의존성 설치
        run: npm ci

      - name: Next.js로 빌드
        run: npm run build:static

      - name: 아티팩트 업로드
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./out

  deploy:
    name: GitHub Pages에 배포
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: GitHub Pages에 배포
        id: deployment
        uses: actions/deploy-pages@v2
```

## 컨테이너 기반 배포

### Docker 구성

#### 멀티스테이지 Dockerfile

```dockerfile
# Dockerfile
FROM node:18-alpine AS base

# 필요한 경우에만 의존성 설치
FROM base AS deps
WORKDIR /app

# 패키지 파일 복사
COPY package*.json ./
COPY turbo.json ./

# 의존성 설치
RUN npm ci --only=production && npm cache clean --force

# 필요한 경우에만 소스 코드 재빌드
FROM base AS builder
WORKDIR /app

COPY --from=deps /app/node_modules ./node_modules
COPY . .

# 애플리케이션 빌드
ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

RUN npm run build:static

# 프로덕션 이미지, 모든 파일 복사 후 nginx 실행
FROM nginx:alpine AS runner

# 빌드된 애플리케이션 복사
COPY --from=builder /app/out /usr/share/nginx/html

# nginx 구성 복사
COPY nginx.conf /etc/nginx/nginx.conf

# 헬스 체크 추가
COPY healthcheck.sh /healthcheck.sh
RUN chmod +x /healthcheck.sh

# 포트 노출
EXPOSE 80

# 헬스 체크
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD /healthcheck.sh

# nginx 시작
CMD ["nginx", "-g", "daemon off;"]
```

#### Nginx 구성

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

    # 로깅
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                   '$status $body_bytes_sent "$http_referer" '
                   '"$http_user_agent" "$http_x_forwarded_for"';
    access_log /var/log/nginx/access.log main;

    # 성능
    sendfile on;
    tcp_nopush on;
    keepalive_timeout 65;
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_types text/plain text/css text/xml text/javascript
               application/javascript application/xml+rss
               application/json image/svg+xml;

    # 보안 헤더
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

        # 정적 자산 캐싱
        location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|webp)$ {
            expires 1y;
            add_header Cache-Control "public, immutable";
        }

        # HTML 파일 - 캐시 없음
        location ~* \.html$ {
            expires -1;
            add_header Cache-Control "no-cache, no-store, must-revalidate";
        }

        # 메인 라우팅
        location / {
            try_files $uri $uri.html $uri/ /index.html;
        }

        # API 라우트 (하이브리드 배포 사용 시)
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

        # 헬스 체크 엔드포인트
        location /health {
            access_log off;
            return 200 "healthy\n";
            add_header Content-Type text/plain;
        }

        # 에러 페이지
        error_page 404 /404.html;
        error_page 500 502 503 504 /50x.html;
    }
}
```

### Kubernetes 배포

#### Kubernetes 매니페스트

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

## CDN 및 성능 최적화

### CloudFront 구성

```javascript
// aws-cloudfront.js
const AWS = require('aws-sdk');

const cloudfrontConfig = {
  CallerReference: 'baidocs-distribution-' + Date.now(),
  Comment: 'BaiDocs 문서 배포',
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

## 모니터링 및 분석

### 성능 모니터링 설정

```javascript
// monitoring.js
const { RealUserMonitoring } = require('@aws-observability/rum-web');
const { Analytics } = require('@vercel/analytics');

// 모니터링 초기화
const rum = new RealUserMonitoring({
  applicationId: 'your-app-id',
  applicationVersion: '1.0.0',
  applicationRegion: 'us-east-1',
  identityPoolId: 'your-identity-pool-id',

  // 세션 구성
  sessionSampleRate: 1.0,
  guestRoleArn: 'arn:aws:iam::123456789012:role/RUM-Monitor-us-east-1-123456789012-Unauth',

  // 페이지 로드 모니터링
  enableXRay: true,
  enableRumClient: true,

  // 사용자 정의 메트릭
  telemetries: [
    'performance',
    'errors',
    'http'
  ]
});

// 사용자 정의 성능 메트릭
export const trackDocumentationMetrics = () => {
  // 첫 번째 의미 있는 페인트까지의 시간 추적
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

  // 검색 성능 추적
  const trackSearch = (query, resultCount, duration) => {
    rum.recordCustomEvent('search_performance', {
      query_length: query.length,
      result_count: resultCount,
      search_duration: duration,
      unit: 'milliseconds'
    });
  };

  // 페이지 전환 추적
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

// 에러 추적
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
      message: event.reason.message || '처리되지 않은 프로미스 거부',
      stack: event.reason.stack
    });
  });
};
```

### 헬스 체크 구현

```javascript
// health-check.js
const express = require('express');
const app = express();

// 헬스 체크 엔드포인트
app.get('/health', async (req, res) => {
  const healthcheck = {
    uptime: process.uptime(),
    message: 'OK',
    timestamp: new Date().toISOString(),
    checks: {}
  };

  try {
    // 데이터베이스 연결 확인
    healthcheck.checks.database = await checkDatabase();

    // 외부 서비스 확인
    healthcheck.checks.search = await checkSearchService();

    // 메모리 사용량 확인
    const memUsage = process.memoryUsage();
    healthcheck.checks.memory = {
      used: Math.round(memUsage.heapUsed / 1024 / 1024 * 100) / 100,
      free: Math.round(memUsage.heapTotal / 1024 / 1024 * 100) / 100,
      status: memUsage.heapUsed < memUsage.heapTotal * 0.9 ? 'OK' : 'WARNING'
    };

    // 디스크 공간 확인
    healthcheck.checks.disk = await checkDiskSpace();

    res.status(200).json(healthcheck);

  } catch (error) {
    healthcheck.message = '서비스 사용 불가';
    healthcheck.error = error.message;
    res.status(503).json(healthcheck);
  }
});

async function checkDatabase() {
  // 데이터베이스 헬스 체크 로직
  return { status: 'OK', responseTime: '< 100ms' };
}

async function checkSearchService() {
  // 검색 서비스 헬스 체크
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

## 모범 사례 및 보안

### 보안 헤더 구현

```javascript
// security-headers.js
const securityHeaders = {
  // 클릭재킹 방지
  'X-Frame-Options': 'DENY',

  // MIME 타입 스니핑 방지
  'X-Content-Type-Options': 'nosniff',

  // XSS 보호 활성화
  'X-XSS-Protection': '1; mode=block',

  // 리퍼러 정책
  'Referrer-Policy': 'strict-origin-when-cross-origin',

  // 콘텐츠 보안 정책
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

  // 권한 정책
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

### 배포 체크리스트

```markdown
## 배포 전 체크리스트

### 성능
- [ ] 빌드 크기가 예산 내에 있음 (< 350KB 초기 번들)
- [ ] 이미지가 최적화되고 압축됨
- [ ] Critical CSS가 인라인됨
- [ ] 오프라인 지원을 위한 서비스 워커 구현
- [ ] 정적 자산용 CDN 구성

### SEO
- [ ] 모든 페이지에 적절한 메타 태그 존재
- [ ] Sitemap.xml 생성 및 제출
- [ ] Robots.txt 구성
- [ ] Open Graph 메타 태그 구현
- [ ] Schema.org 구조화된 데이터 추가

### 보안
- [ ] 보안 헤더 구성
- [ ] HTTPS 강제 적용
- [ ] 콘텐츠 보안 정책 구현
- [ ] 클라이언트 사이드 코드에 민감한 데이터 노출되지 않음
- [ ] 의존성 보안 감사 통과

### 접근성
- [ ] WCAG 2.1 AA 준수 확인
- [ ] 스크린 리더 테스트 완료
- [ ] 키보드 내비게이션 테스트
- [ ] 색상 대비 비율이 표준을 충족
- [ ] 모든 이미지에 대체 텍스트 제공

### 모니터링
- [ ] 에러 추적 구성
- [ ] 성능 모니터링 활성화
- [ ] 헬스 체크 구현
- [ ] 가동 시간 모니터링 구성
- [ ] 분석 추적 확인

### 문서화
- [ ] 배포 문서 업데이트
- [ ] 일반적인 문제에 대한 런북 생성
- [ ] 지원 팀 연락처 정보
- [ ] 롤백 절차 문서화
```

BaiDocs 호스팅 옵션은 간단한 정적 호스팅부터 엔터프라이즈급 컨테이너 오케스트레이션까지 확장 가능한 유연한 배포 전략을 제공하여 전 세계적으로 최적의 성능과 안정성을 갖춘 문서 전달을 보장합니다.
