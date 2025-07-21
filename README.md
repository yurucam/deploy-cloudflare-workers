# Deploy Cloudflare Workers Action

Cloudflare Workers에 Node.js 프로젝트를 자동으로 빌드하고 배포하는 GitHub Action입니다.

## 기능

- Node.js 환경 자동 설정 (`.nvmrc` 파일 기반)
- 의존성 캐싱으로 빌드 시간 단축
- Cloudflare Workers 자동 배포
- 환경별 배포 지원

## 사용법

```yaml
name: Deploy to Cloudflare Workers

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Deploy to Cloudflare Workers
        uses: your-username/deploy-cloudflare-workers@main
        with:
          working-directory: './my-worker'
          account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          environment: 'production'
```

## 입력 매개변수

| 매개변수 | 필수 | 타입 | 설명 |
|---------|------|------|------|
| `working-directory` | ✅ | string | 프로젝트의 작업 디렉토리 경로 |
| `account-id` | ✅ | string | Cloudflare 계정 ID |
| `api-token` | ✅ | string | Cloudflare API 토큰 |
| `environment` | ❌ | string | 배포할 환경 (예: production, staging). 지정하지 않으면 기본 환경으로 배포됩니다. |

## 필수 조건

1. **Node.js 프로젝트**: `package.json`과 `package-lock.json` 파일이 있어야 합니다.
2. **Node 버전 파일**: 작업 디렉토리에 `.nvmrc` 파일이 있어야 합니다.
3. **Wrangler 설정**: `wrangler.toml` 파일이 프로젝트에 구성되어 있어야 합니다.
4. **Cloudflare 설정**:
   - Cloudflare 계정 ID
   - 적절한 권한을 가진 API 토큰

## Secrets 설정

GitHub 저장소의 Settings > Secrets and variables > Actions에서 다음 secrets을 설정하세요:

- `CLOUDFLARE_ACCOUNT_ID`: Cloudflare 대시보드에서 확인할 수 있는 계정 ID
- `CLOUDFLARE_API_TOKEN`: Workers 배포 권한이 있는 API 토큰

## 예제

### 기본 사용법

```yaml
- name: Deploy to Cloudflare Workers
  uses: your-username/deploy-cloudflare-workers@main
  with:
    working-directory: '.'
    account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
    api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
    environment: 'production'
```

### 서브디렉토리 프로젝트

```yaml
- name: Deploy Worker from subdirectory
  uses: your-username/deploy-cloudflare-workers@main
  with:
    working-directory: './apps/my-worker'
    account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
    api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
    environment: 'staging'
```

### 기본 환경으로 배포 (environment 생략)

```yaml
- name: Deploy to default environment
  uses: your-username/deploy-cloudflare-workers@main
  with:
    working-directory: '.'
    account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
    api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
```

## 작동 방식

1. **Node.js 설정**: `.nvmrc` 파일을 기반으로 Node.js 환경을 설정합니다.
2. **의존성 캐싱**: `package-lock.json`을 기반으로 `node_modules`를 캐싱합니다.
3. **의존성 설치**: 캐시가 없는 경우 `npm ci`로 의존성을 설치합니다.
4. **배포**: Cloudflare Wrangler를 사용하여 지정된 환경에 배포합니다.

## 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.

## 기여

버그 리포트, 기능 요청, 풀 리퀘스트를 환영합니다. 기여하기 전에 이슈를 먼저 열어 논의해 주세요.
