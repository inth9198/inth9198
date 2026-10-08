![header](https://capsule-render.vercel.app/api?type=wave&color=auto&height=300&section=header&text=minho%20lee&fontSize=90)

# 진화하는 개발자 이민호입니다🐜      
<br/> <br/> <br/> 



# 약력
 - 2026/03 ~ 현재 : 신한DS 신한자산신탁 AX 플랫폼 구축 (풀스택 · LLM 애플리케이션 · DevOps)
 - 2025/01 ~ 2026/02 : 이카운트 프론트엔드 및 데브옵스
 - 2025/06 ~ 현재 : LLM 운용 애플리캐이션 개발자 과정
 - 2022/10 ~ 2024/11 : 티맥스 클라우드 프론트엔드 연구원
 - 2022/08 ~ 2022/10 : 4차산업기술인재 - Web 1기
 - 2022/03 ~ 2022/06 : (주)클래스 액트 인턴
 - 2021/11 ~ 2022/09 : 42서울 카뎃
 - 2021/03 ~ 2022/02 : 세종대학교 소프트웨어공학과 재학(복수전공)
 - 2017/03 ~ 2022/02 : 세종대학교 바이오융합공학과 재학


# 신한DS · 신한자산신탁 AX 플랫폼 (2026/03 ~ 현재)

신한자산신탁 업무를 AI로 전환하는 사내 플랫폼 구축 프로젝트입니다. 업무 포털, LLM 워크플로, 데이터 수집·저장 서비스로 구성되며 AWS 개발계와 폐쇄망 운영계에 함께 배포합니다.

업무 포털·문서 저장소·뉴스 수집·외부 API 중계·STT 회의록 등 5개 저장소의 초기 구성을 맡았습니다. 문서 접수·업무 오케스트레이션·문서양식 모듈을 담당하면서 LLM 워크플로와 인프라·배포 체계도 함께 만들었습니다.

**기술 스택**
- Backend: Java 21, Spring Boot 3.5 (Web/WebFlux/Security), MyBatis, PostgreSQL 17 (pg_bigm), Flyway, MinIO
- Frontend: React 18, TypeScript, Vite, Tailwind CSS, Zustand, TanStack Query, Storybook, Playwright
- LLM: Dify (Workflow/Chatflow/Plugin), Qwen3.5 (vLLM), 비전 LLM OCR, RAG (BM25·RRF·Reranker)
- Infra: AWS (EC2·VPC·S3·EventBridge·Lambda·GuardDuty·Site-to-Site VPN), Docker Compose, Nginx, Gitea Actions, Nexus, SonarQube, Vault Transit, Prometheus·Grafana·Tempo·OpenTelemetry, OpenSearch·Fluent Bit

## LLM 문서 판독 · 값 추출 파이프라인 (Dify)

- 비전 LLM 판독에 페이지별 검증 게이트 설계 (입력 토큰으로 이미지 미전달 감지 → 재판독 → OCR 폴백)
  → 같은 조건 각 10회 실측에서 재현율 0.707 → 0.9995, 환각이 결과로 나간 횟수 3/10 → 0/10
- 판독 도구를 Dify 워크플로 도구로 공통화하고, 실패 시 OCR 폴백을 파일 단위에서 페이지 단위로 변경 → 폴백 시간 60.1초 → 14.6초
- 자금집행요청서 → ERP 값 추출 워크플로 고도화: 실물 문서로 정답지를 직접 만들어 절대 정확도로 평가 → 정확도 53% → 93~95%
- 매핑 노드 지연의 원인이 모델 사고과정(thinking) 출력임을 노드 로그로 규명하고 no-reasoning 설정을 표준화 → 노드 41.8초 → 2.4~5.3초, 출력 토큰 5,763 → 42~537
- Dify 병렬 분기가 실제로는 순차 실행된다는 것을 노드 타임라인으로 밝혀내고 반복(iteration) 노드 기반 병렬로 재구성
- LLM 없는 결정적 검증 노드 (출처 대조·합계 검산·형식 검사)를 붙여 의심 값에 검토 표시
- 문서 비교 에이전트: 숫자·줄 단위 대조는 코드가 결정적으로, LLM은 해석만 맡도록 분리 → 문장 차이 검출 1건 → 4건, 좌우 대조 화면 제공
- 표준양식 생성기: 하드코딩된 6종 양식을 스키마 주입형 범용 워크플로(양식 정의 / 값 채우기)로 대체해 새 양식 추가를 개발 과제에서 담당자 화면 등록으로 전환, 판독 교체로 채우기 54.7초 → 21.7초
- 문서 렌더러·표 크롭·엑셀 그리드 등 Dify 플러그인 직접 개발

## 폐쇄망 양자화 모델 대응

- 내부망 추론 처리량(프리필 약 5,000 / 디코드 약 61 tok/s)을 바탕으로 병목이 출력(디코드)임을 정리하고, 노드 시간 추정식을 속도 판단 기준으로 문서화
- 실제 호출 563건 회귀분석으로 호출당 고정비를 포함한 시간 모델 수립 (R² = 0.75)
- 양자화 모델이 전사는 하지만 "필요한 줄만 골라 전사"는 못 한다는 것을 실측으로 확정 → 배포처별 판독 경로 분리 (클라우드 = 공통 판독 도구, 폐쇄망 = 자체 판독)
- 표 전사 포맷을 HTML에서 한 줄 파이프 구분으로 바꿔 출력 토큰 절감 → 폐쇄망 예상 처리시간 16.2초 → 9.9초 (−39%), 정확도 96~98%
- 클라우드 ↔ 폐쇄망 환경 차이(모델명·도구 ID) 치환을 한 모듈로 모으고 잔존 시 빌드 중단 → 폐쇄망 반입판 10종 재생성

## AI 업무비서 · 업무 오케스트레이션

- 다단계 업무 자동화 엔진 구현 (진행정의 CRUD, 단계별 에이전트 실행, 승인·날인·지연 알람, 단계 재시작) → 담보신탁 7단계 PoC를 범용 엔진으로 일반화
- 대화형 AI 업무비서: Dify 마스터 챗플로가 첨부 판독·작업현황·내규 검색·일반 대화로 라우팅, SSE 응답에 답변 에이전트 표시, 실행 단계를 대화 카드로 투영 (목록·구성 화면은 팀 공동)
- 단계마다 AI가 제안하고 사람이 확정하는 HITL 카드 (양식 채우기·날인·승인 대기)
- 단일 분기 구조를 "능력 게이트" 직렬 구조로 개편해 한 턴에서 여러 능력이 동시에 동작 (노드 23 → 15)
- 대화로 받은 값은 서버 저장이 확정돼야 다음 턴에 실리도록 해 "기록했습니다" 류 환각 차단, 대화 이력 창·근거 글자 수 상한 설계
- 그래프 지식베이스와 법률 코퍼스, 두 검색 경로의 근거 병합: 점수 종류(BM25/RRF/리랭크) 자동 감지, 경로별 "근거 없음" 판정, 출처 인용
- 도장찍기 에이전트: PDF 텍스트 좌표 우선, 스캔본은 비전 LLM 좌표로 폴백 → 정답 좌표 대비 오차 x 0.1mm / y 4.9mm
- 등기우편·메일·문자 발송 에이전트 구현, 우편 벤더 신규 규격으로 4개 서비스 일괄 교체

## 문서 접수 · 자동분류 (모듈 담당)

- 접수함을 "문서 목록"에서 "업무 단위 + LLM 판독 결과를 사람이 확정(HITL)" 구조로 전면 재설계
- 메일 봉투(.eml · .zip · 전달메일 · 첨부 속 zip)를 첨부 단위로 펼치고 본문을 PDF로 만들어 함께 접수, 외부 메일 수집기용 수신 API 구현 (cp949 zip 문제 해결)
- 묶음으로 들어온 문서는 문서별 근거 강도로 업무분류를 가중 투표 (LLM 재호출 없음) → 실물 25묶음 25/25 일치, 빈칸 45 → 0
- 분류 키워드를 분류 간 겹침이 없도록 재설계하고 결정적 판정에 LLM 판독 폴백 → 실물 회귀 17/25 → 24/25
- 생성 문서에 HMAC 서명 QR 삽입, 회신본이 접수되면 원래 업무 흐름을 자동 재개 (OCR 대조 대신 결정적 매칭)
- LLM 호출 중 DB 트랜잭션 점유 제거, zip 폭탄 방어, 멈춘 판독 재처리 스케줄러 → 동시 판독 시 idle-in-transaction 커넥션 10/10 → 0

## 업무 포털 프론트엔드 · 백엔드 표준

- 포털 프로젝트 초기 구성, CRA → Vite 전환 및 모노레포 통합
- 설정(config) 기반 화면 렌더러 page-engine 설계·도입 → 87개 페이지 중 86개가 같은 엔진으로 동작 (이후 확장은 팀 공동)
- 디자인 토큰·공통 컴포넌트·Storybook 디자인시스템, 백엔드 없이 띄우는 미리보기 모드와 스크린샷 검증 도구
- 메뉴·화면 정의를 JSON 정본으로 옮겨 기동 시 DB와 동기화 (report/apply 모드, CI lint)
- PostgreSQL pg_bigm 한글 부분검색 도입: 검색 매퍼 18개에 bigram GIN 인덱스, ILIKE → LOWER LIKE 재작성 → 4,215ms 순차 스캔 → 0.24ms 인덱스 스캔, 0건이면 유사도 기반 오타 폴백
- Flyway 마이그레이션 358개를 데이터 보존형 baseline으로 압축, 복제본 리허설 후 컷오버 → 행 수·스키마 diff 0
- REST POST-only 규칙 수립 및 전환, 조회/거래 애노테이션 분리로 행위로그 오염 방지
- 문서 본문 편집기와 HTML → DOCX 렌더러 구현 (표 반복 구간 유실·이미지 비율·테두리 유실 근본 수정)
- Playwright 실모드 E2E 230여 개 작성, 화면 79개를 위험 기반으로 등급화한 QA 원장 운영

## 데이터 수집 · 저장 서비스

- 뉴스 수집 서비스 초기 구축: 경쟁사 watchlist, Readability 방식 본문 추출기 (v2 → v5), 저작권 문구 제거·매체 차단 정책
- 같은 사건 기사 묶기: 제목 15자·SimHash·임베딩을 실데이터로 비교한 뒤 제목 토큰 자카드 + 발행일 ±7일 방식 채택 → 1,892건 기준 중복 23.8% 제거 (15자 방식 7.8%), 외부 호출이 없어 폐쇄망에서도 동일하게 동작
- 외부 공공 API 중계 서버 (Spring WebFlux) 구축: 주소·실거래가·전자공시 등, 수집 서버의 외부 직접 호출을 중계 경유로 일원화
- 문서 저장소 (MinIO + PostgreSQL) 초기 구축, 보존 정책·PITR 백업, 72시간 지난 문서를 오브젝트 스토리지로 옮기는 계층형 저장 (첫 회차 200건 실패 0, SHA-256 일치)
- 시공사 리스크 레이더 (Cytoscape 마인드맵), LLM 일배치 경쟁사 동향 리포트, 자연어를 검색조건으로 바꾸는 뉴스 검색
- STT 회의록 PWA (팀 공동): Google OAuth, JWT HttpOnly 쿠키 세션, 녹음 중 청크 스트리밍 업로드, Wake Lock, 개인정보 감사로그·마스킹·자동삭제

## 인프라 · DevOps (AWS 개발계 + 폐쇄망 운영계)

- AWS 개발계 구축: Gitea·Nexus·SonarQube 등 개발도구를 Docker Compose 단일 스택으로 구성, Apache Guacamole 웹 접속 (개인키 없이 계정 권한으로 접속)
- 개발계를 폐쇄망과 같은 다중 호스트 구조로 분리해 "클라우드에선 되는데 폐쇄망에선 안 되는" 반복 사고 원인 제거
- Gitea Actions + Nexus CI/CD, 헬스 실패 시 자동 롤백 → 러너 리소스 경합을 규명하고 레지스트리 pull로 전환해 배포 12분 57초 → 2분 56초
- blue/green 무중단 배포: 백엔드 2슬롯 + nginx upstream 전환, 정적자산 원자 교체, advisory lock 리더 선출로 배치 29종 중복 실행 차단 → 배포 1회 3분 6초 단절 → 연속 단절 0
- 폐쇄망 이관 체계: 릴리즈 이미지 번들, git bundle 단방향 미러, Nexus 오프라인 시드, 내부 CI 이식, 형상관리 경유 운영 배포 후처리 스크립트 (대상망 가드 → 복호화 → 잔류 검사 → DB 백업 → 헬스체크)
- 시크릿 관리 SOPS(age) → Vault Transit(KMS) 전환, 기동 시 복호화하고 설정 누락이나 다른 망 암호문이면 기동 중단
- 관측: Prometheus·Grafana·Tempo + OpenTelemetry 분산 트레이싱, OpenSearch + Fluent Bit 로그·알림, CloudTrail·GuardDuty → EventBridge → Lambda → Slack 보안 알림
- 장애 진단: EBS gp2 버스트 고갈 (gp3 온라인 전환), 컨테이너 메모리 한도 초과로 인한 호스트 OOM, 모니터링 수집기의 CPU 상시 점유 (코어 0.540 → 0.006, 인스턴스 상향 없이 해결)
- 비용: EventBridge Scheduler 업무시간 자동 기동/정지 (주 168h → 75h), DLM 일일 스냅샷, 빌드 서버 디스크 92% → 55%
- 보안: 그룹 클라우드 보안 체크리스트 95항목 실측 대조 및 감사 제출자료 준비, SonarQube 보안 룰 (KISA 보안약점 대응·개인정보 식별자 커스텀 룰)
- 운영 DB 이중화 설계 (PostgreSQL 17 + Patroni + etcd), 외부 연동 IPSec Site-to-Site VPN의 AWS 측 구성

## AI 개발 하네스 (Claude Code)

- Claude Code 훅·스킬·서브에이전트로 팀 개발 하네스 구축: 클라우드 컨트롤플레인 직접 호출 차단, 인프라 변경 후 문서화 누락 시 차단, 실행 증거로만 통과시키는 리뷰어 에이전트 분리
- 팀 컨벤션 스킬 (화면 엔진·화면 이관·실모드 E2E·백엔드/프론트엔드 컨벤션) 최초 작성, 재발 방지 교훈 180여 개 체계 운영

<br/>

## OpenReplay 셀프호스팅 및 통합 프로젝트

**프로젝트 개요**
- OpenReplay(오픈소스 세션 리플레이 도구)를 활용한 사용자 행동 분석 시스템 구축
- ECOUNT 웹 애플리케이션과의 통합 및 커스터마이징

**주요 작업 내용**

1. **셀프호스팅 환경 구축**
   - Docker Compose 기반 OpenReplay 로컬 서버 구축
   - PostgreSQL, ClickHouse, Redis, MinIO 등 멀티 컨테이너 환경 구성

2. **커스텀 통합 스크립트 개발**
   - ECOUNT 웹앱과 OpenReplay 연동을 위한 JavaScript 통합 스크립트 개발
   - 외부 CSS 파일 자동 인라인 처리 기능 구현 (세션 리플레이 시 스타일 보존)
   - Fetch API 활용한 비동기 CSS 다운로드 및 변환

3. **Nginx 리버스 프록시 설정**
   - CORS 정책 처리 및 외부 리소스 프록시 설정
   - 외부 도메인(zeus03.ecount.com, test.ecount.com) 리소스 프록시 구성
   - URL 인코딩 처리 및 SSL 연결 설정

4. **개발 환경 자동화**
   - 단일 사용자 인증 우회 설정 자동화 스크립트 작성
   - 빠른 시작을 위한 가이드 문서 작성

**기술 스택**
- Docker, Docker Compose
- Nginx (리버스 프록시, CORS 설정)
- JavaScript (Vanilla JS)
- Linux Shell Script

**성과**
- 외부 SaaS 대신 자체 호스팅으로 데이터 보안 강화
- 커스텀 스크립트로 CSS 미적용 문제 해결
- 개발 환경 설정 자동화로 초기 세팅 시간 단축

관련세미나 발표자료

https://inth9198.github.io/sessionreplay/


       
# 진행한 프론트엔드 프로젝트  

# ecount ERP
## AI 응답 파싱 서비스 설계 및 구현

- Server-Sent Events(SSE) 기반 AI 응답 파싱 서비스 설계 및 구현

- Markdown 형식의 실시간 AI 응답 데이터를 HTML로 변환하여 사용자 인터페이스에 동적으로 렌더링하는 구조 개발

- 응답 완료 이전 단계에서도 점진적 렌더링을 수행하여 UX 향상 및 사용자 대기 시간 최소화

- 메시지 버퍼링 및 상태 관리를 통한 효율적인 응답 데이터 처리 및 성능 최적화

- SSE 응답 상태 관리 로직을 구현하여 연결 상태 및 오류 발생 시 적절한 예외 처리

## 모노래포 빌드 로직 설계 및 구현

- Turbo 및 PNPM 기반 모듈형 빌드 시스템 설계 및 구현

- Version Map 및 Dependency Map을 활용한 모듈 간 의존성 관리 자동화

- 각 모듈의 독립적인 빌드 산출물 관리 및 중앙 집중형 배포 경로 설정으로 빌드 파일 체계화

- InitializerStrategy에서 Dependency Map을 통한 위상 정렬을 적용, 모듈 로딩 순서 최적화

- 빌드 스크립트 분리 및 자동화를 통한 효율적이고 안정적인 CI/CD 파이프라인 구축

- 해시 기반 버전 관리 시스템 도입하여 지속적인 배포 환경 개선 및 캐시 관리 최적화

- 빌드 산출물 관리 전략 설계 및 구현 (템플릿 번들링 별도 관리, 이전 빌드 파일 자동 삭제 등)

- 빌드 과정 중 발생 가능한 문제 진단 및 해결을 위한 빌드 출력 로그 관리 및 오류 처리 자동화

- 기존 젠킨스 로직 parallel 으로 변경하여 빌드 속도를 향상 시켰습니다.


## Playwright 기반 테스트 자동화<br/>

- 테스트 케이스 작성 및 유지보수<br/>

- Playwright를 활용하여 프론트엔드 전반의 E2E(End-to-End) 테스트 스크립트 작성<br/>

- UI 요소의 렌더링, 인터랙션, 상태 변화 등을 자동화된 테스트 케이스로 검증하여 기능 안정성 확보<br/>

## 메뉴<br/>

- SiteMap 기반 섹션 뷰모델 구성<br/>

- 어트리뷰트 정보를 기반으로 한 뷰모델 초기 구성 및 메뉴 타입 변환<br/>

- 제네레이터를 활용한 타입 변환 및 섹션별 구성 요소 매핑<br/>

- 데이터 변경 유형에 따른 분기 처리 (단순 항목 갱신 및 구조 변경 시 리졸버 호출)<br/>

- 섹션 및 아이템 컴포넌트 구조 설계<br/>

- 계층별 구분된 컴포넌트 설계 및 구현 (1단계~4단계)<br/>

- 항목 상태(접힘, 숨김, 기본값)에 따른 조건부 렌더링 구현<br/>

- 접기/펼치기 기능, 카드뷰 스타일 적용 및 항목 Hover 시 설정 버튼 노출 로직 구현<br/>

- 드래그 앤 드랍 기능 구현 (총 6가지)<br/>

- React DnD 기반 드래그 액션 처리 (그룹 추가, 이름 변경, 순서 변경, 그룹 삭제, 숨기기, 접기, 기본 설정)<br/>

- 동일 계층 간 및 특정 예외 케이스 간 이동 기능 지원<br/>

- 드래그 중 위치 안내 박스 및 화살표 표시, 완료 후 데이터 갱신 및 리렌더링 처리<br/>

- 메뉴 검색 기능 구현<br/>

- 검색어 입력 시 메뉴 항목 리스트업 및 순차 강조 처리<br/>

- Enter 키 활용한 순차 선택, 계층별 인덱스 관리 및 상태 저장<br/>

- 재사용 가능한 커스텀 훅으로 기능 캡슐화<br/>

- 메뉴 이름 변경 및 설정 기능 구현<br/>

- 이름 변경, 기본 설정, 숨기기, 접기 등 설정 기능 제공<br/>

- Hover 시 설정 버튼 노출 및 입력창 관리로 중첩 UI 이슈 해결<br/>

## 성능 최적화<br/>

- React.memo 적용 및 컴포넌트 리렌더링 최소화<br/>

- Throttle을 통한 불필요한 렌더링 방지 및 데이터 상태 관리 최적화<br/>

- 수정된 데이터만 반영하는 효율적인 리졸버 설계<br/>

- 메뉴 상태 저장 기능 구현<br/>

- 하단 저장 버튼 구현을 통해 메뉴 설정 상태 저장 (순서, 이름, 숨김/기본값 상태 등)<br/>

Context Layer 기반 상태 관리 및 데이터 업데이트 처리<br/>

- API 및 라우팅 개선 및 디버깅<br/>

- 메뉴 렌더링 및 데이터 로직 개선<br/>

- 메뉴 정렬 및 하이라이트 기능 개선<br/>

- 메인 컨테이너 및 앱 진입 로직 최적화<br/>

- 메뉴 데이터 변환 로직 고도화<br/>

- 신규 메뉴 데이터 변환 로직 설계 및 기존 로직(V3)을 V5로 업그레이드<br/>

- 메뉴 데이터 관리 전략 개선 및 메뉴 호출 방식 최적화<br/>

- 해시 라우터 이벤트 처리 개선<br/>

- 해시 기반 라우팅 및 클릭 이벤트 처리 로직 개선<br/>

- 무한 루프 및 에러 처리 로직 보완<br/>

## 즐겨찾기 및 추가 UI/UX 개선<br/>

- 즐겨찾기 서비스 코드 리팩토링 및 상태 업데이트 로직 개선<br/>

- 안정성 검증 및 내부 이슈 대응<br/>

- 버튼 설정 작업<br/>

- 버튼 설정 및 드래그 앤 드랍 구현<br/>

- 버튼 설정을 위한 구조 설계 및 데이터 변환 로직(제네레이터) 구현<br/>

- React DnD 활용하여 버튼 그룹 간 이동 및 새로운 그룹 추가 기능 개발<br/>

- 시각적 드래그 애니메이션 구현 및 정확한 위치 예측 UI 제공<br/>

- 버튼 속성 관리<br/>

- 버튼 옵션 관리 및 조건부 제약 로직 구현<br/>

- 미사용 버튼 처리 및 그룹 간 이동 기능 개발<br/>

- 버튼 그룹 관리 및 사용자 경험 개선<br/>

- 버튼 그룹 추가, 이름 변경 기능 구현 및 상태 관리<br/>

- 버튼 그룹 저장 및 히스토리 관리 기능 구현<br/>

- 안내 툴팁 및 사용자 경험 개선을 위한 UI/UX 작업<br/>

## Gantt Chart 작업<br/>

- Gantt 타입 구성 및 데이터 처리

- Gantt 차트 구성을 위한 데이터 제네레이터 및 리졸버 구현<br/>

- ECGantt 컴포넌트 개발 및 다중 태스크 표시 기능 확장<br/>

- Gantt 차트 성능 최적화 및 유지보수성 향상<br/>

- 코드 스플리팅 및 이넘(enum) 활용한 데이터 관리로 유지 보수성 향상<br/>

- 성능 최적화를 위한 Throttle 적용 및 불필요한 리렌더링 최소화<br/>

- 리졸버 및 제네레이터 구조 단순화로 코드 관리 효율성 증가<br/>

- 이러한 작업들을 통해 전체 ERP 시스템의 유지보수성과 사용자 경험 향상에 기여하였습니다.<br/>

# TCP-IaaS

<br/>

TCP(Tmax Cloud Platform) IaaS는 기업의 프라이빗 클라우드 구축을 위한 인프라 가상화 솔루션입니다.

Infra부터 App까지, 클라우드 Full Stack을 패키지 형태로 제공하여

클라우드 운영에 대한 고객의 부담을 줄였습니다.
<br/>
(깃 랩)
<br/>
![스크린샷 2023-11-16 오후 4 58 23](https://github.com/inth9198/inth9198/assets/82989054/a879a152-f5a6-4b93-adfe-cdbfda466e0e)
![스크린샷 2023-12-11 오전 10 52 50](https://github.com/inth9198/inth9198/assets/82989054/803d5ba2-19ce-4363-afef-c0f867f6d671)



<br/>
1. 프로젝트 초기 설정부터 배포 자동화(CI/CD)까지 프론트엔드 개발 전반을 주도하여 기여
하였으며(리드개발), 성능 최적화 및 모듈 관리에 대한 이해를 바탕으로 프로젝트 아키텍처
를 설계하고 유지 보수성을 향상하였습니다.<br/>
2. Webpack을 활용한 번들링 작업으로 파일 구조를 최적화해 페이지 로딩 속도를 개선했습니다.<br/>
3. Webpack Analyzer를 활요하여 불필요한 라이브러리를 제거해 빌드 파일 크기를 53MB에서 16MB
로 경량화해 로딩 속도를 높였습니다.<br/>
4. useContext와 structure schema를 사용한 폼 공통 컴포넌트 구현으로 폼의 일관성 유지와 유지보수
비용 절감을 이루었습니다.<br/>
5. JSON 스키마를 기반으로 모델 폼을 정의하고, 해당 JSON을 통해 CRUD 페이지를 자동 생성하는 시
스템을 설계 및 구현을 통해 UI 컴포넌트를 통일하고, 디자인 시스템의 일관성 유지와 유지보수 비용을 절
감하였습니다<br/>
6. Nginx와 도커, 쿠버네티스를 활용하여 배포(이미지를 생성한 뒤 디플로이, 서비스, 인그래
스 yaml작성 후 쿠버네티스를 이용하여 직접 배포)하여 지속적인 배포와 신속한 복구가 가능해졌습니다.<br/>
7. VM, 스토리지, 네트워크, Host OS 관련 페이지를 안정적으로 구현하여 사용자 리소스 관리가 용이해
졌습니다.<br/>
8. ESLint를 적용해 코드 스타일을 통일하고 오류를 사전 방지하여 DX(개발자 경험)를 최적화했습니다.<br/>
9. NPM을 통한 보안 취약점 분석과 수정으로 시스템 안정성을 높였습니다.<br/>
10. Chart.js, D3.js를 활용해 막대, 라인, 파이, 스캐터 차트를 구현하여 복잡한 데이터를 직관적으로 시
각화했습니다.<br/>
11. Lazy Loading을 도입하여 First Contentful Paint 시간을 2.8s에서 0.8s로 줄여 앱 성능과 사용자
경험을 개선했습니다.<br/>

성과/결과: 공군, 해군, 삼성디스플레이 등 다양한 b2b 수주등의 성과를 이루었습니다.<br/>


<br/>
<br/>

# TCP-PaaS

<br/>

TCP(Tmax Cloud Platform) PaaS는 경쟁력 있는 App을 개발하고
실행, 운영하는데 필요한 모든 요소를 제공하는 플랫폼 서비스입니다.
클라우드 시스템 Full Stack 엔진 및 관리 기술 통합 솔루션을 제공하며,
하드웨어나 운영체제, 미들웨어, 데이터베이스 관리 등에 상관없이
App 개발에만 집중할 수 있는 환경을 제공합니다.<br/>
(깃 랩)
<br/>

![image](https://github.com/user-attachments/assets/ac65f073-9f12-43bd-a934-d0360438519e)

<br/>
1. 네트워크 흐름을 시각화하기 위해 토폴로지 및 콜 트레이스 UI 컴포넌트를 구현하여 데이터 분석과 문
제 해결을 용이하게 했습니다.<br/>
2. react-hook-form과 yup을 사용해 폼 생성을 자동화하여 검증과 생성 속도를 높이고 코드 유지보수를
단순화했습니다.<br/>
3. 네트워크 관리 및 모니터링을 위한 페이지를 개발하여 사용자가 실시간 상태를 쉽게 확인할 수 있도록
했습니다.<br/>
4. TanStack Query를 통해 비동기 데이터 페칭과 캐싱을 관리하여 데이터 일관성과 성능을 최적화했습
니다.<br/>


<br/>
<br/>

# HyperCloud (오픈 소스 프로젝트)

<br/>

HyperCloud는 사용자 주도의 선택적 환경을 제공하는 클라우드 엔진으로, 사용자 환경에 맞는 인프라 및 플랫폼을 선택하여 빠르고 쉽게 구성해주는 통합 클라우드입니다.  또한, 검증을 거친 다양한 오픈소스와 Tmax의 소프트웨어의 결합 및 자동화를 통해 사용 편의를 고려하였으며, 이를 통해 클라우드 기반 신기술 활용에 최적화를 이룬 클라우드 플랫폼입니다.

<br/>
<img width="1323" alt="스크린샷 2022-12-13 오후 5 19 06" src="https://user-images.githubusercontent.com/82989054/207263087-6e74057a-d5db-4842-a1af-a57140f5bd82.png">
<br/>
1. 싱글 클러스터 관련 페이지를 구현해 단일 클러스터 관리가 직관적으로 가능해졌습니다.<br/>
2. CLI에 익숙한 사용자들을 위한 터미널 기능을 UI에 추가하여 사용자 편의성을 강화했습니다.<br/>
3. 많은 기능으로 인해 사용자가 복잡한 인식을 주는 것을 피하기 위해 ClusterMenuPolicy를 설정하여
필요한 메뉴만 볼수있도록 하였습니다<br/>
4. 오픈소스 프로젝트임으로 여러스타일의 코드와 컴포넌트가 섞여있어서 컴포넌트 공통화 및 최적화하였
습니다<br/>
5. 실시간 업데이트를 위해 HTTP에서 웹소켓으로 통신을 변경하여 최신 정보 제공이 용이해졌습니다.<br/>
6. Jenkins를 통한 CI/CD 배포 자동화로 안정적이고 신속한 배포가 가능해졌습니다.<br/>
7. i18n 라이브러리를 적용해 글로벌 사용자들이 일관된 언어 환경에서 서비스를 사용할 수 있도록 했습니
다.<br/>
8. Lazy Loading을 적용해 페이지 초기 로딩 속도를 줄여 사용자 경험과 성능을 개선했습니다.<br/>

성과/결과: 공공, 금융, 제조 등 다양한 분야에서 20여 고객(신한투자증권, 신한은행,국민연금공단, 소방청,충남대학교등등) 보유<br/>

<br/>


# superPX

<br/>

데이터 베이스 활용가능한 코드 에디터
<br/>
(깃 랩으로 이전)
<br/>
![image](https://github.com/inth9198/inth9198/assets/82989054/b3db300c-2606-497f-a244-47d4067c46e6)
![image (1)](https://github.com/inth9198/inth9198/assets/82989054/079ece85-2fc8-4442-87b8-5e3412401268)


<br/> 
1. 파일트리, 브랜치 앤 태그, ci/cd, 머지 리퀘스트, 커밋 히스토리, 에디터페이지 등 여러서비스 및 페이지 구현<br/>
2. 모나코 에디터를 이용하여 편집기능 구현<br/>
<br/>



# 온코리더

<br/>

## 프로젝트 설명

## 유전체 정보 기반으로 큐레이션 및 의견을 공유할 수 있는 플랫폼
<br/> 

(인턴 진행하면서 참여함)

![image](https://user-images.githubusercontent.com/82989054/183032376-28917d04-b619-4c80-88d4-3d8edee64693.png)

<br/> 
1. 로그인 페이지 및 기능 구현
<br/>

# 하우머치

<br/>

중고 가격 추천 웹페이지
(https://github.com/How-much-do-you-need/how-much)
<br/>
![스크린샷 2023-06-27 오후 12 38 02](https://github.com/inth9198/inth9198/assets/82989054/5d591abc-0c85-42fb-83f5-6001941b295c)
![스크린샷 2023-06-27 오후 12 38 14](https://github.com/inth9198/inth9198/assets/82989054/1d9c95f7-d9f2-4ac0-8847-bcc5ab92e378)

<br/>
1. 클라우드를 이용한 배포까지 완료
<br/>

# INEX

<br/>

ai를 활용한 본인 얼굴상에 가장 적합한 소속사를 찾아주는 웹페이지

<br/>

![image](https://github.com/inth9198/inth9198/assets/82989054/57edb3cb-e175-4638-a88b-d3a84d7121c1)


<br/>
https://staroasis.shop/
(서비스 주소)

<br/>

<br/> <br/> 

# 4차산업기술인재 - Web 1기
* 깃허브 주소
https://github.com/inth9198/4th_industrial_technology_talent
* 개발 노트
https://www.notion.so/softsquared/8d25c2cb344b40f6a05b9ff049dd55eb
* 마켓컬리 클로코딩
https://github.com/inth9198/4th_industrial_technology_talent
* 과제 시현 영상
https://www.notion.so/softsquared/3-React-Component-JSX-Props-3d4deb639b55487b8fa83d975eec2ea5

<br/>
<br/>

### 프로젝트 스킬 스택

- next.js - react framework
- redux(redux-saga) - status management
- emotion - css-in-js(style library)
- formik - form control library
- material-ui - ui library

<br/> 

### 프로젝트에 기여한점
데이터 파이프라인을 구축하여 정제된 데이터가 화면에 출력될수 있도록 함

react framework, redux-saga를 이용하여 로그인 시스템 구현

<br/> <br/> <br/> 

# 클래스액트 인턴(특허 기여)

## 임상 유전체, 임상시험 빅데이터 기반 암 진단 및 치료 지원 전문가 서비스

<br/> 

중소벤처기업부 R&D
빅데이터 기반
서비스 개발 사업

<br/> 

## 임상 참여 조건의 정형화를 위한 데이터 파이프라인 처리방법
1) 진행기간 : 2022/03/02~2022/06/30
2) 주요내용 : 환자에게 임상실험을 추천하는 알고리즘 (특허)
3) 출원 번호 : 10-2022-0067882
4) 논문명칭 : 임상 참여 조건의 정형화를 위한 데이터 파이프라인 처리방법
5) 본인이 기여한 점 : 모든 파이프 라인에 대한 코드 구현
6) 사용한 기술스택 및 지식 : 파이썬(주피터 노트북, pandas, numpy)
7) 결과 및 성과 : 이전 임상 실험 추천 시스템보다 더욱 구체화된 환자에 맞춤형 임상 실험 추천하는 파이프라인 구축

<br/> 

## 임상 참여 조건의 정형화를 위한 속성인식방법

1) 진행기간 : 2022/03/02~2022/06/30
2) 주요내용 : 환자에게 임상실험을 추천하는 알고리즘 (특허)
3) 출원 번호 : 10-2022-0067881
4) 논문명칭 : 임상 참여 조건의 정형화를 위한 속성인식방법
5) 본인이 기여한 점 : 모든 파이프 라인에 대한 코드 구현
6) 사용한 기술스택 및 지식 : 파이썬(주피터 노트북, pandas, numpy)
7) 결과 및 성과 : 이전 임상 실험 추천 시스템보다 더욱 구체화된 환자에 맞춤형 임상 실험 추천하는 파이프라인 구축을 위해 환자의 속성을 인식하는 방법

