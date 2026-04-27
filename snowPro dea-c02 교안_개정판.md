# SnowPro Advanced: Data Engineer (DEA-C02) 학습 가이드 [개정판]

---

## 목차

1. [시험 개요 및 도메인 구성](#1-시험-개요-및-도메인-구성)
2. [도메인별 학습 로드맵](#2-도메인별-학습-로드맵)
3. [핵심 개념 비교 요약](#3-핵심-개념-비교-요약)
4. [예상 문제 100선 (도메인 순)](#4-예상-문제-100선-도메인-순)

---

## 1. 시험 개요 및 도메인 구성

> 시험 비중이 가장 높은 도메인부터 우선순위를 두어 설계된 커리큘럼입니다.
> **사전 요구사항:** 반드시 **SnowPro Core 자격증** 보유 필요 / 자격증 유효기간: 취득 후 **2년**

| 단계 | 학습 도메인 (비중) | 주요 학습 주제 및 핵심 개념 |
|------|-------------------|---------------------------|
| **1단계** | Data Movement (28%) | - 데이터 로딩: `COPY INTO` 사용법, `INFER_SCHEMA` 활용<br>- 지속적 파이프라인: Snowpipe(Auto Ingest vs REST API), Snowpipe Streaming(저지연 수집)<br>- 데이터 공유: Data Marketplace, Listing을 통한 공유, 로우 레벨 필터링<br>- 커넥터: Kafka, Spark, Python 커넥터 설정 및 활용 |
| **2단계** | Data Transformation (25%) | - 함수 및 프로시저: Snowpark UDF(Java, Python, Scala), 저장 프로시저 개발<br>- 데이터 처리: JSON 등 반정형 데이터 평면화(`LATERAL FLATTEN`), 비정형 데이터 처리<br>- Snowpark: 아키텍처 이해 및 DataFrame 활용 변환<br>- 최신 기능: Snowflake Cortex를 활용한 AI 워크플로우 및 데이터 분류 |
| **3단계** | Performance Optimization (19%) | - 쿼리 튜닝: 성능 저하 쿼리 식별 및 원인 분석(클러스터링 부족, `SELECT *` 남용 등)<br>- 최적화 서비스: 검색 최적화(Search Optimization), 쿼리 가속화(Query Acceleration)<br>- 캐싱 및 가상 웨어하우스: Result Cache 활용, 웨어하우스 스케일 업/아웃 전략<br>- 모니터링: Resource Monitor 설정 및 크레딧 소비 최적화 |
| **4단계** | Data Governance (14%) | - 보안 정책: Dynamic Data Masking, Row Access Policy(행 단위 보안)<br>- 데이터 관리: Object Tagging, 데이터 분류(Classification), 데이터 리니지 추적<br>- 데이터 클린룸: Snowflake Data Clean Rooms를 통한 안전한 데이터 공유<br>- Horizon Catalog: 외부 데이터 페더레이션 및 거버넌스 관리 |
| **5단계** | Storage & Data Protection (14%) | - 복구 기능: Time Travel(보관 기간 설정 및 비용 균형), Fail-safe, 복제(Replication)<br>- 스토리지 분석: 마이크로 파티션 분석 및 Clustering Depth 확인<br>- 복제(Cloning): Zero-copy Clone을 활용한 개발 환경 구축 및 권한 상속 이해 |

> **시험 전략:** 단순 기능 암기보다 **"왜 이 기능을 선택하는가"** 에 대한 시나리오 기반 판단력이 중요합니다.
> 예: 변경이 잦은 테이블 → Time Travel 기간 단축, 동시 사용자 급증 → Multi-cluster 웨어하우스 선택

---

## 2. 도메인별 학습 로드맵

> 각 도메인의 공식 랩(Lab)과 핵심 실무 포인트를 정리했습니다.

| 단계 | 핵심 실습 (Hands-on Lab) | 집중 학습 포인트 |
|------|------------------------|----------------|
| **1단계: Data Movement** | - Lab: Auto-Ingest Twitter Data into Snowflake<br>- Lab: Automating Data Pipelines<br>- Kafka/Spark/Python 커넥터 설정 실습 | Snowpipe vs Snowpipe Streaming 차이 구분<br>`INFER_SCHEMA` 활용 테이블 자동 설계<br>`COPY INTO` 로드 이력 64일 보관 원리 |
| **2단계: Data Transformation** | - Lab: Intro to Data Engineering with Snowpark for Python<br>- SQL: `LATERAL FLATTEN`으로 중첩 JSON 평면화 실습<br>- Cortex AI 함수(`SUMMARIZE`, `SENTIMENT`) SQL 호출 실습 | Snowpark Lazy Evaluation 이해<br>UDF / UDTF / UDAF / Stored Procedure 용도 구분<br>Scoped URL vs Pre-signed URL 차이 숙지 |
| **3단계: Performance Optimization** | - Lab: Resource Optimization: Performance<br>- Lab: Resource Optimization: Usage Monitoring<br>- `ACCOUNT_USAGE.QUERY_HISTORY` 뷰로 병목 쿼리 분석 | `SELECT *` 지양, 클러스터링 키 설정 우선<br>Scale-up(메모리 부족) vs Scale-out(동시성)을 구분<br>QAS(단일 쿼리) vs Multi-cluster(동시성) 명확히 구분 |
| **4단계: Data Governance** | - DDL: Dynamic Data Masking / Row Access Policy 직접 작성<br>- Governance: Object Tagging 및 데이터 분류 실습<br>- Tag-based Masking 일괄 적용 실습 | 거버넌스 4대 정책(Masking, Row Access, Projection, Aggregation) 구분<br>태그 상속(Inheritance) 원리 이해<br>Data Clean Room vs Direct Share 차이 |
| **5단계: Storage & Data Protection** | - Lab: Getting Started with Time Travel<br>- `CREATE OR REPLACE TABLE` 후 UNDROP 복구 시나리오 실습<br>- Zero-copy Clone 생성 및 권한 상속 검증 | Time Travel 기간 = 스토리지 비용 균형<br>Permanent / Transient / Temporary 테이블별 Fail-safe 유무<br>`SYSTEM$CLUSTERING_DEPTH`로 파티션 효율 측정 |

**공통 실무 주의사항**

| 시나리오 | 권장 접근법 |
|---------|-----------|
| 데이터 변경이 잦은 테이블의 비용 증가 | Time Travel 보존 기간을 테이블/스키마 단위로 줄임 |
| `CREATE OR REPLACE TABLE`로 덮어쓴 경우 | 새 테이블 Rename → 이전 테이블 `UNDROP` |
| 특정 스키마만 긴 이력 필요 | 계정 전체가 아닌 스키마 레벨에서 `DATA_RETENTION_TIME_IN_DAYS` 설정 |
| ETL 중간 단계 임시 테이블 | Transient Table 사용 (Fail-safe 없어 비용 절감) |

---

## 3. 핵심 개념 비교 요약

### 3.1 데이터 수집 방식 비교 (1단계)

| 방식 | 지연 시간 | 트리거 방법 | 주요 사용 사례 |
|-----|---------|-----------|-------------|
| **COPY INTO** | 배치 | 수동 실행 또는 스케줄 Task | 대용량 파일 일괄 로드 |
| **Snowpipe (Auto-Ingest)** | 분 단위 | S3/Azure 이벤트 알림(SQS 등) 자동 감지 | 클라우드 스토리지 파일 자동 수집 |
| **Snowpipe (REST API)** | 분 단위 | 외부 앱이 REST 엔드포인트 직접 호출 | 애플리케이션 연동 수집 |
| **Snowpipe Streaming** | 초 단위(저지연) | Snowflake SDK/API 직접 호출 | IoT 센서, 실시간 이벤트, Kafka 스트리밍 |

> **핵심 구분:** 지연 시간이 중요하면 → Snowpipe Streaming / 파일 기반 자동화는 → Snowpipe Auto-Ingest

**Snowpipe 파일 로드 이력:** Snowflake는 로드된 파일 목록을 **64일간** 유지합니다. 64일이 지난 파일은 중복 로드될 수 있으므로 `LOAD_UNCERTAIN_FILES = TRUE` 또는 테이블 Truncate 후 재로드가 필요합니다.

---

### 3.2 데이터 변환 주요 객체 비교 (2단계)

**UDF / UDTF / UDAF / Stored Procedure 비교**

| 유형 | 입력 | 출력 | 트랜잭션 | 주요 용도 |
|-----|------|------|---------|---------|
| **Scalar UDF** | 단일 행 | 단일 값 | 불가 | 값 계산, 변환 |
| **UDTF** | 단일 행 | 복수 행(테이블) | 불가 | 텍스트 토큰화, 배열 확장 |
| **UDAF** | 복수 행 | 단일 집계값 | 불가 | 커스텀 집계 함수 |
| **Stored Procedure** | 인자 | 결과 또는 없음 | 가능(BEGIN/COMMIT/ROLLBACK) | 비즈니스 로직, DML 실행, 배치 처리 |

- `EXECUTE AS OWNER`: 호출자 권한이 아닌 **소유자 권한**으로 실행 (보안 통제 하에 특정 작업 허용)
- `SECURE` 옵션: UDF/View 정의 로직이 `GET_DDL` 등으로 노출되지 않도록 보호

**Snowpark 핵심 원리**

| 개념 | 설명 |
|-----|------|
| **Lazy Evaluation** | `.filter()`, `.select()` 등 변환 작업은 즉시 실행되지 않고, `.collect()` / `.show()` / `.count()` 같은 Action 함수 호출 시에만 Snowflake 엔진에서 한꺼번에 실행 |
| **DataFrame → SQL** | 클라이언트에서 작성한 Python/Java/Scala 코드가 자동으로 최적화된 SQL로 변환되어 Snowflake에서 실행 |
| **Snowpark-optimized WH** | 일반 웨어하우스보다 노드당 메모리가 훨씬 많음. 머신러닝 전처리, 복잡한 집계 등 메모리 집약적 Snowpark 작업에 필수 |

**파일 URL 유형 비교 (비정형 데이터)**

| URL 유형 | 생성 함수 | 유효 기간 | 접근 권한 | 주요 용도 |
|---------|---------|---------|---------|---------|
| **Scoped URL** | `BUILD_SCOPED_FILE_URL()` | 쿼리 결과 캐시 유효 시간(~24h) | 생성한 사용자만 | UDF 내 파일 처리, 세션 내 보안 전달 |
| **Pre-signed URL** | `GET_PRESIGNED_URL()` | 만료 시간 설정 가능 | URL만 있으면 누구든 접근 가능 | BI 도구 임베드, 외부 파트너 공유 |
| **File URL** | `BUILD_STAGE_FILE_URL()` | 영구(단, 매 접근 시 Snowflake 인증 및 스테이지 권한 필요) | 스테이지 권한 보유 역할만 | 장기 참조 링크 |

> ⚠️ 'Pre-scoped URL'은 공식 Snowflake 용어가 아닙니다.

---

### 3.3 성능 최적화 도구 비교 (3단계)

| 도구 | 목적 | 해결하는 문제 | 비고 |
|-----|------|------------|-----|
| **Result Cache** | 동일 쿼리 반복 비용 제거 | 동일 쿼리 재실행 | 자동 활성화, 무료 |
| **Automatic Clustering** | 마이크로 파티션 정렬 유지 | 파티션 프루닝 비효율 | 크레딧 소비 |
| **Search Optimization Service** | Point Lookup 쿼리 가속 | 대용량 테이블에서 특정 값 검색 | 테이블별 활성화, 스토리지 비용 추가 |
| **Query Acceleration Service (QAS)** | Outlier 쿼리 가속화 | **단일 쿼리**가 평균 대비 스캔 파티션 수가 많아 과도한 리소스를 사용하는 경우 | Shared compute로 오프로드 |
| **Multi-cluster Warehouse** | 동시성(Concurrency) 해결 | **다수 사용자** 동시 접속으로 큐 대기 발생 | 활성 클러스터 수만큼 크레딧 소비 |
| **Scale-up (사이즈 확대)** | 단일 쿼리 리소스 확보 | 메모리 부족으로 Disk Spilling 발생 | - |

> **핵심 구분:** QAS = 단일 쿼리 이상 처리 / Multi-cluster = 동시 사용자 증가 대응

**모니터링 뷰 비교**

| 뷰 | 데이터 반영 지연 | 보관 기간 | 주요 용도 |
|---|--------------|---------|---------|
| **ACCOUNT_USAGE** | 45분 ~ 3시간(뷰별 상이) | 1년(365일) | 장기 비용 분석, 감사, 삭제 객체 포함 |
| **INFORMATION_SCHEMA** | 지연 없음(실시간) | 7일 ~ 6개월(뷰/함수별 상이) | 즉각적인 모니터링, 삭제 객체 미포함 |

> 참고: `QUERY_HISTORY` 테이블 함수는 **최근 7일 또는 10,000행** 중 먼저 도달하는 조건까지만 반환하며, `LOAD_HISTORY`는 **14일**, `DATABASE_STORAGE_USAGE_HISTORY`는 **6개월**까지 반환 등 뷰·함수별로 보존 기간이 다릅니다.

---

### 3.4 거버넌스 4대 보안 정책 비교 (4단계)

| 정책 | 제어 단위 | 효과 | 주요 사용 사례 |
|-----|---------|-----|-------------|
| **Dynamic Data Masking** | 컬럼 값 | 역할에 따라 값 마스킹/변환 출력 | 전화번호 뒷자리 숨김, SSN 마스킹 |
| **Row Access Policy** | 행(Row) | 역할/속성에 따라 보이는 행 필터링 | 지역별 담당자 데이터 접근 제한 |
| **Projection Policy** | 컬럼 조회 자체 | 특정 컬럼을 SELECT에 포함하는 것 자체를 차단 | `SELECT *` 통한 대량 컬럼 유출 방지 |
| **Aggregation Policy** | 쿼리 결과 형태 | 개별 행 조회 차단, 집계 결과(COUNT/SUM)만 허용 | 개인 정보 집계 통계만 허용 |

**태그(Tag) 기반 거버넌스**

- **Object Tagging**: 객체(DB/스키마/테이블/컬럼)에 메타데이터 레이블 부여 → 분류 및 정책 일괄 적용 기반
- **Tag Inheritance**: 상위 객체에 부여된 태그는 하위 객체로 자동 전파 (명시적 변경 전까지 유지)
- **Tag-based Masking**: 특정 태그가 붙은 모든 컬럼에 마스킹 정책을 일괄 자동 적용

**Data Metric Functions (DMF)**: 행 수, NULL 비율, 고유값 수 등 데이터 품질 지표를 시스템이 자동으로 정기 모니터링

---

### 3.5 테이블 유형별 데이터 보호 비교 (5단계)

| 테이블 유형 | Time Travel 최대 기간 | Fail-safe | 세션 종료 후 | 주요 용도 |
|-----------|---------------------|---------|------------|---------|
| **Permanent** | 90일 (Enterprise) | 7일 | 유지 | 일반 업무 데이터 |
| **Transient** | 1일 | **없음** | 유지 | ETL 중간 단계, 비용 절감 |
| **Temporary** | 1일 | **없음** | **삭제** | 세션 내 임시 처리 |
| **External** | 없음 | 없음 | 유지(메타데이터만) | 외부 스토리지 데이터 참조 |
| **Iceberg** | 지원(Snowflake Managed) | **없음** | 유지 | 오픈 포맷 레이크하우스 |
| **Hybrid** | 지원 ⚠️ (TIMESTAMP 기준만, UNDROP 불가) | **없음** | 유지 | OLTP+OLAP 혼합 워크로드 |
| **Dynamic** | 지원 | 지원(기본 Permanent) ⚠️ | 유지 | 자동 증분 파이프라인 |

> **Fail-safe 핵심:** Time Travel 종료 후 7일간 유지. **사용자가 직접 접근 불가**, Snowflake 고객 지원팀을 통해서만 복구 가능. 스토리지 비용 발생.
> ⚠️ **Hybrid Table Time Travel 제한:** `AT` 절에서 TIMESTAMP 파라미터만 지원하며, 동일 DB에 속한 모든 Hybrid Table에는 동일 TIMESTAMP 값을 사용해야 합니다. OFFSET, STATEMENT, STREAM 파라미터 및 BEFORE 절 미지원. UNDROP TABLE 미지원. Time Travel 데이터는 행 저장소(row store)가 아닌 객체 스토리지에 저장되어 일반 테이블 요율로 과금됩니다(2025.08 업데이트).
> ⚠️ **Iceberg Table Fail-safe 미지원:** Snowflake-managed 및 External Iceberg 테이블 모두 Fail-safe를 지원하지 않습니다. Time Travel은 Snowflake-managed에 한해 지원됩니다.
> ⚠️ **Hybrid Table 주요 미지원 기능:** 공식 문서의 "Unsupported features" 기준 — **Fail-safe, Data Sharing, Dynamic Tables, Materialized Views, Replication, Search Optimization Service, Snowpipe(Streaming 포함), Streams, UNDROP, Query Acceleration Service, Clustering Keys** 모두 미지원. AWS·Azure 상용 리전에서만 GA(GCP·SnowGov·Trial 미지원).
> ⚠️ **Dynamic Table Fail-safe 조건부 지원:** `CREATE DYNAMIC TABLE`은 기본적으로 Permanent 유형으로 생성되어 **기본 7일 Fail-safe**가 적용됩니다. 리프레시 빈도가 높은 경우 스토리지 비용이 크게 늘 수 있으므로 `CREATE DYNAMIC TABLE ... TRANSIENT` 옵션으로 Transient Dynamic Table을 생성하면 Fail-safe 비용을 제거할 수 있습니다.

---

## 4. 예상 문제 100선 (도메인 순)

---

### 🔵 1단계: Data Movement (28%) — Q1 ~ Q25

---

**Q1.** IoT 센서에서 발생하는 고빈도 소량 데이터를 실시간에 가깝게(Near Real-time) 수집하기 위해 가장 적합한 도구는 무엇입니까?

- A) Traditional Snowpipe
- B) **Snowpipe Streaming** ✅
- C) Bulk Loading using COPY INTO
- D) External Tables

