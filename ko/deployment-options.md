---
title: 배포 옵션
description: BaiDocs 문서를 다양한 플랫폼에 배포하기
---

# 배포 옵션

BaiDocs는 다양한 플랫폼에 배포할 수 있습니다.

## Vercel (권장)

Vercel은 Next.js 애플리케이션 배포에 가장 쉬운 옵션입니다.

### Vercel CLI로 배포

```bash
# Vercel CLI 설치
npm install -g vercel

# 배포
vercel

# 프로덕션 배포
vercel --prod
```

### GitHub 통합으로 배포

1. 코드를 GitHub에 푸시
2. [vercel.com](https://vercel.com) 방문
3. "Import Project" 클릭
4. 저장소 선택
5. 빌드 설정 구성:
   - **Framework**: Next.js
   - **Root Directory**: `apps/viewer`
   - **Build Command**: `pnpm build`

6. 환경 변수 추가:
   ```
   ENABLE_PDF_GENERATION=true
   ```

7. 배포!

## Docker

Docker 컨테이너를 사용하여 배포.

### Dockerfile

```dockerfile
FROM node:18-alpine

RUN npm install -g pnpm

WORKDIR /app
COPY . .

RUN pnpm install
RUN pnpm build

EXPOSE 3000
CMD ["node", ".next/standalone/apps/viewer/server.js"]
```

빌드 및 실행:

```bash
docker build -t baidocs .
docker run -p 3000:3000 baidocs
```

## 자체 호스팅

### PM2 사용

```bash
# PM2 설치
npm install -g pm2

# 애플리케이션 시작
cd .next/standalone/apps/viewer
pm2 start server.js --name baidocs

# PM2 설정 저장
pm2 startup
pm2 save
```

### Nginx 역방향 프록시

Nginx 설정:

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
    }
}
```

## SSL/HTTPS

### Let's Encrypt (무료)

Certbot 사용:

```bash
# Certbot 설치
sudo apt-get install certbot python3-certbot-nginx

# 인증서 획득
sudo certbot --nginx -d docs.example.com
```

## 배포 체크리스트

배포 전:

- [ ] 빌드가 성공적으로 완료
- [ ] 환경 변수 설정
- [ ] 사용자 정의 도메인 설정
- [ ] SSL 인증서 설치
- [ ] 모니터링 활성화
- [ ] 백업 계획 준비

## 결론

이제 BaiDocs를 배포하는 데 필요한 모든 지식을 갖추었습니다!

:::success
BaiDocs 가이드 완료를 축하합니다! 이제 놀라운 문서를 만들 준비가 되었습니다.
:::