<br/> 

## 클래스액트 인턴(논문 참여)

1) 진행기간 : 2022/03/02~2022/06/30
2) 주요내용 : 부정을 처리하는 알고리즘 논문(예정)
3) 본인이 기여한 점 : 부정을 처리하는 알고리즘에 대한 모든 코드 적 부분의 구현과 여러 알고리즘 추가
4) 사용한 기술스택 및 지식 : 파이썬(주피터 노트북, pandas, numpy)
5) 결과 및 성과 : 이전에 부정을 처리하는 알고리즘 negex, deepen 에 비하여 더 좋은 성능을 나타냄 (더 높은 accuracy, 더 높은 precision, 더 높은 recall, 더 높은 f1, 더 적은 loss)


<br/> 

## 개발자가 된 후 스터디

### 데브옵스 스터디
<br/> 
### 리눅스
<br/> 

https://github.com/inth9198/dev-ops-study
<br/> 

### 프론트엔드 스터디(웹팩, 바벨, eslint, 도커, 리액트 쿼리, 최신 라이브러리나 스택등등)

<br/> 
https://rigorous-tibia-973.notion.site/f70fb898b13d482db2729b51b11a9234?pvs=4
<br/> 
https://velog.io/@inth19198/posts
<br/> 

### UX 스터디