> **해설:** Snowpipe Streaming은 저지연(Low-latency) 수집을 위해 설계되었으며, 마이크로 배칭을 통해 소량의 레코드를 매우 빈번하게 수집하는 IoT나 이벤트 데이터에 최적화되어 있습니다.

---

**Q2.** 클라우드 스토리지에 64일 이상 보관된 파일을 테이블에 중복 없이 다시 로드하려면 어떤 방법을 사용해야 합니까?

- A) FORCE = TRUE 옵션과 함께 COPY INTO 실행
- B) 테이블을 Truncate한 후 COPY INTO 실행
- C) **LOAD_UNCERTAIN_FILES = TRUE 옵션 사용** ✅
- D) 기존 데이터를 삭제(Delete)하고 COPY INTO 실행

> **해설:** Snowflake는 로드된 파일 목록을 64일간 유지합니다. 64일이 지난 파일은 로드 이력이 사라져 Snowflake가 해당 파일의 로드 여부를 확인할 수 없는 상태(Uncertain)가 됩니다. `LOAD_UNCERTAIN_FILES = TRUE` 옵션은 이처럼 이력이 불확실한 파일만 대상으로 로드를 시도하는 Snowflake의 **공식 권장 방법**입니다. 테이블 Truncate(B)는 전체 데이터를 삭제하고 재로드해야 하므로 부작용이 크고 더 침습적인 우회 방법입니다. `FORCE = TRUE`(A)는 64일 이내 이력이 있는 파일을 무조건 중복 재로드하므로 중복 데이터를 발생시킵니다.

---

**Q3.** Snowpipe를 구성할 때, 'Auto-Ingest' 방식과 'REST API' 호출 방식의 가장 큰 차이점은 무엇입니까?

- A) Auto-Ingest는 수동으로 `ALTER PIPE ... REFRESH`를 호출해야 한다.
- B) REST API 방식은 클라우드 스토리지의 이벤트 알림 서비스(S3 Event 등)를 사용한다.
- C) **Auto-Ingest는 클라우드 메시징 서비스 알림을 통해 자동 실행되며, REST API는 외부 애플리케이션이 직접 호출한다.** ✅
- D) REST API 방식은 오직 내부 스테이지(Internal Stage)에서만 작동한다.

> **해설:** Auto-Ingest는 S3나 Azure Blob의 이벤트 알림을 감지하여 자동으로 작동하는 반면, REST API 방식은 사용자의 코드나 툴에서 직접 파이프를 트리거합니다.

---

**Q4.** `COPY INTO` 명령으로 파일을 로드할 때 64일 이내에 로드된 이력이 있는 파일을 중복 여부와 상관없이 강제로 재로드하려면 어떤 옵션을 사용해야 합니까?

- A) ON_ERROR = CONTINUE
- B) LOAD_UNCERTAIN_FILES = TRUE
- C) **FORCE = TRUE** ✅
- D) PURGE = TRUE

> **해설:** `FORCE = TRUE` 옵션을 사용하면 Snowflake의 파일 로드 이력(64일 보관)을 무시하고 무조건 해당 파일을 재로드합니다. 단, 중복 데이터가 삽입될 수 있으므로 주의해야 합니다. 반면 `LOAD_UNCERTAIN_FILES = TRUE`는 로드 이력 자체가 불확실한(64일 초과 등) 파일만 로드하는 옵션입니다.

---

**Q5.** Kafka Connector를 사용하여 Snowflake로 데이터를 인입할 때, 가장 낮은 지연 시간(Latency)과 비용 효율성을 동시에 달성하기 위해 선택할 수 있는 인입 방식은?

- A) Kafka Connector with Internal Stage
- B) Kafka Connector with Snowpipe (Auto-ingest)
- C) **Kafka Connector with Snowpipe Streaming** ✅
- D) Kafka Connector with Bulk Loading

> **해설:** 최신 Kafka 커넥터는 Snowpipe Streaming을 지원하여, 중간 스테이지 파일 생성 없이 데이터를 Snowflake 테이블로 직접 스트리밍 로드함으로써 지연 시간과 스토리지 비용을 획기적으로 줄입니다.

---

**Q6.** Kafka Connector를 통해 데이터를 인입할 때, 데이터의 누락이나 중복 없이 '정확히 한 번'만 전달되도록 보장하는 Snowflake의 메커니즘은?

- A) Auto-Ingest
- B) **Exactly-once Delivery (via Snowpipe Streaming)** ✅
- C) REST API Retries
- D) Materialized View Refresh

> **해설:** 최신 Snowflake Kafka 커넥터와 Snowpipe Streaming을 함께 사용하면, 오프셋(Offset) 관리를 통해 분산 환경에서도 데이터의 유실이나 중복 없이 정확히 한 번 전달되는 것을 보장합니다.

---

**Q7.** Snowpipe를 통해 데이터를 로드하는 과정에서 발생한 오류 이력을 확인하고, 특정 파이프의 현재 상태(작동 중 여부 등)를 진단하기 위해 사용하는 시스템 함수는?

- A) SYSTEM$PIPE_ERROR_LOG
- B) SYSTEM$GET_PRECONDITION
- C) **SYSTEM$PIPE_STATUS** ✅
- D) SYSTEM$VALIDATE_PIPE

> **해설:** `SYSTEM$PIPE_STATUS` 함수를 실행하면 해당 Snowpipe가 현재 실행 중인지, 대기열 상태는 어떤지 등의 실시간 정보를 JSON 형태로 반환하여 문제 해결을 돕습니다.

---

**Q8.** `COPY INTO` 명령을 사용하여 대량의 CSV 파일을 로드하던 중, 일부 파일에 형식이 맞지 않는 행이 포함되어 오류가 발생했습니다. 오류가 발생한 행만 무시하고 나머지 정상 데이터를 계속 로드하기 위해 사용해야 할 옵션은?

- A) ON_ERROR = ABORT_STATEMENT
- B) **ON_ERROR = CONTINUE** ✅
- C) ON_ERROR = SKIP_FILE_1
- D) PURGE = TRUE

> **해설:** `ON_ERROR = CONTINUE` 옵션은 데이터 로드 중 오류를 발견하더라도 해당 행을 건너뛰고 나머지 데이터 로드를 지속하여 전체 파이프라인의 중단을 방지합니다.

---

**Q9.** `COPY INTO` 명령을 사용하여 Snowflake 데이터를 외부 클라우드 스토리지로 내보낼(Unload) 때의 장점이 **아닌** 것은?

- A) 데이터 전송 시 Gzip 등의 압축 옵션을 선택할 수 있다.
- B) CSV, Parquet 등 다양한 파일 형식을 지정할 수 있다.
- C) **데이터를 항상 Snowflake 내부 스토리지에만 저장하도록 강제한다.** ✅
- D) 대량의 데이터를 병렬로 빠르게 내보낼 수 있다.

