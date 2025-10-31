# Kubernetes Manifests

## 파일 구조

```
k8s/
├── backend/
│   └── backend-api.yaml    # 백엔드 API 서버 배포 설정
├── frontend/
│   ├── namespace.yaml      # 프론트엔드 네임스페이스
│   ├── deployment.yaml      # 프론트엔드 Deployment
│   ├── service.yaml        # 프론트엔드 Service
│   └── ingress.yaml        # 프론트엔드 Ingress
├── secrets.yaml            # 민감한 정보 (Git에 올리지 않음)
└── secrets.yaml.example    # Secret 파일 예시 템플릿
```

## Secret 설정 방법

### 1. secrets.yaml.example을 복사

```bash
cp k8s/secrets.yaml.example k8s/secrets.yaml
```

### 2. 실제 값으로 base64 인코딩하여 입력

```bash
# 예시: Access Key ID 인코딩
echo -n "AKIAUJDY7FD7XJ6WEQWM" | base64

# 예시: Secret Key 인코딩
echo -n "your-secret-key" | base64

# 예시: Region 인코딩
echo -n "ap-northeast-2" | base64

# 예시: Bucket Name 인코딩
echo -n "hyjk826-mlops-1011" | base64

# 예시: CORS Origins 인코딩
echo -n "http://localhost:8080" | base64
```

### 3. secrets.yaml 파일 편집

```yaml
data:
  AWS_ACCESS_KEY_ID: <위에서 생성한 base64 값>
  AWS_SECRET_ACCESS_KEY: <위에서 생성한 base64 값>
  AWS_REGION: <위에서 생성한 base64 값>
  S3_BUCKET_NAME: <위에서 생성한 base64 값>
  CORS_ORIGINS: <위에서 생성한 base64 값>
```

## 배포 순서

### 1. Namespace 및 Secret 배포

```bash
kubectl apply -f k8s/secrets.yaml
```

### 2. 백엔드 API 배포

```bash
kubectl apply -f k8s/backend/backend-api.yaml
```

### 3. 프론트엔드 배포

```bash
kubectl apply -f k8s/frontend/
```

## 주의사항

⚠️ **`secrets.yaml` 파일은 절대 Git에 커밋하지 마세요!**

- `.gitignore`에 이미 추가되어 있습니다
- 실제 자격 증명이 포함되어 있으므로 로컬에서만 관리하세요
- GitHub Actions나 CI/CD에서는 Kubernetes Secret을 직접 생성하거나 GitHub Secrets를 사용하세요