<br/> 
https://www.notion.so/UX-a8800e77ae0745dab052eda00818e10a
<br/>

![스크린샷 2024-03-11 14 07 31](https://github.com/inth9198/inth9198/assets/82989054/cb4453ef-90ef-4954-8a0a-31c7e23a6ed8)

<br/> <br/> 

![inth9198's GitHub stats](https://github-readme-stats.vercel.app/api?username=inth9198&show_icons=true&theme=radical)

### LLM 애플리 캐이션 구축



# 🦀Skills
### Platforms & Languages

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E.svg?&style=for-the-badge&logo=JavaScript&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26.svg?&style=for-the-badge&logo=HTML5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6.svg?&style=for-the-badge&logo=CSS3&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB.svg?&style=for-the-badge&logo=React&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6.svg?&style=for-the-badge&logo=TypeScript&logoColor=white)
![Redux](https://img.shields.io/badge/Redux-764ABC.svg?&style=for-the-badge&logo=Redux&logoColor=white)
</br>
![Python](https://img.shields.io/badge/Python-3776AB.svg?&style=for-the-badge&logo=Python&logoColor=white)
![numpy](https://img.shields.io/badge/numpy-013243.svg?&style=for-the-badge&logo=numpy&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458.svg?&style=for-the-badge&logo=pandas&logoColor=white)



## 백준 티어

[![Solved.ac Profile](http://mazassumnida.wtf/api/v2/generate_badge?boj=inth9198)](https://solved.ac/inth9198/)

<br/> <br/> <br/> 

## CS공부 노션
https://www.notion.so/171722de241544698e075f5fe42d06ce
<br/> <br/> <br/> 
https://www.notion.so/b7a400f79ff84743ae8254472646e7e6

## 블로그
https://velog.io/@inth19198/posts

## 네이버 블로그
https://blog.naver.com/inth2474
<br/> <br/> <br/> 

## 개발일지
https://www.notion.so/softsquared/8d25c2cb344b40f6a05b9ff049dd55eb

### 인사평가


![스크린샷 2024-09-26 17 30 19](https://github.com/user-attachments/assets/fecc84eb-5985-4135-85aa-ecaf8142fefe)
![스크린샷 2024-09-26 17 30 08](https://github.com/user-attachments/assets/06a94cb3-061a-42d4-ad31-48775a92db62)
![스크린샷 2024-09-26 17 29 56](https://github.com/user-attachments/assets/e25f2443-3bf7-4223-a71f-e162db4510b7)