> **해설:** `COPY INTO <location>`의 핵심 장점은 데이터를 외부 클라우드 스토리지(S3, Azure Blob 등)로 직접 써서 다른 시스템과의 상호운용성을 높이는 것입니다.

---

**Q10.** 스테이지에 있는 파일을 `COPY INTO`로 로드할 때, 파일의 이름(FILENAME)이나 행 번호(ROW_NUMBER)와 같은 메타데이터를 함께 테이블 컬럼에 저장하기 위해 사용하는 방식은?

- A) INFER_SCHEMA 함수를 호출한다.
- B) **`METADATA$FILENAME` 및 `METADATA$FILE_ROW_NUMBER` 가상 컬럼을 쿼리에 포함한다.** ✅
- C) 별도의 외부 함수(External Function)를 작성한다.
- D) 파일 포맷 설정에서 INCLUDE_METADATA = TRUE를 지정한다.

> **해설:** Snowflake는 데이터 로드 시 파일의 출처를 추적할 수 있도록 가상 메타데이터 컬럼을 제공합니다. 이를 SELECT 문에 포함하여 실제 테이블 컬럼에 매핑할 수 있습니다.

---

**Q11.** `INFER_SCHEMA` 기능이 **지원하지 않는** 파일 형식은?

- A) Apache Parquet
- B) Apache Avro
- C) ORC
- D) **XML** ✅

> **해설:** Snowflake 공식 문서에 따르면 `INFER_SCHEMA`는 **Parquet, Avro, ORC, JSON, CSV**(PARSE_HEADER=TRUE 옵션 필요) 형식을 지원합니다. XML은 지원하지 않습니다. CSV의 경우 파일 포맷에 `PARSE_HEADER = TRUE`를 설정해야 컬럼명을 인식할 수 있으며, 미설정 시 c1, c2, ... 형태로 반환됩니다.

---

**Q12.** `INFER_SCHEMA`로 스키마를 감지한 후, `COPY INTO` 시 파일의 컬럼명과 테이블의 컬럼명을 이름 기준으로 매핑하여 순서 차이에 관계없이 정확히 로드하기 위해 사용하는 옵션은?

- A) ON_ERROR = CONTINUE
- B) **MATCH_BY_COLUMN_NAME = CASE_INSENSITIVE** ✅
- C) PURGE = TRUE
- D) FORCE = TRUE

> **해설:** `MATCH_BY_COLUMN_NAME` 옵션을 사용하면 소스 파일의 컬럼과 대상 테이블의 컬럼을 이름 기준으로 매핑합니다. `CASE_INSENSITIVE`로 설정하면 대소문자 구분 없이 매핑되어, `INFER_SCHEMA`로 자동 생성된 테이블에 파일을 로드할 때 발생하는 컬럼 순서 불일치 문제를 해결합니다.

---

**Q13.** 테이블의 스키마가 자주 변경되는 환경에서, 로드되는 소스 파일의 컬럼 추가를 자동으로 감지하여 테이블 구조를 업데이트하는 기능은?

- A) Object Tagging
- B) **Schema Evolution** ✅
- C) Table Cloning
- D) Dynamic Tables

> **해설:** 스키마 진화(Schema Evolution) 기능을 활성화하면 Snowflake가 로드되는 파일의 구조 변화를 감지하여 자동으로 테이블 컬럼을 추가하거나 수정할 수 있습니다.

---

**Q14.** 테이블의 DML 변경 사항(Insert, Delete 등)만 캡처하여 증분(Incremental) ETL 파이프라인을 구축하는 데 사용되는 객체는 무엇입니까?

- A) Task
- B) Materialized View
- C) **Stream** ✅
- D) Dynamic Table

> **해설:** Stream의 주요 역할은 테이블의 변경 사항을 캡처하여 마지막으로 체크한 시점 이후의 '새로운 데이터'만 감지하는 것이며, 이는 증분 파이프라인 구축의 핵심입니다.

---

**Q15.** 특정 태스크(Task)가 대량의 데이터를 처리하던 중 기본 실행 시간 제한에 걸려 실패했습니다. 이 태스크의 실행 제한 시간을 기본값(60분)보다 높게 설정하기 위해 수정해야 할 파라미터는?

- A) TASK_EXECUTION_TIMEOUT_SEC
- B) **USER_TASK_TIMEOUT_MS** ✅
- C) TASK_RUNTIME_LIMIT
- D) USER_TASK_MINIMUM_TRIGGER_INTERVAL_IN_SECONDS

> **해설:** Snowflake 태스크의 기본 제한 시간은 60분(3,600,000ms)입니다. 대규모 배치 작업 시 이 시간을 초과하면 태스크가 실패하므로, `ALTER TASK ... SET USER_TASK_TIMEOUT_MS = <num>` 으로 값을 상향 조정할 수 있습니다. 또한 Warehouse에 `STATEMENT_TIMEOUT_IN_SECONDS`가 설정되어 있으면 두 값 중 **작은 쪽**이 실제 타임아웃으로 적용됩니다.

---

**Q16.** 여러 개의 태스크(Task)가 체인 형태로 연결된 태스크 그래프(Task Graph)에서, 상위 태스크의 성공 여부와 상관없이 모든 작업이 끝난 후 반드시 실행되어야 하는 정리 작업을 설정할 때 사용하는 기능은?

- A) Parent Task
- B) Child Task
- C) **Finalizer Task** ✅
- D) Root Task

> **해설:** 파이널라이저 태스크(Finalizer Task)는 태스크 그래프의 마지막에 위치하여, 이전 단계들의 성공/실패 여부에 관계없이 리소스 정리나 알림 전송 등의 마무리 작업을 수행합니다.

---

**Q17.** 태스크 그래프(Task Graph)에서 스케줄(예: `SCHEDULE = '5 MINUTES'`)에 따라 가장 먼저 실행되며, 하위 자식 태스크(Child Tasks)들의 실행 시작점이 되는 태스크를 무엇이라 합니까?

- A) **Root Task** ✅
- B) Stand-alone Task
- C) Finalizer Task
- D) Trigger Task

> **해설:** Root Task(루트 태스크)는 태스크 그래프의 최상위에 위치하여 정해진 스케줄 또는 조건에 따라 실행됩니다. 루트 태스크가 성공하면 이를 시작점으로 하위 Child Task들이 DAG 종속성에 따라 실행됩니다(독립적인 태스크들은 병렬로 실행될 수 있습니다). Finalizer Task는 그래프 전체가 완료된 뒤 마무리 작업을 수행하며, Root Task와는 다른 역할입니다.

---

**Q18.** 데이터 공유(Data Sharing) 시나리오에서, 데이터 제공자(Provider)가 공유한 데이터를 소비자(Consumer)가 사용하기 위해 가장 먼저 수행해야 할 작업은?

- A) 데이터를 자신의 로컬 테이블로 복사한다.
- B) **제공된 공유(Share) 객체를 바탕으로 데이터베이스를 생성한다.** ✅
- C) 데이터베이스 복제(Replication)를 요청한다.
- D) 가상 웨어하우스를 공유 설정으로 변경한다.

> **해설:** 데이터 소비자는 공유된 객체에 접근하기 위해 `CREATE DATABASE ... FROM SHARE` 구문을 사용하여 자신의 계정에 읽기 전용 데이터베이스를 생성해야 합니다.

---

**Q19.** 데이터 제공자(Provider)가 특정 소비자(Consumer)에게만 데이터를 공유하면서, 소비자의 지역이나 역할에 따라 조회할 수 있는 행(Row)을 제한하고자 할 때 가장 적합한 구현 방식은?

- A) 가상 웨어하우스의 크기를 다르게 할당한다.
- B) **공유할 뷰(Secure View) 내에 로우 레벨 필터링(Row-level filtering) 로직을 적용한다.** ✅
- C) 모든 소비자에게 별도의 태스크(Task)를 생성해 준다.
- D) 데이터를 별도의 물리적 테이블로 복제하여 공유한다.

> **해설:** Snowflake의 데이터 공유 기능을 사용할 때, Secure View와 함께 Row Access Policy 또는 조건부 로직을 사용하여 소비자가 접근할 수 있는 데이터 범위를 행 단위로 정교하게 제어할 수 있습니다.

---

**Q20.** 데이터 공유(Data Sharing) 시 제공자(Provider)가 Row-level filtering을 구현하여 소비자마다 다른 데이터를 보여주고자 할 때 사용하는 가장 핵심적인 객체는?

- A) Materialized View
- B) **Secure View** ✅
- C) Directory Table
- D) Transient Table

> **해설:** 보안 뷰(Secure View)를 사용하면 뷰 내부의 필터링 로직이 소비자에게 노출되지 않으면서도, 사용자의 식별자(Current Role 등)에 따라 행 단위로 데이터를 필터링하여 제공할 수 있습니다.

---

**Q21.** Snowflake Marketplace에서 리스팅(Listing)을 통해 데이터를 공유할 때, 데이터 제공자(Provider)가 특정 소비자의 지역에 상관없이 데이터를 자동으로 가용하게 만드는 기능은?

- A) Data Replication
- B) **Auto-fulfillment** ✅
- C) Cross-Region Sharing
- D) Direct Share

> **해설:** Auto-fulfillment 기능을 사용하면 제공자가 데이터를 한 지역에서 리스팅하더라도 Snowflake가 다른 지역의 소비자에게 데이터를 자동으로 복제하고 동기화하여 지연 없이 공유할 수 있게 합니다.

---

**Q22.** Snowflake 계정이 없는 외부 파트너사에게 데이터를 공유하여 그들이 직접 쿼리하고 결과를 확인하게 하려 할 때, 제공자(Provider)가 생성해 주어야 하는 계정 유형은?

- A) Managed Account
- B) **Reader Account** ✅
- C) Virtual Account
- D) Enterprise Account

> **해설:** Reader Account를 사용하면 Snowflake 계정이 없는 제3자에게 데이터를 공유할 수 있습니다. 이 계정의 컴퓨팅 비용은 데이터를 제공하는 쪽(Provider)이 지불합니다.

---

**Q23.** 데이터 공유(Secure Data Sharing) 시 제공자(Provider)가 공유 객체(Share)에 포함할 **수 없는** 것은?

- A) Secure View / Secure Materialized View
- B) Tables
- C) **Hybrid Tables** ✅
- D) External Tables

> **해설:** Snowflake 공식 문서의 Hybrid Tables 제한사항(Unsupported features)에 따르면 **Hybrid Table은 Data Sharing이 지원되지 않습니다**. 일반 테이블, External Table, Iceberg Table, Secure View/MV, 디렉터리 테이블 등은 `GRANT ... TO SHARE` 구문으로 공유 가능합니다. (참고: 데이터베이스는 `GRANT USAGE ON DATABASE <db> TO SHARE <share>` 구문으로 공유에 포함시킬 수 있으며, 이는 공유의 필수 선행 단계입니다.)

---

**Q24.** 클라우드 스토리지(S3, Azure Blob 등)에 접근할 때, 자격 증명(Credential)을 직접 SQL에 노출하지 않고 Snowflake와 클라우드 공급자 간의 신뢰 관계를 통해 보안 연결을 관리하는 객체는?

- A) API Integration
- B) **Storage Integration** ✅
- C) Security Integration
- D) External Stage

> **해설:** Storage Integration은 클라우드 스토리지에 대한 인증 정보를 안전하게 캡슐화하여, 데이터 엔지니어가 개별 스테이지 생성 시 복잡한 자격 증명을 입력하지 않고도 안전하게 외부 데이터에 접근할 수 있게 합니다.

---

**Q25.** 외부 테이블(External Tables)을 수동으로 관리할 때, 클라우드 스토리지에 새로 추가된 파일을 인식시키기 위해 메타데이터를 강제로 동기화하는 명령은?

- A) ALTER TABLE ... REFRESH
- B) **ALTER EXTERNAL TABLE ... REFRESH** ✅
- C) SYSTEM$PIPE_FORCE_RESUME()
- D) COPY INTO ... FORCE = TRUE

> **해설:** 외부 테이블은 내부 테이블과 달리 메타데이터가 별도로 관리되므로, 자동 알림을 설정하지 않은 경우 `ALTER EXTERNAL TABLE ... REFRESH` 명령을 통해 새로운 파일을 인지시켜야 합니다.

---

