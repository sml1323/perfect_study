# 🚀 Prefect 완전정복 학습 계획서

완전한 Prefect 레시피북 작성을 위한 체계적 학습 로드맵입니다.

## 📋 학습 목표
✅ **PostgreSQL DB 연결 및 쿼리**  
✅ **API POST 요청 처리**  
✅ **웹훅 URL 트리거 자동화**  
✅ **실무 레시피북 작성**  
✅ **진행상황 추적 시스템**

## 🗓 5단계 학습 로드맵 (5주 완성)

### 📚 Phase 1: 환경설정 & Prefect 기초 (1주차)
**목표**: Prefect 개발환경 구축 및 PostgreSQL 백엔드 설정

**학습 과제**:
- [ ] Python 3.8+ 가상환경 설치 및 Prefect 설치
- [ ] PostgreSQL 로컬 설치 또는 Docker 컨테이너 설정
- [ ] Prefect Cloud 계정 생성 및 API 키 설정
- [ ] 첫 번째 'Hello World' Prefect 플로우 생성 및 실행
- [ ] Prefect PostgreSQL 백엔드 데이터베이스 설정

**핵심 기술스택**:
```bash
# 필수 설치 패키지
pip install prefect prefect-sqlalchemy psycopg2-binary python-dotenv
```

**PostgreSQL 연결 설정**:
```bash
prefect config set PREFECT_API_DATABASE_CONNECTION_URL="postgresql+asyncpg://postgres:password@localhost:5432/prefect"
```

### 🗄️ Phase 2: PostgreSQL 통합 (2주차)  
**목표**: Database CRUD 작업 및 ETL 파이프라인 구축

**학습 과제**:
- [ ] prefect-sqlalchemy 설치 및 첫 번째 SqlAlchemyConnector 블록 생성
- [ ] 기본 CRUD 작업이 포함된 Prefect 플로우 구축 (CREATE, READ, UPDATE, DELETE)
- [ ] CSV 데이터를 읽어 PostgreSQL에 로드하는 ETL 파이프라인 생성
- [ ] 동기 및 비동기 데이터베이스 쿼리 패턴 구현

**핵심 패턴**:
- SqlAlchemyConnector 블록 생성
- Sync/Async 쿼리 패턴
- 배치 처리 및 트랜잭션 관리

**예제 코드 패턴**:
```python
from prefect_sqlalchemy import SqlAlchemyConnector

# 블록 생성
connector = SqlAlchemyConnector(
    connection_info=ConnectionComponents(
        driver=SyncDriver.POSTGRESQL_PSYCOPG2,
        username="postgres",
        password="password",
        host="localhost",
        port=5432,
        database="prefect_db",
    )
)
connector.save("postgres-connector")
```

### 🌐 Phase 3: API 통합 (3주차)
**목표**: HTTP 요청 처리 및 외부 API 연동

**학습 과제**:
- [ ] 외부 API(날씨/뉴스 API)에 HTTP GET 요청하는 플로우 생성
- [ ] 데이터 페이로드로 HTTP POST 요청을 보내는 플로우 구축
- [ ] API 요청에 대한 에러 핸들링, 재시도, 속도 제한 구현

**핵심 기능**:
- GET/POST 요청 처리
- 에러 핸들링 & 재시도 로직
- Rate limiting & 인증

**예제 API 패턴**:
```python
import httpx
from prefect import flow, task

@task(retries=3)
async def make_api_request(url: str, data: dict):
    async with httpx.AsyncClient() as client:
        response = await client.post(url, json=data)
        response.raise_for_status()
        return response.json()
```

### ⚡ Phase 4: 웹훅 자동화 (4주차)
**목표**: 이벤트 기반 자동화 및 웹훅 트리거

**학습 과제**:
- [ ] 정적 템플릿으로 첫 번째 Prefect Cloud 웹훅 설정
- [ ] 요청 본문 데이터를 사용하는 Jinja2 템플릿으로 동적 웹훅 생성
- [ ] 웹훅 URL을 통해 Prefect 플로우를 트리거하는 자동화 구축
- [ ] 웹훅 보안, 유효성 검사, 에러 핸들링 테스트

**핵심 요소**:
- Prefect Cloud 웹훅 설정
- Jinja2 동적 템플릿
- 자동화 트리거 & 보안 검증

**웹훅 템플릿 예제**:
```json
{
    "event": "model-update",
    "resource": {
        "prefect.resource.id": "product.models.{{ body.model_id }}",
        "prefect.resource.name": "{{ body.friendly_name }}",
        "run_count": "{{ body.run_count }}"
    }
}
```

### 📖 Phase 5: 레시피북 작성 (5주차)
**목표**: 완전한 문서화 및 재사용 가능한 템플릿 생성

**학습 과제**:
- [ ] 레시피북 디렉토리 구조 및 README 템플릿 생성
- [ ] 레시피북에 모든 PostgreSQL 연결 및 쿼리 패턴 문서화
- [ ] API 요청 패턴, 에러 핸들링, 모범 사례 문서화
- [ ] 웹훅 설정, 템플릿, 자동화 패턴 문서화
- [ ] 일반적인 Prefect + PostgreSQL + API 패턴에 대한 재사용 가능한 코드 템플릿 생성
- [ ] 일반적인 문제 및 해결책이 포함된 종합적인 문제해결 가이드 작성
- [ ] 모든 레시피북 예제를 검증하는 테스팅 프레임워크 구축
- [ ] 모든 개념을 결합한 종합 예제 생성 (PostgreSQL + API + 웹훅)

