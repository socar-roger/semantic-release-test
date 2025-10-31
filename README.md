# Semantic Release 테스트 프로젝트

이 프로젝트는 Semantic Release 자동 버저닝을 테스트하기 위한 간단한 프로젝트입니다.


## 🚀 빠른 시작

### 1. 의존성 설치
```bash
cd /Users/jangdongha/Documents/semantic-release-test
yarn install
```

### 2. Git 저장소 초기화
```bash
git init
git add .
git commit -m "feat: 초기 프로젝트 설정"
```

### 3. GitHub 저장소 연결
```bash
# GitHub에서 새 저장소 생성 후
git remote add origin https://github.com/username/semantic-release-test.git
git branch -M main
git push -u origin main
```

### 4. 브랜치 생성
```bash
# dev 브랜치 생성
git checkout -b dev
git push -u origin dev

# prod 브랜치 생성  
git checkout -b prod
git push -u origin prod
```

## 🧪 테스트 방법

### 로컬 테스트 (드라이런)
```bash
# 실제 릴리즈 없이 테스트
yarn release:dry
```

### dev 브랜치 테스트
```bash
git checkout dev
echo "console.log('새 기능 추가');" > feature.js
git add .
git commit -m "feat: 새로운 기능 추가"
git push origin dev
# → v1.1.0-dev.1 태그 생성 예상
```

### prod 브랜치 테스트
```bash
git checkout prod
git merge dev
git push origin prod
# → v1.1.0 정식 태그 생성 예상
```

## 📋 커밋 메시지 규칙

- `feat:` - 새로운 기능 (Minor 버전 증가)
- `fix:` - 버그 수정 (Patch 버전 증가)
- `feat!:` 또는 `BREAKING CHANGE:` - 호환성 깨지는 변경 (Major 버전 증가)
- `docs:` - 문서 변경
- `style:` - 코드 포맷팅
- `refactor:` - 리팩토링
- `test:` - 테스트 추가/수정
- `chore:` - 빌드 프로세스 또는 보조 도구 변경

## 🏷️ 예상 태그 형태

| 브랜치 | 태그 형태 | 예시 |
|--------|-----------|------|
| **prod** | `v{version}` | `v1.1.0`, `v1.2.0` |
| **dev** | `v{version}-dev.{number}` | `v1.1.0-dev.1`, `v1.1.0-dev.2` |

## 📁 프로젝트 구조

```
semantic-release-test/
├── .github/
│   └── workflows/
│       └── release.yaml      # GitHub Actions 워크플로우
├── .releaserc.json          # Semantic Release 설정
├── package.json             # 프로젝트 설정
├── CHANGELOG.md             # 자동 생성되는 변경 로그
└── README.md               # 이 파일
```

## 🔧 주요 설정

### Semantic Release 브랜치 설정
- **prod**: 정식 릴리즈 브랜치
- **dev**: 개발 프리릴리즈 브랜치 (`-dev.x` 접미사)

### 자동 업데이트 파일
- `package.json` - 버전 필드
- `CHANGELOG.md` - 변경 로그

### GitHub Actions 트리거
- `dev`, `prod` 브랜치에 push 시 자동 실행
- `.github/**` 경로 변경은 제외

## 🎯 테스트 시나리오

1. **첫 번째 기능 추가**
   ```bash
   git commit -m "feat: 사용자 로그인 기능 추가"
   # → v1.1.0-dev.1 (dev), v1.1.0 (prod)
   ```

2. **버그 수정**
   ```bash
   git commit -m "fix: 로그인 오류 수정"
   # → v1.1.1-dev.1 (dev), v1.1.1 (prod)
   ```

3. **호환성 깨지는 변경**
   ```bash
   git commit -m "feat!: API 응답 형식 변경"
   # → v2.0.0-dev.1 (dev), v2.0.0 (prod)
   ```

## 📝 참고사항

- 이 프로젝트는 실제 npm 패키지를 배포하지 않습니다 (`npmPublish: false`)
- GitHub 저장소가 연결되어야 정상 작동합니다
- `GITHUB_TOKEN` 권한이 필요합니다 (GitHub Actions에서 자동 제공)