### 🟠 2단계: Data Transformation (25%) — Q26 ~ Q55

---

**Q26.** Variant 컬럼에 저장된 중첩된 JSON 배열(Nested Array)을 쿼리하기 쉬운 테이블 형태(Row)로 펼치기 위해 사용하는 함수는?

- A) TRY_PARSE_JSON
- B) OBJECT_CONSTRUCT
- C) **LATERAL FLATTEN** ✅
- D) ARRAY_AGG

> **해설:** `LATERAL FLATTEN` 함수는 JSON 내의 배열이나 오브젝트 구조를 풀어서 각 요소를 별도의 행으로 변환해 줍니다.

---

**Q27.** `SELECT TRY_PARSE_JSON(v) FROM vartab` 쿼리를 실행했을 때, 컬럼 v의 값이 유효하지 않은 JSON 형식인 경우의 결과값은?

- A) 구문 오류(Parsing Error) 발생
- B) **NULL 반환** ✅
- C) 빈 문자열("") 반환
- D) 정의되지 않음(Undefined) 반환

> **해설:** `TRY_PARSE_JSON`은 일반 `PARSE_JSON`과 달리 오류 발생 시 쿼리를 중단시키지 않고 단순히 `NULL`을 반환하여 파이프라인의 안정성을 높입니다.

---

**Q28.** 데이터 파이프라인에서 `TRY_PARSE_JSON` 함수를 사용하는 주된 이유는 무엇입니까?

- A) JSON 파싱 속도를 2배 이상 높이기 위해
- B) **잘못된 JSON 형식이 들어와도 쿼리 전체가 실패하지 않고 NULL을 반환하게 하기 위해** ✅
- C) JSON 데이터를 자동으로 테이블 형태로 변환하기 위해
- D) 외부 API와의 통신을 최적화하기 위해

> **해설:** 일반 `PARSE_JSON`은 잘못된 형식의 데이터 처리 시 오류를 내며 멈추지만, `TRY_PARSE_JSON`은 NULL을 반환하고 계속 진행되므로 파이프라인의 견고함을 높여줍니다.

---

**Q29.** 기존의 정적 테이블과 달리, 정의된 쿼리에 따라 데이터를 자동으로 업데이트하며 결과값을 지속적으로 구체화(Materialize)하여 제공하는 객체는?

- A) External Table
- B) **Dynamic Table** ✅
- C) Transient Table
- D) Secure View

> **해설:** 다이내믹 테이블(Dynamic Table)은 데이터 파이프라인을 간소화하기 위해 도입된 기능으로, 사용자가 타겟 상태를 정의하면 시스템이 자동으로 변경 사항을 감지하여 데이터를 동기화합니다.

---

**Q30.** Snowflake 쿼리 내에서 외부 ML 모델을 호출하거나 제3자 API 서비스를 통해 데이터를 보강(Enrichment)하려 할 때 사용하는 기능은?

- A) Snowpark Stored Procedure
- B) **External Function** ✅
- C) Python Connector
- D) JavaScript UDF

> **해설:** 외부 함수(External Function)는 Snowflake 외부의 API 엔드포인트(AWS Lambda 등)와 통신할 수 있는 보안 브리지를 제공하여 외부 처리 능력을 통합합니다.

---

**Q31.** 외부 함수(External Functions)를 생성할 때, Snowflake와 외부 엔드포인트(예: AWS Lambda) 간의 보안 통신 및 인증 정보를 저장하기 위해 반드시 먼저 생성해야 하는 객체는?

- A) **API Integration** ✅
- B) Storage Integration
- C) Security Integration
- D) External Stage

> **해설:** 외부 함수는 외부 API와의 신뢰 관계를 설정하기 위해 API Integration 객체를 필요로 합니다. 이는 보안 자격 증명과 엔드포인트 허용 목록을 관리하는 역할을 합니다.

---

**Q32.** 다양한 외부 SaaS 소스(예: Kafka, PostgreSQL, Slack, Google Ads 등)에서 Snowflake로 데이터를 인입할 수 있도록 Snowflake가 관리하는 서비스 형태로 제공되는 통합 데이터 인제스천 플랫폼은?

- A) Snowpipe Streaming
- B) External Function
- C) **Openflow** ✅
- D) Data Marketplace

> **해설:** Openflow는 Apache NiFi 기반의 Snowflake-managed 데이터 인제스천 서비스로, 커넥터 기반의 코드리스 파이프라인으로 다양한 외부 소스에서 Snowflake로 데이터를 실시간/배치로 로드할 수 있게 해줍니다. `OPENFLOW_USAGE_HISTORY` 뷰로 사용량을 추적할 수 있습니다.

---

**Q33.** UDF(사용자 정의 함수)를 생성할 때, 함수 내부의 비즈니스 로직이나 SQL 구문을 다른 사용자(함수 호출자)가 볼 수 없도록 보호하기 위해 설정해야 하는 옵션은?

- A) IMMUTABLE
- B) VOLATILE
- C) **SECURE** ✅
- D) PRIVATE

> **해설:** SECURE 옵션이 적용된 UDF는 함수의 정의(로직)가 `GET_DDL` 등의 명령으로 노출되지 않으며, 최적화 과정에서도 보안을 우선시하여 데이터 유출 위험을 줄입니다.

---

**Q34.** Snowpark 저장 프로시저(Stored Procedure)와 사용자 정의 함수(UDF)의 주요 차이점은?

- A) UDF는 반드시 하나의 값만 반환해야 하지만, 저장 프로시저는 값을 반환하지 않을 수 있다.
- B) **저장 프로시저는 SQL 문을 직접 실행하고 트랜잭션을 제어할 수 있지만, UDF는 계산된 결과값만 반환하고 DML을 실행할 수 없다.** ✅
- C) 저장 프로시저는 오직 SQL로만 작성 가능하며, UDF는 Python으로 작성 가능하다.
- D) 둘 다 항상 동일한 수준의 트랜잭션 제어 능력을 갖는다.

> **해설:** 저장 프로시저는 데이터베이스 내에서 비즈니스 로직을 수행하고 트랜잭션(Commit/Rollback)을 관리하는 데 최적화되어 있는 반면, UDF는 쿼리 내에서 특정 값을 계산하거나 변환하여 반환하는 목적으로 사용됩니다.

---

**Q35.** 저장 프로시저(Stored Procedures)를 작성할 때, 프로시저를 실행하는 사용자의 권한이 아닌 '프로시저를 생성한 사람'의 권한으로 실행되도록 설정하는 옵션은?

- A) EXECUTE AS CALLER
- B) **EXECUTE AS OWNER** ✅
- C) EXECUTE AS SYSTEM
- D) EXECUTE AS ADMIN

> **해설:** `EXECUTE AS OWNER`로 설정된 프로시저는 호출자의 권한과 상관없이 생성자(Owner)가 가진 권한을 사용하여 작업을 수행합니다. 이는 보안 통제 하에 특정 작업만 허용할 때 유용합니다.

---

**Q36.** Snowflake 저장 프로시저 내에서 여러 개의 DML 작업을 하나의 논리적 단위로 묶어, 하나라도 실패하면 전체를 취소(Rollback)하도록 관리하는 기능은?

- A) Result Cache
- B) **Transaction Management (BEGIN/COMMIT/ROLLBACK)** ✅
- C) Multi-cluster Warehouse
- D) Task Graph

> **해설:** 저장 프로시저는 트랜잭션(Transaction) 제어 능력을 갖추고 있어, 복잡한 데이터 변환 과정에서 데이터의 일관성을 보장할 수 있습니다.

---

**Q37.** Snowpark에서 하나의 입력 행에 대해 여러 개의 결과 행을 반환해야 하는 복잡한 로직(예: 텍스트 토큰화)을 구현할 때 가장 적합한 함수 유형은?

- A) Scalar UDF
- B) **User-Defined Table Function (UDTF)** ✅
- C) Stored Procedure
- D) User-Defined Aggregate Function (UDAF)

> **해설:** UDTF(사용자 정의 테이블 함수)는 테이블 형태의 결과를 반환할 수 있으므로, 행 하나를 여러 행으로 확장하거나 복잡한 구조를 펼치는 작업에 최적화되어 있습니다.

---

**Q38.** Snowpark를 사용하여 대규모 메모리 집약적인 데이터 변환 작업(예: 복잡한 머신러닝 전처리)을 수행할 때, 성능 병목 현상을 방지하기 위해 선택해야 할 가상 웨어하우스 유형은?

- A) Standard Virtual Warehouse
- B) Multi-cluster Warehouse
- C) **Snowpark-optimized Virtual Warehouse** ✅
- D) Large Warehouse with Query Acceleration Service

> **해설:** Snowpark 최적화 웨어하우스는 일반 웨어하우스보다 노드당 훨씬 더 많은 메모리를 제공하므로, 메모리 소모가 큰 Python/Java/Scala 기반의 Snowpark 작업에 필수적입니다.

---

**Q39.** Snowpark에서 데이터 변환 로직을 작성할 때, 클라이언트 측에서 작성한 Python 코드가 Snowflake 서버에서 실행되는 방식에 대한 설명으로 옳은 것은?

- A) Python 코드가 Snowflake 내부의 가상 머신에서 직접 실행된다.
- B) **DataFrame 작업이 자동으로 최적화된 SQL 구문으로 변환되어 Snowflake 엔진에서 실행된다.** ✅
- C) 모든 데이터를 클라이언트 메모리로 로드한 후 처리한다.
- D) 외부 함수(External Function)를 통해서만 실행 가능하다.

> **해설:** Snowpark의 핵심 아키텍처는 개발자가 익숙한 언어(Python 등)로 작성한 데이터프레임 작업을 SQL로 변환하여 Snowflake의 강력한 컴퓨팅 엔진에서 실행되도록 하는 것입니다.

---

**Q40.** Snowpark Python 라이브러리를 사용하여 개발을 진행할 때, 클라이언트 측의 DataFrame 변환 작업이 즉시 실행되지 않고 `.collect()`나 `.show()`와 같은 'Action' 함수가 호출될 때까지 대기하는 방식을 무엇이라 합니까?

- A) Instant Execution
- B) **Lazy Evaluation (지연 평가)** ✅
- C) Batch Processing
- D) Server-side Pushing

> **해설:** Snowpark는 Lazy Evaluation 방식을 사용합니다. 이는 모든 변환 단계를 미리 SQL로 최적화하여 준비해 두었다가, 실제 데이터 결과가 필요한 시점에만 Snowflake 서버에서 한꺼번에 실행하여 효율성을 극대화합니다.

---

**Q41.** Snowpark Python 라이브러리에서 DataFrame 변환 작업이 `Lazy Evaluation` 방식으로 수행될 때, 실제로 쿼리를 실행하는 'Action' 함수는?

- A) .alias()
- B) **`.collect()`** ✅
- C) .with_column()
- D) .limit()

> **해설:** Snowpark는 지연 평가(Lazy Evaluation)를 수행하므로, `.collect()`, `.show()`, `.count()`와 같은 액션 함수가 호출될 때까지 실제 쿼리는 실행되지 않습니다.

---

**Q42.** Snowpark Python에서 여러 행(Row)을 입력받아 단일 집계값(예: 중앙값, 커스텀 통계)을 반환하는 사용자 정의 함수 유형은?

- A) Scalar UDF
- B) UDTF (User-Defined Table Function)
- C) **UDAF (User-Defined Aggregate Function)** ✅
- D) Stored Procedure

> **해설:** UDAF(사용자 정의 집계 함수)는 그룹 내 여러 행을 입력받아 단일 집계값을 반환합니다. SQL의 `SUM`, `AVG`와 같은 내장 집계 함수로 구현하기 어려운 커스텀 집계 로직(예: 중앙값, 가중 평균)을 구현할 때 사용합니다. Scalar UDF는 1행 → 1값, UDTF는 1행 → 여러 행, Stored Procedure는 DML 실행 및 트랜잭션 제어가 목적입니다.

---

**Q43.** Snowpark Python을 사용하여 두 DataFrame을 조인(Join)한 후, 양쪽 테이블에 동일한 이름의 컬럼이 존재하여 충돌이 발생했습니다. 이를 해결하기 위한 가장 권장되는 방식은?

- A) SQL 문으로 다시 작성한다.
- B) **조인 전에 `alias()` 또는 `with_column_renamed()`를 사용하여 컬럼 이름을 명확히 구분한다.** ✅
- C) Snowflake가 자동으로 이름을 바꿀 때까지 기다린다.
- D) 중복된 컬럼 중 하나를 수동으로 삭제(Drop)한다.

