# TestWeb Ops Repository

이 리포지토리는 testweb 애플리케이션의 Kubernetes 매니페스트를 관리합니다.

## 구조

```
testweb-ops/
├── .github/
│   └── workflows/
│       └── update-manifests.yml  # 매니페스트 업데이트 워크플로우
├── k8s/                          # Kubernetes 매니페스트 파일들
│   ├── namespace.yaml
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
└── README.md
```

## GitHub Actions 설정

### 1. 워크플로우 권한 설정

리포지토리 설정에서 다음 권한을 활성화해야 합니다:
- Settings → Actions → General → Workflow permissions
- "Read and write permissions" 선택
- "Allow GitHub Actions to create and approve pull requests" 체크

### 2. 자동 머지 설정 (선택사항)

자동 머지를 원한다면:
- Settings → General → Pull Requests
- "Allow auto-merge" 활성화

## 워크플로우 동작

1. **이미지 업데이트 수신**: testweb-dev에서 이미지 푸시 완료 시 웹훅 수신
2. **매니페스트 업데이트**: deployment.yaml의 이미지 태그 자동 업데이트
3. **PR 생성**: 변경사항이 있으면 자동으로 Pull Request 생성
4. **자동 머지**: 설정에 따라 자동으로 머지 (권장하지 않음)

## 수동 배포

수동으로 특정 이미지로 배포하려면:

```bash
# Actions 탭에서 "Update Kubernetes Manifests" 워크플로우 수동 실행
# 입력값:
# - image: ghcr.io/USERNAME/testweb-dev
# - tag: v1.0.0
```

## Kubernetes 배포

```bash
# 모든 매니페스트 적용
kubectl apply -f k8s/

# 특정 리소스만 적용
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

## 모니터링

```bash
# 파드 상태 확인
kubectl get pods -n testweb

# 서비스 확인
kubectl get svc -n testweb

# 인그레스 확인
kubectl get ingress -n testweb
```
