# 문제 해결 가이드

## 이미지 목록 불러오기 실패 문제

### 1. Pod 상태 확인

```bash
# 백엔드 Pod 확인
kubectl get pods -n image-upload

# Pod 로그 확인
kubectl logs -n image-upload -l app=backend-api --tail=50

# 프론트엔드 Pod 확인
kubectl get pods -n web
```

### 2. Service 확인

```bash
# 백엔드 Service 확인
kubectl get svc -n image-upload

# 프론트엔드 Service 확인
kubectl get svc -n web
```

### 3. Ingress 확인

```bash
# Ingress 상태 확인
kubectl get ingress -n image-upload

# Ingress 상세 정보
kubectl describe ingress backend-api-ingress -n image-upload
```

### 4. API 직접 테스트

```bash
# Pod 내부에서 테스트
kubectl exec -it -n image-upload deployment/backend-api -- curl http://localhost:8000/api/health

# Service로 테스트 (같은 네임스페이스)
kubectl run curl-test --image=curlimages/curl --rm -it --restart=Never -n image-upload -- curl http://backend-api-service:8000/api/health
```

### 5. CORS 확인

브라우저 개발자 도구 (F12) → Console/Network 탭에서 CORS 오류 확인