> **해설:** Snowpark에서는 DataFrame 조인 시 동일 이름의 컬럼이 있으면 모호성 오류가 발생할 수 있습니다. 조인 수행 전이나 직후에 컬럼명을 명시적으로 변경하여 관리하는 것이 중요합니다.

---

**Q44.** Snowpark DataFrame에서 데이터를 필터링할 때, SQL의 WHERE 절과 동일한 역할을 수행하며 체인 형태로 연결 가능한 함수는?

- A) .select()
- B) **`.filter()` 또는 `.where()`** ✅
- C) .group_by()
- D) .agg()

> **해설:** Snowpark API에서 `.filter()`와 `.where()`는 완전히 동일하게 작동하며, 조건에 맞는 행만 남기는 데이터프레임 변환 단계에서 사용됩니다.

---

**Q45.** 비정형 데이터 처리를 위해 Snowpark Java 또는 Python UDF를 개발할 때, 스테이지에 저장된 실제 파일에 보안을 유지하며 접근하기 위해 사용되는 URL 유형은?

- A) Public URL
- B) **Scoped URL** ✅
- C) Constant URL
- D) Static Link

> **해설:** 범위 지정 URL(Scoped URL)은 현재 세션 내에서만 유효한 임시 링크를 생성하여, 비정형 데이터에 대한 접근 제어와 보안을 동시에 강화하는 데 사용됩니다.

---

**Q46.** 비정형 데이터를 처리할 때 사용하는 URL 유형 중, `GET_PRESIGNED_URL()` 함수로 생성하며 만료 시간(Expiration Time)을 설정할 수 있고 Snowflake에 인증 없이도 URL만으로 파일에 직접 접근 가능한 URL 유형은?

- A) **Pre-signed URL** ✅
- B) Scoped URL
- C) File URL
- D) Stage URL

> **해설:** Snowflake는 세 가지 파일 URL 유형을 제공합니다. ① **Pre-signed URL**: `GET_PRESIGNED_URL()`로 생성, 만료 시간 설정 가능, Snowflake 인증 없이 누구든 접근 가능 (BI 도구, 외부 공유용). ② **Scoped URL**: `BUILD_SCOPED_FILE_URL()`로 생성, 쿼리 결과 캐시(24시간) 동안 유효, 생성한 사용자만 접근 가능. ③ **File URL**: `BUILD_STAGE_FILE_URL()`로 생성, 영구적이며 스테이지에 권한이 있는 역할만 접근 가능. ⚠️ 'Pre-scoped URL'은 공식 Snowflake 용어가 아닙니다.

---

**Q47.** 스테이지에 저장된 비정형 데이터(이미지, 문서 등)의 파일 경로를 기반으로, 외부 애플리케이션이나 Snowpark UDF에서 해당 파일에 직접 접근할 수 있는 'Scoped URL'을 생성하는 함수는?

- A) GET_STAGE_LOCATION()
- B) **BUILD_SCOPED_FILE_URL()** ✅
- C) GET_PRESIGNED_URL()
- D) SYSTEM$FILE_ACCESS_URL()

> **해설:** `BUILD_SCOPED_FILE_URL()` 함수는 현재 세션 내에서만 유효한 보안 URL을 생성하여, 비정형 데이터를 처리하는 로직에 안전하게 파일을 전달합니다.

---

**Q48.** Snowflake Cortex에서 제공하는 AI 기능을 SQL 내에서 활용하여 제품 리뷰 텍스트의 요약본을 즉시 생성하고 싶을 때 사용하는 AI 함수는?

- A) CORTEX.SENTIMENT()
- B) **SNOWFLAKE.CORTEX.SUMMARIZE()** ✅
- C) CORTEX.TRANSLATE()
- D) SNOWFLAKE.AI_EXTRACT()

> **해설:** `SNOWFLAKE.CORTEX.SUMMARIZE()` 함수는 대규모 언어 모델(LLM)을 사용하여 긴 텍스트 데이터를 짧고 명확하게 요약해 주며, SQL 쿼리 내에서 직접 호출이 가능합니다.

---

**Q49.** 자연어 질문을 의미 모델(Semantic Model/Semantic View) 기반으로 SQL로 변환하여 비즈니스 사용자에게 답변하는 Snowflake Cortex 서비스는?

- A) Cortex Search
- B) **Cortex Analyst** ✅
- C) Cortex Complete
- D) Document AI

> **해설:** Cortex Analyst는 YAML 기반의 Semantic Model 또는 Semantic View를 활용하여 자연어를 검증된 SQL로 변환하는 Text-to-SQL 서비스입니다. Cortex Search는 하이브리드(벡터+키워드) 검색 서비스로 RAG 워크로드에 사용됩니다.

---

**Q50.** Snowflake Cortex에서 제공하는 AI 기능을 활용하여 수천 개의 제품 리뷰 텍스트 데이터의 감성(Positive/Negative)을 분석하고 요약하고자 할 때 가장 적합한 도구는?

- A) Search Optimization Service
- B) **Cortex LLM 및 AI 워크플로우** ✅
- C) Snowpark JavaScript UDF
- D) External Function to Google AI API

> **해설:** Snowflake Cortex는 SQL 내에서 직접 호출 가능한 거대언어모델(LLM) 기능을 제공하여, 외부 API 호출 없이도 텍스트 요약, 감성 분석, 번역 등의 작업을 안전하게 처리할 수 있게 해줍니다.

---

**Q51.** Snowflake Cortex에서 SQL 내에서 직접 지정한 LLM 모델을 호출하여 사용자 정의 프롬프트에 대한 응답을 생성하는 함수는?

- A) SNOWFLAKE.CORTEX.SUMMARIZE()
- B) SNOWFLAKE.CORTEX.SENTIMENT()
- C) **SNOWFLAKE.CORTEX.COMPLETE()** ✅
- D) SNOWFLAKE.CORTEX.TRANSLATE()

> **해설:** `SNOWFLAKE.CORTEX.COMPLETE(model, prompt)` 함수는 사용자가 모델명(예: `'mistral-7b'`, `'llama3-8b'`)과 프롬프트를 직접 지정하여 LLM을 호출할 수 있는 범용 Completion 함수입니다. SUMMARIZE, SENTIMENT, TRANSLATE는 특정 목적에 맞게 사전 구성된 함수인 반면, COMPLETE는 자유로운 형식의 프롬프트 엔지니어링이 가능합니다.

---

**Q52.** Snowflake Cortex를 활용하여 텍스트 데이터를 분석할 때, 문장의 의미적 유사성을 기반으로 데이터를 검색하거나 분류하는 기술은?

- A) Keyword Searching
- B) **Semantic Data Analysis (의미론적 분석)** ✅
- C) Regular Expression Matching
- D) Binary Extraction

> **해설:** Snowflake Cortex는 AI 기반의 의미론적 분석(Semantic Analysis) 기능을 제공하여, 단순 키워드 매칭을 넘어 텍스트의 맥락과 의미를 이해하고 처리할 수 있게 지원합니다.

---

**Q53.** Snowflake 내에서 데이터 모델링 및 변환 작업을 수행할 때, dbt(data build tool) 프로젝트를 직접 관리하고 실행하여 SQL 기반의 데이터 파이프라인 가시성을 높이는 기능은?

- A) Snowsight Workspaces
- B) **Snowflake dbt Projects management** ✅
- C) Git Integration
- D) Task Graphs

> **해설:** Snowflake는 dbt 프로젝트 관리 기능을 내장하여, 외부 툴 없이도 데이터 엔지니어가 변환 로직의 종속성을 관리하고 문서화하며 실행할 수 있도록 지원합니다.

---

**Q54.** Snowpark 또는 SQL Scripting을 사용하여 작성한 복잡한 데이터 파이프라인 코드를 버전 관리 시스템과 연동하여 관리하고 배포하기 위해 제공되는 Snowflake의 기능은?

- A) Snowsight Workspaces
- B) **Git Integration (Git 연동)** ✅
- C) Snowflake dbt Projects
- D) Automatic Clustering

> **해설:** Snowflake는 Git 리포지토리와의 직접 연동을 지원하여, 코드를 직접 업로드하지 않고도 버전 관리 시스템에서 최신 스크립트를 가져와 파이프라인을 운영할 수 있게 합니다.

---

**Q55.** Snowflake Notebooks(Snowsight Notebook)의 실행 환경으로 **Container Runtime**을 선택했을 때의 주요 이점은?

- A) 크레딧이 전혀 소비되지 않는다.
- B) **GPU를 포함한 Snowpark Container Services 컴퓨팅에서 실행되어 ML/딥러닝 워크로드 및 오픈소스 파이썬 패키지를 자유롭게 사용할 수 있다.** ✅
- C) SQL 셀은 실행되지 않고 Python 셀만 지원한다.
- D) 세션 종료 시 노트북 파일이 자동 삭제된다.

> **해설:** Notebooks의 Container Runtime은 Snowpark Container Services(SPCS) 기반으로 동작하여 CPU/GPU 컴퓨트 풀을 활용할 수 있고, pip 설치를 통한 임의의 오픈소스 패키지 사용이 가능합니다. 반면 Warehouse Runtime은 일반 웨어하우스에서 실행되며 Anaconda 채널 패키지로 제한됩니다.

---

### 🟢 3단계: Performance Optimization (19%) — Q56 ~ Q70

---

**Q56.** 기본 데이터가 변경되지 않은 상태에서 동일한 쿼리가 반복 실행될 때, 컴퓨팅 비용을 쓰지 않고 즉시 결과를 제공하는 기능은?

- A) Metadata Cache
- B) **Result Cache** ✅
- C) Warehouse Cache
- D) Query Acceleration Service

> **해설:** Result Cache는 이전에 실행된 쿼리의 결과를 저장해 두었다가, 데이터 변경이 없다면 가상 웨어하우스를 가동하지 않고 즉시 결과를 서빙하여 비용을 절감합니다.

---

**Q57.** Materialized View(MVs)가 자동으로 갱신을 중단(Suspended)하거나 무효화되는 상황은 언제입니까? **(2개 선택)**

- A) 베이스 테이블에 새로운 데이터가 삽입될 때
- B) **베이스 테이블의 이름을 변경(Rename)했을 때** ✅
- C) **베이스 테이블의 컬럼이 삭제(DROP)되었을 때** ✅
- D) 베이스 테이블에서 데이터를 삭제했을 때

> **해설:** Materialized View는 기반 테이블에 INSERT/UPDATE/DELETE가 발생하면 정상적으로 refresh됩니다. 그러나 ① 베이스 테이블 이름 변경(ALTER TABLE … RENAME) 같은 DDL 작업이나 ② 베이스 테이블의 컬럼이 삭제(DROP)/변경될 때는 MV가 suspended 상태가 되어 자동 갱신이 중단됩니다. (참고: 비결정적 함수 제한은 MV 정의(CREATE MV) 시에 적용되는 제약이며, 베이스 테이블 DML과는 무관합니다.) A)와 D)는 정상적인 DML로 MV가 자동 refresh됩니다.

---

**Q58.** 대시보드 쿼리 속도가 갑자기 느려졌을 때, 데이터 엔지니어가 가장 먼저 확인해야 할 '주요 용의자'가 **아닌** 것은?

- A) 과도한 SELECT * 사용
- B) 클러스터링(Clustering) 부족
- C) **Result Cache 활성화 여부** ✅
- D) 부적절한 가상 웨어하우스 크기

> **해설:** SELECT *는 불필요한 데이터를 모두 스캔하게 만들며, 클러스터링이 없으면 파티션 프루닝이 비효율적으로 일어납니다. Result Cache는 성능 저하의 원인이 아니라 **개선 도구**입니다.

---

**Q59.** 대규모 데이터 세트에서 복잡한 조인(Join)이나 집계 작업을 수행할 때, 쿼리가 메모리 부족으로 인해 로컬 디스크로 스필(Spilling)되는 현상을 해결하기 위한 가장 적합한 웨어하우스 전략은?

- A) 멀티 클러스터 웨어하우스를 활성화하여 스케일 아웃(Scale-out)한다.
- B) **가상 웨어하우스의 크기를 늘려(Scale-up) 더 많은 메모리와 CPU를 확보한다.** ✅
- C) Result Cache를 비활성화하여 리소스를 확보한다.
- D) Snowpipe를 사용하여 데이터를 다시 로드한다.