**레시피북 구조**:
```
prefect-recipe-book/
├── README.md                 # 개요 및 인덱스
├── setup/                   # 환경설정 가이드
│   ├── environment-setup.md
│   ├── postgres-setup.md
│   └── prefect-config.md
├── database/               # PostgreSQL 패턴
│   ├── connection-patterns.md
│   ├── crud-operations.md
│   ├── query-patterns.md
│   └── best-practices.md
├── api/                   # API 통합 패턴  
│   ├── http-requests.md
│   ├── error-handling.md
│   ├── rate-limiting.md
│   └── authentication.md
├── webhooks/             # 웹훅 자동화
│   ├── webhook-setup.md
│   ├── jinja-templates.md
│   ├── automation-triggers.md
│   └── security.md
├── examples/            # 실무 예제
│   ├── etl-pipeline/
│   ├── api-integration/
│   ├── event-driven/
│   └── monitoring/
└── troubleshooting/     # 문제해결 가이드
    ├── common-issues.md
    ├── debugging-guide.md
    └── performance-tips.md
```

## ⚙️ 개발 환경 요구사항

### 필수 도구
- **Python 3.8+** with virtual environment
- **PostgreSQL** (local/Docker)
- **Prefect Cloud** 계정 (무료 티어)
- **VS Code** + Python extension
- **Git** (버전 관리용)

### 핵심 라이브러리
```bash
pip install prefect prefect-sqlalchemy asyncpg httpx python-dotenv
```

### 선택 사항
- **Docker** (PostgreSQL 컨테이너용)
- **ngrok** (웹훅 테스트용)

## 🎯 즉시 시작 가이드

### 1단계: 환경 설정
```bash
# 가상환경 생성
python -m venv prefect-env
source prefect-env/bin/activate  # Linux/Mac
# 또는 Windows의 경우: prefect-env\Scripts\activate

# Prefect 설치
pip install prefect prefect-sqlalchemy asyncpg

# Prefect 초기화
prefect init
```

### 2단계: PostgreSQL 설정
```bash
# Docker로 PostgreSQL 실행
docker run -d --name prefect-postgres \
  -e POSTGRES_PASSWORD=prefect123 \
  -e POSTGRES_DB=prefect \
  -p 5432:5432 postgres:15

# 또는 로컬 PostgreSQL 설치 후 데이터베이스 생성
createdb prefect_db
```

### 3단계: Prefect 설정
```bash
# Prefect Cloud 로그인
prefect cloud login

# PostgreSQL 백엔드 설정
prefect config set PREFECT_API_DATABASE_CONNECTION_URL="postgresql+asyncpg://postgres:prefect123@localhost:5432/prefect"
```

### 4단계: 첫 번째 플로우 테스트
```python
from prefect import flow, task

@task
def say_hello(name: str):
    print(f"Hello, {name}!")
    return f"Hello, {name}!"

@flow
def hello_world_flow(name: str = "Prefect"):
    message = say_hello(name)
    return message

if __name__ == "__main__":
    hello_world_flow()
```

## 📊 진행상황 추적

### 체크리스트 시스템
각 단계별로 체크리스트를 사용하여 진행상황을 추적합니다.

### 성공 지표
- ✅ 각 단계별 실습 완료
- ✅ PostgreSQL 통합 패턴 마스터  
- ✅ API 통합 및 에러 핸들링
- ✅ 웹훅 자동화 구현
- ✅ 완전한 레시피북 작성

### 학습 시간 계획
**집중 학습**: 2-3주 (일 3-4시간)
- 1주차: Phase 1-2 완료
- 2주차: Phase 3-4 완료  
- 3주차: Phase 5 완료

**점진 학습**: 5주 (일 1-2시간)
- 각 단계별로 1주씩 할당

## 💡 학습 팁

### 효과적인 학습 방법
1. **실습 중심**: 이론보다 실제 코드 작성에 집중
2. **점진적 구축**: 각 단계가 이전 단계를 기반으로 구축
3. **실시간 문서화**: 학습과 동시에 문서 작성
4. **실제 예제**: 장난감 예제가 아닌 실무 예제 사용
5. **버전 관리**: 모든 코드 예제에 Git 사용

### 문제 해결 접근법
1. **체계적 디버깅**: 로그와 에러 메시지 분석
2. **공식 문서 활용**: Prefect 공식 문서 참조
3. **커뮤니티 활용**: Prefect Slack 채널 및 GitHub Issues
4. **테스트 우선**: 각 기능에 대한 테스트 케이스 작성

## 🎉 완료 후 혜택

이 학습 계획을 완료하면:
- **Production-ready** Prefect 워크플로우 구축 능력
- **PostgreSQL 통합** 전문 지식
- **API 통합 및 에러 핸들링** 마스터
- **이벤트 기반 자동화** 구현 능력
- **완전한 레시피북** 보유로 향후 프로젝트에서 즉시 활용 가능

---

**시작일**: ___________  
**목표 완료일**: ___________  
**현재 진행 단계**: ___________

이제 첫 번째 과제인 **환경 설정**부터 시작하세요! 🚀