> **해설:** 디스크 스필링은 주로 메모리 부족으로 발생합니다. 스케일 아웃(A)은 동시성(Concurrency) 문제 해결에 좋으나, 단일 쿼리의 리소스 부족은 웨어하우스 사이즈를 키우는 **스케일 업(B)**으로 해결해야 합니다.

---

**Q60.** 가상 웨어하우스에서 복잡한 쿼리를 처리할 때, 가용한 메모리(RAM)를 초과하여 로컬 디스크나 원격 스토리지(Cloud Storage)를 임시 공간으로 사용하게 되는 현상을 무엇이라 합니까?

- A) Cloud Bursting
- B) **Disk Spilling (Local/Remote)** ✅
- C) Partition Pruning
- D) Resource Monitoring

> **해설:** 메모리가 부족할 때 데이터를 디스크로 옮겨 처리하는 것을 Disk Spilling이라고 합니다. 로컬 디스크 스필링보다 원격 스토리지(Remote)로의 스필링이 발생할 때 쿼리 속도가 훨씬 더 느려집니다.

---

**Q61.** 테이블의 클러스터링 상태를 진단하고, 마이크로 파티션의 겹침(Overlap) 정도를 수치화하여 클러스터링 키 변경 여부를 판단할 때 사용하는 시스템 함수는?

- A) SYSTEM$PIPE_STATUS
- B) SYSTEM$EXPLAIN_PLAN_JSON
- C) **SYSTEM$CLUSTERING_DEPTH** ✅
- D) SYSTEM$WHITELIST

> **해설:** `SYSTEM$CLUSTERING_DEPTH`는 테이블의 클러스터링 효율성을 측정하며, 이 수치가 낮을수록 파티션 프루닝이 잘 이루어지고 있음을 의미합니다. 이론적 최솟값은 **1.0(모든 파티션이 겹치지 않는 완벽한 정렬 상태)**이며, 수치가 높아질수록 파티션 간 데이터 겹침(Overlap)이 심해 스캔 범위가 커짐을 뜻합니다.

---

**Q62.** 매우 큰 테이블에서 특정 ID나 키 값을 찾는 쿼리의 성능을 획기적으로 개선하고 싶지만, 클러스터링 키를 변경하기 어려운 상황에서 고려할 수 있는 최적화 서비스는?

- A) Query Acceleration Service
- B) Materialized Views
- C) **Search Optimization Service** ✅
- D) Resource Monitor

> **해설:** 검색 최적화(Search Optimization) 서비스는 대규모 테이블에서 특정 값을 찾는 점 검색(Point Lookup) 쿼리의 성능을 클러스터링 변경 없이도 개선해 줍니다.

---

**Q63.** 매우 큰 테이블(수 TB 이상)에서 수백만 개의 행 중 단 몇 개의 행만 필터링하는 '점 검색(Point Lookup)' 쿼리의 성능을 개선하기 위해 Search Optimization Service가 가장 효과를 발휘하는 컬럼 타입은?

- A) 자주 변경되는 카운트 컬럼
- B) **고유값(High Cardinality)이 많은 문자열이나 숫자 컬럼** ✅
- C) 2개 이상의 값이 반복되는 불리언(Boolean) 컬럼
- D) 데이터가 정렬되지 않은 바이너리 컬럼

> **해설:** Search Optimization Service는 Cardinality가 높은(중복이 적은) 컬럼에서 특정 값을 빠르게 찾는 작업에 가장 최적화되어 있습니다.

---

**Q64.** 동시 사용자가 급증하여 쿼리가 큐(Queue)에 대기하는 동시성(Concurrency) 문제를 해결하기 위해, 클러스터를 자동으로 추가하여 동시 처리 용량을 늘리는 Snowflake 기능은?

- A) Search Optimization Service
- B) Query Acceleration Service
- C) **Multi-cluster Warehouse** ✅
- D) Automatic Clustering

> **해설:** 동시성 문제(Concurrency) — 많은 사용자가 동시에 쿼리를 실행하여 큐 대기가 발생하는 경우 — 는 **Multi-cluster Warehouse**로 해결합니다. Query Acceleration Service(QAS)는 평균 대비 스캔 파티션 수가 많은 'outlier 쿼리'의 scan 작업 일부를 shared compute로 오프로드하여 가속화하는 서비스로, 동시성 문제가 아닌 **단일 쿼리 성능 개선**에 사용됩니다. 모든 쿼리에 적용되는 것이 아니라 Snowflake가 자동으로 outlier 여부를 판단합니다.

---

**Q65.** 멀티 클러스터 웨어하우스(Multi-cluster Warehouse)를 운영할 때, 성능(Concurrency)을 최우선시하여 쿼리가 큐에 쌓이면 즉시 새 클러스터를 시작하는 스케일링 정책은?

- A) **Standard Scaling Policy** ✅
- B) Economy Scaling Policy
- C) Optimized Scaling Policy
- D) Static Scaling Policy

> **해설:** **Standard** 정책은 쿼리가 대기(Queued) 상태가 되면 즉시 새 클러스터를 추가하여 대기 시간을 최소화합니다. 반면 **Economy** 정책은 비용 절감을 우선시하여, 클러스터가 일정 시간 이상 완전히 가득 차 있을 것으로 예상될 때만 새 클러스터를 시작합니다.

---

**Q66.** 멀티 클러스터 웨어하우스 운영 시, 크레딧 절감을 우선시하여 클러스터가 최대한 활용되고 있을 때에만 새로운 클러스터를 시작하도록 제어하는 스케일링 정책(Scaling Policy)은?

- A) Standard
- B) **Economy** ✅
- C) Optimized
- D) Conservative

> **해설:** 스케일링 정책은 두 가지입니다. ① **Standard(기본값)**: 쿼리 대기가 감지되면 즉시 클러스터를 추가 → 성능 우선. ② **Economy**: 클러스터가 최대한 가득 차 있을 것으로 예상될 때만 새 클러스터를 시작 → 비용 우선. Economy 정책은 약간의 대기 시간을 감수하더라도 불필요한 클러스터 기동을 줄여 크레딧을 절약합니다.

---

**Q67.** 리소스 모니터(Resource Monitor)를 설정할 때, 할당된 크레딧 할당량의 100%에 도달했을 때 'Suspend Immediate' 액션을 선택하면 어떤 현상이 발생합니까?

- A) 현재 실행 중인 모든 쿼리가 완료될 때까지 기다린 후 웨어하우스를 중지한다.
- B) **실행 중인 쿼리를 즉시 중단시키고 새로운 쿼리 접수를 거부하며 웨어하우스를 즉각 중지한다.** ✅
- C) 다음 달 크레딧이 충전될 때까지 계정 전체가 잠긴다.
- D) 관리자에게 알림만 보내고 웨어하우스는 계속 작동한다.

> **해설:** 'Suspend Immediate'는 크레딧 낭비를 막기 위해 진행 중인 작업까지 모두 취소하고 즉시 중단시키는 가장 강력한 조치입니다.

---

**Q68.** 리소스 모니터(Resource Monitor)를 설정할 때, 할당된 크레딧의 80%를 소비했을 때 관리자에게 이메일 알림만 보내고 서비스는 중단하지 않도록 하려면 어떤 액션을 지정해야 합니까?

- A) Suspend
- B) Suspend Immediate
- C) **Notify Only** ✅
- D) Alert Only

> **해설:** 리소스 모니터의 임계값 설정 시 Notify 액션을 선택하면 서비스 중단 없이 예산 소비 현황을 모니터링할 수 있는 알림만 트리거됩니다.

---

**Q69.** 가상 웨어하우스의 크레딧 소비를 모니터링할 때, 최대 45분~3시간의 지연은 있지만 훨씬 더 긴(365일) 이력을 제공하는 뷰가 포함된 스키마는?

- A) PUBLIC
- B) INFORMATION_SCHEMA
- C) **ACCOUNT_USAGE** ✅
- D) MONITORING_SCHEMA

> **해설:** `ACCOUNT_USAGE` 스키마의 뷰들은 보관 기간이 길어(최대 1년) 장기적인 비용 분석과 감사에 적합하지만, 데이터가 나타나기까지 약간의 지연 시간이 있습니다.

---

**Q70.** 가상 웨어하우스의 크레딧 소비 이력을 분석할 때, `ACCOUNT_USAGE` 뷰와 `INFORMATION_SCHEMA` 뷰의 가장 큰 차이점은?

- A) INFORMATION_SCHEMA는 최대 1년간의 데이터를 보관한다.
- B) **ACCOUNT_USAGE는 1년(365일)의 이력을 제공하지만 데이터 반영에 지연(45분~3시간)이 있고, INFORMATION_SCHEMA는 지연 없이 실시간에 가깝지만 보관 기간이 짧다(뷰별 7일~6개월).** ✅
- C) ACCOUNT_USAGE는 삭제된 객체의 정보를 포함하지 않는다.
- D) 성능 튜닝에는 오직 ACCOUNT_USAGE만 사용해야 한다.

> **해설:** `ACCOUNT_USAGE`는 장기 분석(1년)에 적합하며 **삭제된 객체도 포함**하지만 반영 지연이 있습니다. `INFORMATION_SCHEMA`는 지연은 없지만 보존 기간이 뷰마다 다릅니다(`QUERY_HISTORY` 7일 또는 10,000행, `LOAD_HISTORY` 14일, `DATABASE_STORAGE_USAGE_HISTORY` 6개월 등).

---

### 🔴 4단계: Data Governance (14%) — Q71 ~ Q85

---

**Q71.** 특정 역할(Role)을 가진 사용자에게만 전화번호 컬럼의 뒷자리를 마스킹하여 보여주고자 할 때, 가장 먼저 구현해야 할 Snowflake 거버넌스 기능은?

- A) Row Access Policy
- B) **Dynamic Data Masking** ✅
- C) Object Tagging
- D) Data Classification

> **해설:** Dynamic Data Masking은 컬럼 레벨 보안 기능으로, 정책에 따라 민감한 데이터를 실시간으로 가리거나 변환하여 출력하는 데 사용됩니다.

---

**Q72.** 특정 역할(Role)에 따라 테이블의 행(Row) 단위로 접근 권한을 제어하고 싶을 때 사용하는 정책은?

- A) Dynamic Data Masking
- B) **Row Access Policy** ✅
- C) Object Tagging
- D) Projection Policy

> **해설:** Row Access Policy는 사용자의 권한에 따라 볼 수 있는 행을 필터링하여 행 단위 보안을 구현합니다.

---

**Q73.** 데이터 거버넌스 정책 중, 사용자가 `SELECT *`와 같은 쿼리를 통해 특정 컬럼의 전체 데이터를 무분별하게 추출하는 것을 방지하기 위해 사용되는 기능은?

- A) Dynamic Data Masking
- B) Row Access Policy
- C) **Projection Policy** ✅
- D) Aggregation Policy

> **해설:** 프로젝션 정책(Projection Policy)은 사용자가 특정 컬럼을 개별적으로 조회하거나 전체 출력에 포함시키는 것을 제한하여, 데이터 유출 위험을 줄이는 거버넌스 기능입니다.

---

**Q74.** 데이터 거버넌스 정책 중, 사용자가 개별 행을 직접 쿼리하는 것은 제한하되 그룹화된 통계 결과(예: 평균, 합계)만 조회할 수 있도록 강제하는 정책은?

- A) Row Access Policy
- B) Dynamic Data Masking
- C) **Aggregation Policy** ✅
- D) Projection Policy

> **해설:** 집계 정책(Aggregation Policy)은 개인 정보 보호를 위해 데이터 세트에서 개별 레코드의 세부 정보를 노출하지 않고 요약된 통계치만 제공하도록 제어할 때 사용됩니다.

---

**Q75.** Row Access Policy가 테이블에 적용된 상태에서, 정책 조건을 충족하지 않는 사용자가 해당 테이블을 쿼리하면 어떤 결과가 반환됩니까?

- A) 쿼리 전체가 오류로 실패한다.
- B) NULL 값이 포함된 행만 반환된다.
- C) **정책 조건을 충족하는 행만 필터링되어 반환된다(조건을 충족하지 않는 행은 숨겨짐).** ✅
- D) 테이블에 접근 권한이 없다는 오류(Access Denied)가 발생한다.

> **해설:** Row Access Policy는 보이지 않는 필터처럼 작동합니다. 정책이 적용된 테이블을 쿼리하면 Snowflake가 자동으로 정책 조건을 적용하여 허용된 행만 결과에 포함됩니다. 사용자는 다른 행이 존재하는지조차 알 수 없으므로, 지역별 담당자 데이터 격리와 같은 행 단위 보안 구현에 적합합니다.

---

**Q76.** 태그 기반 마스킹 정책(Tag-based Masking Policy)을 구성할 때, 기존에 생성된 마스킹 정책을 특정 태그에 연결하는 올바른 SQL 명령은?

- A) `CREATE TAG sensitive WITH MASKING POLICY phone_mask;`
- B) `GRANT MASKING POLICY phone_mask TO TAG sensitive;`
- C) **`ALTER TAG sensitive SET MASKING POLICY phone_mask;`** ✅
- D) `ATTACH MASKING POLICY phone_mask ON TAG sensitive;`

> **해설:** Snowflake에서 마스킹 정책을 태그에 연결하려면 `ALTER TAG <tag_name> SET MASKING POLICY <policy_name>` 구문을 사용합니다. 이 설정 이후 해당 태그가 부여된 모든 컬럼에 마스킹 정책이 자동으로 적용되어, 수백 개의 컬럼에 개별적으로 정책을 설정하는 번거로움 없이 일괄 적용할 수 있습니다.

---

**Q77.** 특정 태그(예: Level = 'Critical')가 부여된 모든 테이블 컬럼에 대해 일괄적으로 마스킹 정책을 적용하여, 개별 컬럼마다 정책을 수동으로 설정하는 번거로움을 줄여주는 기능은?

- A) Dynamic Data Masking
- B) **Tag-based Masking Policy (태그 기반 마스킹)** ✅
- C) Row Access Policy
- D) Object Classification

> **해설:** 태그 기반 마스킹은 거버넌스 관리의 효율성을 획기적으로 높여주는 기능으로, 특정 태그가 지정된 모든 객체에 보안 정책을 자동으로 상속시켜 적용합니다.

---

**Q78.** 계정 내의 수많은 테이블 중 민감한 정보가 포함된 객체를 모니터링하고 관리하기 위해 가장 먼저 적용해야 할 기능은?

- A) Data Cloning
- B) **Object Tagging 및 Classification** ✅
- C) Resource Monitor
- D) External Tokenization

> **해설:** 데이터 분류(Classification)를 통해 민감 정보를 식별하고 오브젝트 태깅을 적용하면, 거버넌스 준수 여부를 체계적으로 추적할 수 있습니다.

---

**Q79.** 데이터 리니지(Lineage)를 추적하고 태그 기반 마스킹 정책을 적용하기 위해, 테이블이나 뷰와 같은 객체에 부여하는 메타데이터 레이블은?

- A) Row Access Policy
- B) Dynamic Data Masking
- C) **Object Tagging** ✅
- D) Horizon Catalog

> **해설:** Object Tagging은 데이터 거버넌스의 기초로, 객체에 태그를 달아 분류(Classification)하고 이를 기반으로 보안 정책을 일괄 적용하는 데 사용됩니다.

---

**Q80.** 데이터 거버넌스를 위해 오브젝트 태깅(Object Tagging)을 적용할 때, 데이터베이스 수준에서 설정한 태그가 하위 스키마와 테이블에 자동으로 적용되는 원리는?

- A) Tag Replication
- B) Tag Cloning
- C) **Tag Inheritance (태그 상속)** ✅
- D) Metadata Sync

> **해설:** Snowflake의 태그는 상속(Inheritance) 구조를 가집니다. 상위 객체에 부여된 태그는 명시적으로 변경하지 않는 한 하위 객체들로 자동으로 전파되어 일관된 거버넌스 관리를 돕습니다.

---

**Q81.** 데이터 파이프라인의 신뢰성을 보장하기 위해 Snowflake에서 제공하는 최신 기능으로, 테이블의 행 수 변화나 NULL 값 비율 등을 자동으로 모니터링하여 데이터 품질을 측정하는 기능은?

- A) Resource Monitors
- B) **Data Metric Functions (DMFs)** ✅
- C) Object Tagging
- D) Search Optimization

> **해설:** 데이터 품질 메트릭 함수(DMF)를 사용하면 데이터 엔지니어가 수동으로 쿼리를 짜지 않아도 시스템이 정기적으로 데이터의 무결성과 품질 지표를 감시할 수 있습니다.

---

**Q82.** Snowflake Horizon Catalog에서 계정 내의 모든 데이터 자산(테이블, 뷰, 스테이지, ML 모델 등)과 외부 카탈로그(Apache Polaris, AWS Glue 등)의 자산을 하나의 인터페이스에서 검색·거버넌스하기 위해 제공하는 기능은?

- A) Directory Tables
- B) **Universal Search 및 통합 거버넌스(Object Tagging, Classification, Lineage)** ✅
- C) ACCOUNT_USAGE 뷰
- D) Resource Monitor

> **해설:** Horizon Catalog는 검색(Universal Search), 거버넌스(Classification, Tagging, Masking/Row Access/Projection/Aggregation Policy), 규정 준수(Trust Center), 리니지, 품질(DMF), 디스커버리, 협업(Listings) 기능을 통합 제공하는 Snowflake의 카탈로그·거버넌스 계층입니다. 외부 Iceberg 카탈로그와의 연동도 지원합니다.

---

**Q83.** 데이터를 이동시키지 않고 외부 파트너와 안전하게 데이터를 공유하며 쿼리 결과를 분석할 수 있는 환경은?

- A) Data Marketplace
- B) **Snowflake Data Clean Rooms** ✅
- C) Private Listing
- D) Secure View

> **해설:** Clean Room은 데이터를 공유하면서도 원본 데이터를 노출하지 않고 안전하게 분석할 수 있는 프라이버시 보호 환경을 제공합니다.

---

**Q84.** Snowflake의 직접 데이터 공유(Direct Data Sharing)에서 소비자(Consumer)가 `CREATE DATABASE ... FROM SHARE`로 생성한 데이터베이스에 대한 설명으로 옳은 것은?

- A) 공유된 데이터는 소비자 계정의 스토리지에 물리적으로 복사되어 저장된다.
- B) **소비자는 데이터를 복사하지 않고 제공자의 데이터를 실시간으로 읽기 전용(Read-only)으로 쿼리한다.** ✅
- C) 소비자가 공유 데이터베이스에 직접 INSERT나 UPDATE 작업을 수행할 수 있다.
- D) 제공자가 Share를 갱신(Refresh)해야만 소비자가 최신 데이터를 조회할 수 있다.

> **해설:** Snowflake 직접 공유의 핵심은 **Zero-copy**입니다. 소비자는 데이터를 복사하지 않고 제공자의 실제 데이터를 실시간으로 쿼리합니다. 따라서 추가 스토리지 비용이 발생하지 않으며, 제공자의 데이터 변경 사항이 즉시 반영됩니다. 공유 데이터베이스는 읽기 전용(Read-only)으로, 소비자의 DML 작업은 불가합니다.

---

**Q85.** Snowflake의 역할 기반 접근 제어(RBAC) 설계 시 권장되는 가장 효율적인 권한 상속 모델은?

- A) Role Composition (여러 역할을 한 사용자에게 부여)
- B) **Role Hierarchy (역할 간의 계층 구조를 통한 권한 상속)** ✅
- C) 모든 사용자에게 개별 권한 부여
- D) ACCOUNTADMIN 역할을 모든 개발자에게 공유

> **해설:** Snowflake RBAC에서는 `GRANT ROLE child_role TO ROLE parent_role` 구문으로 역할 계층을 구성하면, **자식 역할(child)의 권한이 부모 역할(parent)로 상속**됩니다. 즉, 상위 롤(예: SYSADMIN)은 하위 롤의 모든 권한을 자동으로 포함하게 되어, 개별 사용자/객체에 직접 권한을 부여하는 방식 대비 관리 포인트가 크게 줄어듭니다. 권장 계층: ACCOUNTADMIN → SECURITYADMIN/SYSADMIN → 커스텀 기능 역할 → 접근 역할.

---

### 🟡 5단계: Storage & Data Protection (14%) — Q86 ~ Q100

---

**Q86.** 데이터 변경이 매우 잦은(High Churn) 테이블에서 스토리지 비용이 급증하는 것을 막기 위해 조정해야 할 설정은 무엇입니까?

- A) Fail-safe 기간 연장
- B) **Time Travel 보존 기간(Retention Period) 단축** ✅
- C) 데이터 압축 옵션 변경
- D) 마이크로 파티션 크기 조정

> **해설:** 변경이 잦은 테이블은 Time Travel 기능을 위해 엄청난 양의 과거 데이터를 생성합니다. 감사나 복구 요구사항에 맞춰 보존 기간을 줄이면 스토리지 비용을 직접적으로 절감할 수 있습니다.

---

**Q87.** 실수로 `CREATE OR REPLACE TABLE`을 사용하여 기존 테이블을 덮어썼을 때, 원래 데이터를 복구하는 가장 올바른 순서는?

- A) UNDROP TABLE 실행
- B) **새 테이블의 이름을 변경(Rename)한 후, 이전 테이블을 UNDROP 함** ✅
- C) Time Travel의 OFFSET을 사용하여 데이터를 다시 Insert함
- D) Fail-safe 영역에서 데이터를 추출함

> **해설:** REPLACE 명령은 기존 테이블을 삭제(Drop)한 것으로 처리합니다. 따라서 현재 이름으로 존재하는 새 테이블의 이름을 바꾼 뒤, 삭제된 이전 테이블을 UNDROP 명령으로 살려내야 합니다.

---

**Q88.** Enterprise Edition 사용 시, 특정 스키마(SALES)에만 10일간의 데이터 히스토리를 유지하고 나머지는 기본값(1일)을 유지하려 할 때 가장 비용 효율적인 명령은?

- A) `ALTER ACCOUNT SET DATA_RETENTION_TIME_IN_DAYS = 10;`
- B) **`ALTER SCHEMA SALES SET DATA_RETENTION_TIME_IN_DAYS = 10;`** ✅
- C) `ALTER DATABASE FENIX_DWH SET DATA_RETENTION_TIME_IN_DAYS = 10;`
- D) 모든 테이블마다 개별적으로 설정을 변경함

> **해설:** 계정이나 데이터베이스 수준에서 변경하면 불필요한 스토리지 비용이 발생합니다. 요구사항이 있는 특정 스키마 레벨에서만 기간을 설정하는 것이 가장 비용 효율적입니다.

---

**Q89.** Snowflake의 데이터 보호 기능 중 하나인 Fail-safe에 대한 설명으로 옳은 것은?

- A) 사용자가 직접 데이터를 복구하는 데 사용할 수 있는 기능이다.
- B) **Time Travel 기간이 종료된 후 7일 동안 Snowflake 고객 지원을 통해서만 복구가 가능한 영역이다.** ✅
- C) 모든 테이블 유형(Permanent, Transient, Temporary)에서 동일하게 제공된다.
- D) 추가적인 스토리지 비용이 발생하지 않는다.

> **해설:** Fail-safe는 Time Travel 이후의 최후 보루로, 7일간 유지되며 사용자가 직접 접근할 수 없고 오직 Snowflake 운영팀을 통해서만 데이터 복구를 시도할 수 있습니다.

---

**Q90.** Snowflake의 영구 테이블(Permanent Table)에서 Time Travel 보존 기간이 끝난 후, 데이터 보호를 위해 7일간 유지되는 Fail-safe 영역에 대해 사용자가 지불해야 하는 비용은?

- A) 컴퓨팅(Warehouse) 비용으로 청구된다.
- B) 무료로 제공된다.
- C) **실제 데이터가 차지하는 스토리지 용량만큼 비용이 발생한다.** ✅
- D) 계정 관리 비용에 포함되어 고정 금액으로 청구된다.

> **해설:** Fail-safe 영역에 저장된 데이터도 Snowflake 스토리지의 일부를 차지하므로, 해당 기간 동안 저장된 평균 용량에 따라 스토리지 비용이 청구됩니다.

---

**Q91.** Time Travel을 활용하여 실수로 삭제된 데이터를 특정 과거 시점으로 복원하려 합니다. 이때 가장 일반적으로 사용하는 방법은?

- A) ALTER TABLE ... RESTORE AT(TIMESTAMP => ...) 명령을 실행한다.
- B) **`CREATE TABLE restored_table AS SELECT * FROM original_table AT(TIMESTAMP => '<과거 시점>');`** ✅
- C) UNDROP TABLE을 실행하면 자동으로 과거 데이터가 복원된다.
- D) COPY INTO를 사용하여 Fail-safe 영역에서 데이터를 가져온다.

> **해설:** Snowflake에서 Time Travel 데이터 복원은 `AT` 또는 `BEFORE` 절을 사용하는 SELECT 쿼리로 과거 시점 데이터를 조회한 뒤, CTAS(CREATE TABLE AS SELECT)로 새 테이블에 저장하는 방식이 일반적입니다. UNDROP TABLE은 삭제(DROP)된 테이블 객체 자체를 복구하는 것으로, 특정 시점의 데이터 상태를 복원하는 것과는 다릅니다.

---

**Q92.** 데이터 적재 과정에서 임시적으로만 사용되는 테이블로, Time Travel은 최대 1일까지 지원하지만 Fail-safe 영역은 제공되지 않아 스토리지 비용을 절감할 수 있는 테이블 유형은?

- A) Permanent Table
- B) **Transient Table** ✅
- C) Temporary Table
- D) External Table

> **해설:** Transient(일시적) 테이블은 영구 테이블과 달리 Fail-safe 기간이 없어 스토리지 비용을 아낄 수 있으며, 세션이 종료되어도 유지되므로 ETL 중간 단계 데이터를 저장하기에 적합합니다.

---

**Q93.** 데이터 적재 단계에서만 일시적으로 사용되는 'Transient Table'과 세션 내에서만 유효한 'Temporary Table'의 공통점은?

- A) 세션이 종료되면 둘 다 자동으로 삭제된다.
- B) Time Travel 기간을 최대 90일까지 설정할 수 있다.
- C) **Fail-safe 영역을 제공하지 않아 스토리지 비용을 절감할 수 있다.** ✅
- D) 다른 계정과 직접 공유가 가능하다.

> **해설:** 두 테이블 유형 모두 Fail-safe를 지원하지 않아 영구 테이블보다 비용 효율적입니다. 단, Transient는 세션 종료 후에도 유지되지만 Temporary는 세션과 함께 삭제됩니다.

---

**Q94.** Zero-copy Cloning 기능을 사용하여 1TB 크기의 운영 테이블을 개발용으로 복제했습니다. 복제 직후 이 새로운 테이블이 차지하는 추가적인 스토리지 공간은 얼마입니까?

- A) 1TB (전체 복사)
- B) 500GB (압축 복사)
- C) **0GB (메타데이터만 복제)** ✅
- D) Fail-safe 공간만큼 차지

> **해설:** Zero-copy Cloning은 실제 데이터 파일을 복사하지 않고 기존 마이크로 파티션의 메타데이터만 복제하므로, **복제 직후에는** 추가 스토리지 비용이 발생하지 않습니다. 단, 이후 **원본 또는 복제본 중 어느 쪽에서든 데이터 변경(INSERT/UPDATE/DELETE)이 발생하면**, 변경된 마이크로 파티션은 해당 테이블에 독립적으로 저장되어 그 시점부터 추가 스토리지 비용이 발생합니다.

---

**Q95.** 데이터 플랫폼 간의 상호운용성을 높이기 위해 Apache Iceberg 형식을 사용하여 외부 클라우드 스토리지에 데이터를 저장하고, Snowflake의 성능을 활용하여 쿼리하고자 할 때 적합한 테이블 유형은?

- A) External Tables
- B) **Iceberg Tables** ✅
- C) Hybrid Tables
- D) Transient Tables

> **해설:** Iceberg 테이블은 개방형 표준 스토리지를 사용하면서도 Snowflake 내부 테이블과 유사한 성능 및 거버넌스 기능을 제공하여 플랫폼 간 데이터 이동 없이 효율적인 관리를 가능하게 합니다.

---

**Q96.** 데이터 레이크 하우스 아키텍처를 구축하면서, Apache Iceberg 형식을 사용하는 테이블을 Snowflake가 직접 메타데이터를 관리하도록(Managed) 설정했을 때의 특징은?

- A) 데이터 스캔 성능이 일반 테이블보다 2배 느려진다.
- B) Snowflake 외부의 엔진에서는 데이터를 읽을 수 없다.
- C) **Snowflake가 스토리지 최적화(파일 압축, 클러스터링 등)를 수행하며 성능을 관리한다.** ✅
- D) Time Travel 기능을 사용할 수 없다.

> **해설:** Snowflake Managed Iceberg 테이블은 개방형 형식을 유지하면서도 Snowflake 엔진이 직접 파일 관리와 최적화를 수행하여 최상의 쿼리 성능을 제공합니다.

---

**Q97.** 단일 테이블 내에서 트랜잭션 처리(OLTP)와 분석 쿼리(OLAP)를 동시에 지원하여, 데이터 이동 없이 실시간 애플리케이션 워크로드를 처리할 수 있는 Snowflake의 테이블 유형은?

- A) Transient Tables
- B) Iceberg Tables
- C) **Hybrid Tables** ✅
- D) Dynamic Tables

> **해설:** Hybrid Tables는 Unistore 아키텍처의 일부로, 행 기반 엔진의 빠른 트랜잭션 성능과 컬럼 기반 엔진의 분석 능력을 동시에 제공하여 하이브리드 워크로드를 최적화합니다.

---

**Q98.** 비즈니스 연속성 계획(BCP)의 일환으로 데이터베이스를 다른 지역(Region)이나 다른 클라우드 플랫폼으로 복제하고자 할 때, Snowflake에서 관리해야 할 핵심 요소는?

- A) 오직 테이블 데이터만 복제된다.
- B) **데이터베이스뿐만 아니라 계정 수준의 객체(사용자, 역할, 권한 등)도 함께 복제 설정이 가능하다.** ✅
- C) 복제된 데이터베이스는 즉시 쓰기(Write) 작업이 가능하다.
- D) 복제 시 스토리지 비용은 발생하지 않는다.

> **해설:** Snowflake는 데이터베이스 복제뿐만 아니라 계정 복제(Account Replication) 기능을 통해 사용자, 역할, 네트워크 정책 등 계정 수준의 메타데이터를 함께 동기화하여 재해 복구(DR) 시 완벽한 환경 전환을 지원합니다.

---

**Q99.** PDF나 이미지와 같은 비정형 데이터(Unstructured Data)를 Snowflake에서 관리하고, 파일의 메타데이터를 쿼리하기 위해 반드시 설정해야 하는 객체는?

- A) **Directory Tables** ✅
- B) External Tables
- C) Materialized Views
- D) Sequence

> **해설:** Directory Table은 스테이지에 있는 비정형 파일들에 대한 카탈로그 역할을 하며, 파일 크기, 마지막 수정일 등 메타데이터를 SQL로 쿼리할 수 있게 해줍니다.

---

**Q100.** 스테이지(Stage)에 Directory Table을 활성화하여 파일 메타데이터를 쿼리하려면 스테이지 생성 또는 변경 시 어떤 속성을 설정해야 합니까?

- A) AUTO_INGEST = TRUE
- B) METADATA_TRACKING = TRUE
- C) FILE_CATALOG = ENABLED
- D) **DIRECTORY = (ENABLE = TRUE)** ✅

> **해설:** Directory Table을 사용하기 위해서는 스테이지에 `DIRECTORY = (ENABLE = TRUE)` 속성을 설정해야 합니다. 예: `CREATE STAGE my_stage URL='s3://...' DIRECTORY = (ENABLE = TRUE);` 또는 `ALTER STAGE my_stage SET DIRECTORY = (ENABLE = TRUE);`. 활성화 후 `SELECT * FROM DIRECTORY(@my_stage)`로 파일 목록과 메타데이터를 쿼리할 수 있습니다.

---

---

## 5. 보충 문제 (최신 출제 경향, 2026.04 기준)

> 공식 문서 기준 최근 GA 기능 및 DEA-C02 최신 블루프린트 보강 문제입니다.

---

**Q101. [Data Movement / Storage]** 외부 Apache Iceberg 카탈로그(예: AWS Glue, Apache Polaris)에서 생성되는 새 테이블을 Snowflake가 자동으로 감지하여 Iceberg Table 객체로 동기화하는 기능은?

- A) External Stage Auto-Refresh
- B) **Catalog-Linked Database (LINKED_CATALOG)** ✅
- C) Snowpipe Auto-Ingest
- D) Directory Table Sync

> **해설:** Catalog-Linked Database는 `CREATE DATABASE ... LINKED_CATALOG = (...)` 구문으로 생성하며, 외부 Iceberg 카탈로그의 테이블을 자동 탐색/동기화하여 Snowflake 내에서 별도 DDL 없이 Iceberg Table로 읽기 가능합니다. External Volume과 Catalog Integration이 사전 구성되어야 합니다.

---

**Q102. [Data Transformation]** 자연어로 구조화 데이터(Cortex Analyst), 비구조화 데이터(Cortex Search), 외부 도구(Tools)를 오케스트레이션하여 멀티 스텝 추론을 수행하는 Snowflake의 에이전틱 AI 서비스는?

- A) Cortex Complete
- B) **Snowflake Intelligence / Cortex Agents** ✅
- C) Document AI
- D) Cortex Fine-tuning

> **해설:** Cortex Agents(Snowflake Intelligence의 기반)는 Cortex Analyst(Text-to-SQL)와 Cortex Search(RAG)를 함께 오케스트레이션하여, 사용자의 자연어 질문에 대해 도구 선택 → 검색/SQL 실행 → 결과 합성까지 수행하는 REST API 기반 에이전트 서비스입니다.

---

**Q103. [Performance Optimization / Cost]** 팀/프로젝트 단위로 월간·주간 크레딧 지출 한도를 설정하고, 임계치 도달 시 이메일 알림을 받을 수 있는 Snowflake 기능은? (Resource Monitor의 상위 개념으로, 서버리스·Cortex·Snowpipe 등 모든 크레딧 소비 객체에 적용 가능)

- A) Resource Monitor
- B) **Budgets** ✅
- C) Query Tag
- D) Warehouse Credit Quota

> **해설:** Budgets는 SNOWFLAKE.LOCAL.ACCOUNT_ROOT_BUDGET 또는 커스텀 Budget 객체로 구성되며, Warehouse뿐 아니라 **Serverless Task, Snowpipe, Cortex 함수, Automatic Clustering, Search Optimization** 등 모든 크레딧 소비 리소스에 대한 지출 한도를 설정할 수 있습니다. Resource Monitor는 Warehouse에만 적용되는 반면, Budgets는 훨씬 포괄적입니다.

---

**Q104. [Data Movement]** Snowflake Task를 **Serverless Task**로 생성할 때의 특징으로 올바른 것은?

- A) 반드시 User-managed Warehouse를 지정해야 한다.
- B) Task는 지정된 Warehouse가 자동 재개(Resume)될 때까지 대기한다.
- C) **Warehouse 지정 없이 `USER_TASK_MANAGED_INITIAL_WAREHOUSE_SIZE` 파라미터로 초기 크기를 지정하면, Snowflake가 관리하는 컴퓨팅 자원이 워크로드에 맞춰 자동으로 스케일된다.** ✅
- D) Serverless Task는 Task Graph(DAG)에 포함될 수 없다.

> **해설:** Serverless Task는 `CREATE TASK ... USER_TASK_MANAGED_INITIAL_WAREHOUSE_SIZE = 'XSMALL'` 과 같이 초기 크기만 지정하고 Warehouse를 생략하면 생성됩니다. Snowflake가 과거 실행 통계를 바탕으로 자원을 자동 스케일하며, 짧거나 간헐적인 작업에서 User-managed Task보다 비용 효율적입니다. Task Graph에도 포함 가능합니다. 최신(2025+) 버전에서는 `SERVERLESS_TASK_MIN_STATEMENT_SIZE` / `SERVERLESS_TASK_MAX_STATEMENT_SIZE` 파라미터로 최소·최대 Warehouse 크기를 제한하여 성능과 비용을 동시에 제어할 수 있고(최대는 XXLARGE), `TARGET_COMPLETION_INTERVAL`로 목표 완료 시간을 지정하면 Snowflake가 해당 시간 내 완료를 목표로 자원을 자동 스케일합니다.

---

> **수고하셨습니다!** DEA-C02 시험 대비 100선 + 최신 보충 4문제를 도메인 순서로 완주했습니다.
> 틀린 문제가 많은 도메인은 해당 단계의 **섹션 3 핵심 개념 비교 요약**으로 돌아가 복습하세요.
