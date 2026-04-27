# SnowPro Core (COF-C03) 합격 마스터 가이드
> 핵심 개념 및 실행 전략

데이터 클라우드 시장이 'AI 데이터 클라우드'로 급격히 전환됨에 따라, Snowflake의 기술 생태계 역시 단순한 저장소를 넘어 고도의 연산 효율성과 보안성, 그리고 AI/ML 통합을 지향하고 있습니다. SnowPro Core(COF-C03) 자격증은 이러한 현대적 데이터 플랫폼의 아키텍처를 이해하고 실무에 적용할 수 있는 능력을 검증하는 표준입니다.

특히 이번 개편에서는 오픈 스토리지 표준인 **Apache Iceberg**와 **선언적 데이터 변환 기술**이 핵심 도메인으로 부상했습니다. 따라서 개별 기능의 암기를 넘어, 각 도메인이 어떻게 유기적으로 연결되어 비즈니스 가치를 창출하는지 파악하는 전략적 로드맵이 합격의 필수 요건입니다.

---

## 1. 전략적 학습 로드맵 및 도메인 개요

| 학습 단계 | 도메인 번호 | 비중(%) | 핵심 주제 및 전략적 목표 |
|-----------|-------------|---------|--------------------------|
| 1단계: 기초 및 구조 | 도메인 1.0 | 31% | 3계층 아키텍처, Cortex AI, Snowpark, Apache Iceberg 기초 확립 |
| 2단계: 보안 및 관리 | 도메인 2.0 | 20% | RBAC 권한 체계 구축, 데이터 거버넌스 및 비용 통제 전략 수립 |
| 3단계: 데이터 수집 | 도메인 3.0 | 18% | Snowpipe 및 Dynamic Tables를 활용한 파이프라인 자동화 이해 |
| 4단계: 최적화 및 변환 | 도메인 4.0 | 21% | Query Profile 분석, 3단계 캐싱, dbt 연동을 통한 성능 극대화 |
| 5단계: 공유 및 보호 | 도메인 5.0 | 10% | Zero-copy Cloning 및 실시간 데이터 공유를 통한 협업 민첩성 확보 |

> 위 단계들은 독립된 개체가 아닙니다. 1단계의 아키텍처 이해는 4단계의 성능 최적화 근거가 되며, 2단계의 보안 설정은 5단계의 안전한 데이터 공유를 가능하게 합니다.

---

## 2. 도메인 1.0: Snowflake 아키텍처 및 차세대 기술(AI/ML)

Snowflake는 데이터 스토리지, 컴퓨팅, 클라우드 서비스를 완전히 분리하여 운영 효율성을 극대화한 독보적인 **3계층 아키텍처**를 가집니다.

### 3계층 구조와 스토리지 혁신

| 계층 | 역할 |
|------|------|
| **Database Storage** | 중앙 집중식 저장소. 데이터는 **마이크로 파티션(Micro-partition)** 이라는 불변(Immutable) 단위로 저장. **Apache Iceberg** 지원으로 오픈 스토리지 표준까지 수용 |
| **Query Processing (Compute)** | 실제 연산이 일어나는 '가상 웨어하우스' 계층. **Scaling Up**(크기 확대)으로 단일 쿼리 성능 향상, **Scaling Out**(멀티 클러스터 추가)으로 동시성 문제 해결 |
| **Cloud Services** | 인증, 메타데이터 관리, 쿼리 최적화를 담당하는 '뇌' 역할 |

### 마이크로 파티션과 프루닝(Pruning) 메커니즘

모든 데이터는 로드 시 자동으로 마이크로 파티션화되며, 각 파티션의 메타데이터(최소/최댓값 등)는 **Cloud Services 계층**에 저장됩니다. 쿼리 실행 시 이 메타데이터를 활용해 불필요한 데이터를 건너뛰는 **프루닝(Pruning)** 이 발생하며, 이는 성능 최적화의 핵심 원리입니다.

### AI 및 비SQL 실행 환경 (Cortex & Snowpark)

- **Cortex AI**: 데이터 이동 없이 계정 내에서 안전하게 실행되는 LLM 기능. 데이터가 외부로 유출되지 않는 강력한 보안적 이점을 가짐
- **Snowpark**: **Python, Java, Scala** 개발자를 위한 비SQL 실행 환경. 대규모 메모리 작업에는 **Snowpark 최적화 웨어하우스(Snowpark-optimized Warehouse)** 활용

### 에디션별 차등 기능

| 에디션 | 주요 기능 |
|--------|-----------|
| **Standard** | 기초 기능, Time Travel **최대 1일** |
| **Enterprise** | 멀티 클러스터 웨어하우스 지원, Time Travel **최대 90일** |
| **Business Critical** | **Private Link/Private Service Connect**, 고객 관리 키(Tri-Secret Secure), HIPAA/HITRUST/PCI 등 규제 준수, 데이터베이스 페일오버/리플리케이션 지원 |
| **Virtual Private Snowflake (VPS)** | 완전히 격리된 고객 전용 환경. 최고 수준의 보안이 필요한 금융·정부 기관용 |

---

## 3. 도메인 2.0: 데이터 거버넌스 및 보안 아키텍처

### RBAC 계층 구조 및 관리 역할

Snowflake는 **역할 기반 접근 제어(RBAC) + 임의 접근 제어(DAC)** 하이브리드 모델을 사용합니다. `GRANT ROLE child_role TO ROLE parent_role` 구문으로 롤 계층을 구성하면 **자식 역할(child)의 권한이 부모 역할(parent)에게 상속**됩니다. 즉, 상위 롤(예: SYSADMIN)이 하위 롤의 권한을 모두 포함하게 되는 구조입니다. **ACCOUNTADMIN** 은 계정 내 최상위 권한으로, 보안 설정과 비용 모니터링을 총괄합니다.

### 데이터 거버넌스 기술 대조

| 기능 | 보안 수준 | 설명 |
|------|-----------|------|
| **Dynamic Data Masking** | 열(Column) 수준 | 특정 권한이 없는 사용자에게 민감한 컬럼 정보를 숨김 |
| **Row Access Policy** | 행(Row) 수준 | 쿼리 결과에서 특정 조건에 맞는 데이터만 노출 |

> 두 기능 모두 원본 데이터를 물리적으로 수정하지 않고 논리적으로 접근을 제한합니다.

### 전략적 비용 통제 및 인증

**Resource Monitor** 조치 옵션:

| 옵션 | 동작 |
|------|------|
| **Suspend** | 진행 중인 쿼리를 완료한 후 웨어하우스 중단 |
| **Suspend Immediate** | 실행 중인 모든 쿼리를 즉시 중단 |
| **Notify** | 알림만 발송, 웨어하우스는 계속 실행 |

**인증 방식 권장 아키텍처:**
- 서비스 계정 / 자동화 연결 → **Key-pair** 인증
- 일반 사용자 → **MFA/SSO** 적용

---

## 4. 도메인 3.0 & 4.0: 데이터 수집 최적화 및 쿼리 성능 분석

### 데이터 수집 및 자동 변환

| 구분 | 설명 |
|------|------|
| **Internal Stage** | Snowflake가 관리하는 내부 저장소 |
| **External Stage** | S3, Azure Blob 등 외부 클라우드 참조 |
| **PUT 명령어** | 로컬 파일 업로드 시 **SnowSQL(CLI) 또는 드라이버/커넥터(JDBC, ODBC, Python 등)** 에서만 실행 가능 (Snowsight 워크시트에서는 직접 실행 불가). 단, Snowsight UI의 '명명된 내부 스테이지에 파일 업로드' 기능(Ingestion » Add Data)은 제공됨 |
| **COPY INTO** | 벌크 로드용 |
| **Snowpipe** | 연속 수집용, Snowflake 관리형 **서버리스** 컴퓨팅 사용 |
| **Dynamic Tables** | 복잡한 데이터 변환을 선언적으로 자동화 |

### 반정형 데이터와 캐싱 전략

반정형 데이터는 **VARIANT 타입**으로 저장되며, **FLATTEN 함수**를 통해 관계형 구조로 변환됩니다. 2026년 4월 기준 VARIANT/OBJECT/ARRAY의 **기본 최대 크기는 128 MB(비압축)** 로 확대되었습니다(2025_03 BCR 번들, 이전 기준은 16 MB). 단, 시험 문항이 과거 기준(16 MB)을 전제로 출제될 수 있으므로 두 기준 모두 숙지해야 합니다.

**3계층 캐시:**

| 캐시 종류 | 특징 |
|-----------|------|
| **Result Cache** | 동일 쿼리 결과 재사용. 웨어하우스 가동 불필요. **최대 24시간** 유지 |
| **Metadata Cache** | 테이블 통계 정보 활용 |
| **Warehouse Cache** | 로컬 디스크에 데이터 임시 저장 |

### 성능 병목 분석: Spilling 현상

Spilling은 처리 데이터가 웨어하우스 메모리를 초과할 때 두 단계로 발생합니다.

| 단계 | 현상 | 심각도 |
|------|------|--------|
| **Spilling to Local Disk** | 메모리 초과 시 웨어하우스의 로컬 SSD에 임시 저장 | 중간 |
| **Spilling to Remote Storage** | 로컬 디스크마저 부족할 경우 원격 스토리지(S3 등)에 데이터를 쓰고 읽음 | 심각 |

> **해결책**: 웨어하우스 크기를 키워(Scaling Up) 로컬 메모리 및 디스크 자원을 확보

---

## 5. 도메인 5.0: 데이터 협업 및 보호 체계

### 데이터 보호 및 민첩한 복제

| 기능 | 기간 | 제어 주체 |
|------|------|-----------|
| **Time Travel** | 0~90일 (사용자 설정) | 사용자 |
| **Fail-safe** | 7일 고정 | Snowflake |

- **Zero-copy Cloning**: 실제 데이터를 복제하지 않고 메타데이터만 복사. 추가 스토리지 비용 없이 개발/테스트 환경 즉시 생성 가능. 데이터 변경(DML) 발생 시점부터만 스토리지 비용 발생

### 데이터 공유 생태계

| 기능 | 설명 |
|------|------|
| **Secure Data Sharing** | 데이터를 이동시키지 않고 실시간으로 조회 권한만 부여 |
| **Reader Account** (Managed Account, TYPE=READER) | Snowflake 계정이 없는 파트너에게 제공. 공급자 계정이 `CREATE MANAGED ACCOUNT ... TYPE = READER` 로 생성·소유·관리하며 **모든 컴퓨팅/크레딧 비용은 공급자(Provider) 부담**. Reader 계정에서는 INSERT/UPDATE/DELETE/MERGE/COPY INTO `<table>` 등 쓰기 DML과 PIPE·STAGE·SHARE 생성이 제한됨 |
| **Snowflake Marketplace** | ETL 과정 없이 외부 데이터를 즉시 구독하여 분석 가능 |
| **Replication / Failover** | 계정 간 복제 및 장애 조치로 비즈니스 연속성 보장 |

> **공유 가능한 오브젝트 (2026.04 기준):** 데이터베이스, 일반 테이블, **Dynamic Tables**, 외부 테이블, **Apache Iceberg™ Tables(외부/관리형)**, **Delta Lake Tables(Delta Direct/Catalog-linked DB)**, 마스킹/행 접근 정책이 적용된 뷰·MV, 보안 뷰, 보안 구체화 뷰, **시맨틱 뷰(Semantic View)**, **Cortex Search 서비스**, UDF(보안/비보안), USER_MODEL·CORTEX_FINETUNED·DOC_AI 모델. 모두 **읽기 전용**으로 공유됩니다.

---

## 6. 합격 확정: 최종 핵심 개념 체크리스트 및 행동 지침

### 핵심 개념 체크리스트

- [ ] 3계층 아키텍처에서 각 계층의 역할과 데이터가 저장되는 물리적 위치를 설명할 수 있는가?
- [ ] **Scaling Up** 과 **Scaling Out** 이 해결하는 비즈니스 문제가 각각 무엇인지 구분하는가?
- [ ] **Apache Iceberg** 가 Snowflake 스토리지 전략에서 갖는 개방형 표준으로서의 의미를 이해하는가?
- [ ] **Cortex AI** 가 데이터 보안 측면에서 외부 LLM 서비스보다 우수한 이유는 무엇인가?
- [ ] **Snowpark-optimized Warehouse** 가 일반 웨어하우스와 차별화되는 지점은 무엇인가?
- [ ] RBAC 계층 구조에서 권한 상속의 원리와 **ACCOUNTADMIN** 의 책임 범위를 아는가?
- [ ] **Resource Monitor** 의 세 가지 조치(Suspend, Suspend Immediate, Notify)를 구분할 수 있는가?
- [ ] **PUT** 명령어가 Snowsight UI에서 실행 불가능한 기술적 이유와 SnowSQL의 역할을 이해하는가?
- [ ] **Dynamic Tables** 와 **Snowpipe** 중 어떤 상황에 어떤 도구를 쓰는 것이 적합한가?
- [ ] **Query Profile** 의 Spilling to Remote Storage 발생 시 아키텍처 관점의 해결책은?
- [ ] **Zero-copy Cloning** 이 스토리지 비용을 발생시키지 않는 메타데이터 기반의 원리를 아는가?
- [ ] **Reader Account** 의 컴퓨팅 비용 부담 주체와 생성 목적을 이해하는가?

### 합격 팁 (Pass Tips)

> - **Result Cache** 를 활용하면 가상 웨어하우스 가동 없이(비용 0) 결과를 즉시 확인할 수 있습니다.
> - 멀티 클러스터 웨어하우스(Scaling Out) 기능은 **Enterprise** 에디션부터 지원됩니다.

### 행동 지침 (Action Items)

1. **무료 평가판 활용**: 30일 평가판을 통해 가상 웨어하우스를 생성하고, Scaling Up/Out의 차이를 비용과 성능 대시보드에서 직접 체감
2. **실습 환경 설정**: SnowSQL을 설치하고 로컬 파일을 Internal Stage로 업로드(PUT)하는 전 과정 연습 (UI와의 차별점 인지 필수)
3. **거버넌스 실습**: ACCOUNTADMIN 역할을 사용하여 리소스 모니터를 생성하고, 크레딧 한도 도달 시의 제어 메커니즘 확인
4. **데이터 복구 체험**: Time Travel과 UNDROP 명령어를 사용하여 삭제된 오브젝트를 복구해 보고, Cloning으로 즉각적인 테스트 환경 구축
5. **최신 트렌드 정리**: Cortex AI의 SQL 함수와 Apache Iceberg 테이블 생성 문법을 정리하여 최신 문항에 대비

### 2026년 4월 기준 주요 업데이트 포인트 (공식 문서 검증 반영)

- **반정형 데이터 크기 확대**: VARIANT / OBJECT / ARRAY 컬럼의 기본 최대 크기가 **128 MB**(비압축)로 확대(2025_03 BCR 번들, GA). VARCHAR도 128 MB까지 확장 가능. 단, 기존 문항·교재는 16 MB 기준으로 작성된 경우가 많으므로 두 기준을 모두 숙지.
- **공유 가능 오브젝트 최신 목록**: 데이터베이스, 일반 테이블, Dynamic Tables, 외부 테이블, Apache Iceberg™(외부/관리형), Delta Lake(Delta Direct/Catalog-linked DB), 일반·보안·MV·시맨틱 뷰, Cortex Search 서비스, UDF(보안/비보안), USER_MODEL·CORTEX_FINETUNED·DOC_AI 모델. 공유된 오브젝트는 **모두 읽기 전용**.
- **Reader Account**: 여전히 `CREATE MANAGED ACCOUNT ... TYPE = READER`로 생성·관리(최대 20개 기본). 소비자는 DML/COPY INTO `<table>`/SHARE/STAGE/PIPE 등 쓰기 계열 명령 실행 불가.
- **Snowpark 공식 지원 언어**: Python, Java, Scala (Scala 2.12 GA, 2.13은 프리뷰). R, C++, Go는 미지원.
- **MFA 정책 강화**: 단일 요소 비밀번호 로그인의 단계적 폐지가 진행 중이므로, 사람 사용자는 **MFA/SSO 필수**, 서비스 계정은 **Key-pair / OAuth**를 권장.

---
---

# SnowPro Core (COF-C03) 예상 문제 100개
> 도메인 순서에 따라 재편성된 문제집 (공식 5개 도메인 기준)

---

## 1단계: 기초 및 구조 — 도메인 1.0 예상 문제
> **도메인 1.0 Snowflake AI Data Cloud Features & Architecture | 비중 31%**
> 3계층 아키텍처, Cortex AI, Snowpark, Apache Iceberg, Snowflake 도구 및 인터페이스

---

### 1. Snowflake의 3계층 아키텍처 중 메타데이터 저장, 쿼리 최적화 및 액세스 제어를 담당하는 계층은 어디인가?

- A. 데이터베이스 스토리지 계층 (Database Storage)
- B. 컴퓨팅 계층 (Compute / Virtual Warehouses)
- C. 클라우드 서비스 계층 (Cloud Services)
- D. 네트워크 계층 (Network Layer)

> **정답: C**

---

### 2. 기존 가상 웨어하우스의 크기를 'X-Small'에서 'Medium'으로 변경하여 단일 쿼리의 처리 속도를 높이는 작업을 무엇이라 부르는가?

- A. Scaling Out
- B. Scaling Up
- C. Auto-resume
- D. Multi-cluster scaling

> **정답: B**

---

### 3. Snowflake의 데이터 저장 단위인 '마이크로 파티션(Micro-partition)'에 대한 설명으로 옳은 것은?

- A. 사용자가 직접 크기를 지정해야 한다.
- B. 데이터가 로드될 때 자동으로 생성되며 불변(Immutable)의 특성을 갖는다.
- C. 인덱스를 수동으로 생성해야만 프루닝(Pruning)이 가능하다.
- D. 정형 데이터에만 적용되며 반정형 데이터에는 적용되지 않는다.

> **정답: B**

---

### 4. 외부 인프라 연결이나 데이터 이동 없이 Snowflake 계정 내에서 직접 대규모 언어 모델(LLM) 기능을 SQL 함수로 사용할 수 있게 해주는 기능은?

- A. Snowpipe Streaming
- B. Snowflake Cortex AI
- C. Dynamic Tables
- D. Snowpark Python

> **정답: B**

---

### 5. Snowflake의 파트너 에코시스템 중 '데이터 카탈로그(Data Catalog)' 솔루션에 특화된 파트너사는 어디인가?

- A. dbt
- B. Alation
- C. Tableau
- D. DataRobot

> **정답: B** — Alation은 기업 전반의 데이터를 찾고 이해하며 관리하는 데이터 카탈로그 전문 파트너입니다.

---

### 6. Python이나 Java로 작성된 복잡한 데이터 변환 로직을 Snowflake 내부 인프라에서 안전하게 실행하기 위해 가장 적합한 기능은?

- A. Snowflake Cortex AI
- B. Snowpark
- C. External Tables
- D. Copy Into Command

> **정답: B** — Snowpark는 비SQL 코드를 Snowflake 관리형 컴퓨팅 계층에서 직접 실행할 수 있게 해줍니다.

---

### 7. Python 기반의 Snowpark 워크로드나 머신러닝 모델 학습과 같이 대규모 메모리가 필요한 작업을 수행할 때 선택해야 하는 웨어하우스 유형은?

- A. Standard Virtual Warehouse
- B. Snowpark-optimized Warehouse
- C. High-Concurrency Warehouse
- D. AI-Cortex Warehouse

> **정답: B**

---

### 8. Snowflake Cortex AI 서비스가 제공하는 LLM(대규모 언어 모델) 기능을 사용할 때의 데이터 보안적 특징은?

- A. 데이터 처리를 위해 외부 AI 서비스로 데이터가 전송된다.
- B. 모든 데이터 처리가 Snowflake의 관리형 컴퓨팅 계층 내에서 이루어지므로 계정 외부로 데이터가 유출되지 않는다.
- C. 사용자가 별도의 API 키를 외부 업체로부터 발급받아야 한다.
- D. 보안을 위해 마스킹된 데이터는 Cortex AI 함수를 사용할 수 없다.

> **정답: B**

---

### 9. Snowflake 아키텍처 중 '쿼리 컴파일(Query Compilation)'이 일어나는 계층은 어디인가?

- A. 컴퓨팅 계층 (Compute Layer)
- B. 스토리지 계층 (Storage Layer)
- C. 클라우드 인프라 계층 (Cloud Infrastructure Layer)
- D. 클라우드 서비스 계층 (Cloud Services Layer)

> **정답: D**

---

### 10. Python이나 Java로 작성된 비-SQL 코드를 Snowflake 내부에서 안전하게 실행하며, 감성 분석 점수 계산과 같은 대규모 워크로드를 처리하는 데 가장 적절한 기능은?

- A. 외부 함수 (External Functions)
- B. Snowpark
- C. 저장 프로시저 (Stored Procedure)
- D. Cortex Complete

> **정답: B**

---

### 11. Snowpark를 사용하여 작성된 Python 또는 Java 코드가 실제로 실행되는 위치는 어디인가?

- A. 사용자의 로컬 컴퓨터
- B. 외부 클라우드 서비스(AWS Lambda 등)
- C. Snowflake의 관리형 컴퓨팅 계층(Virtual Warehouse)
- D. 클라우드 서비스 계층(Cloud Services)

> **정답: C**

---

### 12. Snowflake의 '클라우드 서비스 계층(Cloud Services Layer)'에 저장되어 관리되는 정보가 아닌 것은?

- A. 테이블의 메타데이터 및 마이크로 파티션 목록
- B. 사용자 권한 및 액세스 제어 정보
- C. 실제 테이블 데이터(Raw Data)
- D. 쿼리 컴파일 및 최적화 정보

> **정답: C** — 실제 데이터는 스토리지 계층에 저장됩니다.

---

### 13. Snowflake의 관리형 컴퓨팅 계층 내에서 안전하게 실행되는 'Snowpark'에서 현재 공식적으로 지원하는 프로그래밍 언어들은 무엇인가?

- A. Python, JavaScript, R
- B. Python, Java, Scala
- C. SQL, Python, Go
- D. Java, C#, Python

> **정답: B**

---

### 14. Snowflake 아키텍처에서 테이블의 행 수(Row count)나 컬럼의 최소/최대값과 같은 메타데이터가 저장되고 관리되는 계층은 어디인가?

- A. 데이터베이스 스토리지 계층 (Database Storage)
- B. 컴퓨팅 계층 (Compute / Virtual Warehouses)
- C. 클라우드 서비스 계층 (Cloud Services)
- D. 하드웨어 인프라 계층

> **정답: C**

---

### 15. Snowpark 환경에서 Snowflake의 컴퓨팅 자원을 활용하여 데이터 파이프라인이나 ML 모델을 개발할 때 공식적으로 사용할 수 있는 언어는? *(두 가지 선택)*

- A. Python
- B. R
- C. Java (또는 Scala)
- D. C++

> **정답: A, C** — 2026.04 공식 문서 기준 Snowpark는 **Python, Java, Scala** 세 가지 언어를 공식 지원합니다. Java와 Scala는 동일 JVM 기반이므로 C 보기로 통합 표기했습니다. R과 C++는 지원되지 않습니다.

---

### 16. Snowflake Cortex AI의 LLM 함수(Translate, Summarize 등)가 보안 측면에서 가지는 이점은 무엇인가?

- A. 데이터를 외부 AI 모델 공급자의 API 서버로 전송하여 처리한다.
- B. 모든 데이터 처리가 Snowflake 계정 내부의 관리형 컴퓨팅 계층에서 실행되어 데이터 유출 위험이 없다.
- C. 모델 학습을 위해 사용자의 데이터를 익명화하여 Snowflake 공용 저장소에 보관한다.
- D. 데이터 보안을 위해 암호화된 상태에서는 AI 분석이 불가능하다.

> **정답: B**

---

### 17. Python 워크로드나 대규모 메모리를 필요로 하는 머신러닝 작업을 위해 설계된 특수 가상 웨어하우스 유형은?

- A. Standard Warehouse
- B. Snowpark-optimized Warehouse
- C. High-Concurrency Warehouse
- D. AI-Cortex Warehouse

> **정답: B**

---

### 18. Snowsight(웹 인터페이스)에서 SQL 워크시트 작업을 할 때 코드 실행 없이 즉시 데이터의 분포를 시각화하여 보여주는 기능은?

- A. Query Profile
- B. Instant Visualization (Contextual Charts)
- C. Dashboard
- D. Worksheet History

> **정답: B**

---

### 19. Snowflake 클라우드 데이터 플랫폼 아키텍처 중 '쿼리 컴파일(Query compilation)'이 수행되는 계층은 어디인가?

- A. 스토리지 계층 (Storage Layer)
- B. 컴퓨팅 계층 (Compute Layer)
- C. 클라우드 서비스 계층 (Cloud Services Layer)
- D. 클라우드 인프라 계층 (Cloud Infrastructure Layer)

> **정답: C**

---

### 20. 사용자가 자연어를 사용하여 데이터와 상호 작용하고 통찰력을 얻을 수 있게 해주는 Snowflake Cortex 기능은 무엇인가?

- A. Cortex Search
- B. Cortex Analyst
- C. Cortex Complete
- D. Cortex Translate

> **정답: B**

---

### 21. 'Snowpark-optimized Warehouse'를 사용하는 것이 가장 권장되는 경우는 언제인가?

- A. 표준 SQL 보고서 및 대시보드 쿼리 실행 시
- B. Snowpipe를 통한 대량의 데이터 로딩 시
- C. 대규모 메모리가 필요한 머신러닝 모델 학습 또는 Python 기반 변환 작업 시
- D. 읽기 전용 계정(Reader Account) 관리 시

> **정답: C**

---

### 22. Snowflake에서 'Apache Iceberg™ 테이블'을 사용할 때의 주요 장점은 무엇인가?

- A. 기본적으로 365일의 Time Travel 기간을 제공한다.
- B. 오직 Snowflake에서만 지원하는 폐쇄형 독점 형식을 사용한다.
- C. 오픈 데이터 저장 형식을 유지하면서도 Snowflake의 성능과 거버넌스 혜택을 누릴 수 있다.
- D. 멀티 클러스터 확장을 지원하는 유일한 테이블 유형이다.

> **정답: C**

---

## 2단계: 보안 및 관리 — 도메인 2.0 예상 문제
> **도메인 2.0 Account Management and Data Governance | 비중 20%**
> RBAC 권한 체계, 데이터 거버넌스, 비용 통제, 인증, 에디션 보안 기능

---

### 23. 보안 관리자가 특정 역할의 사용자에게만 주민등록번호 열의 데이터 일부를 숨기고(Masking) 보여주려 할 때 가장 적절한 기능은?

- A. Row Access Policy
- B. Object Tagging
- C. Dynamic Data Masking
- D. Network Policy

> **정답: C**

---

### 24. AWS/Azure PrivateLink 또는 Google Cloud Private Service Connect를 사용하기 위해 필요한 Snowflake의 최소 에디션은 무엇인가?

- A. Standard
- B. Enterprise
- C. Business Critical
- D. Virtual Private Snowflake (VPS)

> **정답: C** — Business Critical 에디션부터 프라이빗 링크 연결을 지원합니다.

---

### 25. Snowflake 클라우드 서비스 비용은 컴퓨팅 크레딧 사용량의 몇 %를 초과할 때부터 실제 사용자에게 청구되는가?

- A. 5%
- B. 10%
- C. 15%
- D. 20%

> **정답: B** — Snowflake는 일일 컴퓨팅 사용량의 10%까지 클라우드 서비스 비용을 면제해주며, 이를 초과하는 분량만 청구합니다.

---

### 26. 계정 내에서 적용된 '동적 데이터 마스킹(Dynamic Data Masking)' 정책의 참조 정보와 감사 내용을 확인하기 위해 조회해야 하는 뷰는 무엇인가? *(두 가지 선택)*

- A. POLICY_REFERENCES
- B. ACCESS_HISTORY
- C. QUERY_HISTORY
- D. ROLES

> **정답: A, B** — 어떤 정책이 어떤 오브젝트에 적용되었는지는 `POLICY_REFERENCES`에서, 실제 데이터 접근 이력(마스킹 적용 여부 포함)은 `ACCESS_HISTORY`에서 확인합니다. `QUERY_HISTORY`는 쿼리 실행 이력을 보여주지만, 마스킹 정책 적용 감사에는 `ACCESS_HISTORY`가 더 적합합니다.

---

### 27. Snowflake 연결 시 권장되는 '표준 계정 식별자(Standard Account Identifier)'의 형식으로 가장 적절한 것은?

- A. `locator.region` (예: xyz123.us-east-1)
- B. `organization_name-account_name` (예: myorg-account1)
- C. `account_id.cloud_provider` (예: 12345.aws)
- D. `user_name.account_name` (예: admin.myaccount)

> **정답: B**

---

### 28. 리소스 모니터(Resource Monitor)의 조치 중, 할당된 크레딧 한도에 도달하는 즉시 현재 실행 중인 모든 쿼리를 중단시키고 웨어하우스를 일시 중지시키는 옵션은?

- A. Suspend
- B. Suspend Immediate
- C. Notify & Suspend
- D. Abort All

> **정답: B**

---

### 29. 다음 중 사용자가 '클라우드 서비스(Cloud Services)' 비용을 지불해야 하는 시나리오는 무엇인가? *(컴퓨팅 사용량의 10%를 초과하는 경우를 선택, 두 가지)*

| 보기 | 컴퓨팅 크레딧 | 클라우드 서비스 크레딧 | 10% 기준값 | 청구 여부 |
|------|--------------|----------------------|-----------|-----------|
| A | 50 | 10 | 5 | **초과 → 청구** |
| B | 80 | 5 | 8 | 미초과 → 미청구 |
| C | 120 | 10 | 12 | 미초과 → 미청구 |
| D | 200 | 26 | 20 | **초과 → 청구** |

> **정답: A, D** — A는 50의 10%인 5를 초과하므로 청구 대상, D는 200의 10%인 20을 초과하므로 청구 대상. B는 80의 10%=8이므로 5는 미초과, C는 120의 10%=12이므로 10은 미초과.

---

### 30. 오브젝트를 소유한 역할이 해당 오브젝트에 대한 액세스 권한을 다른 역할에 부여할 수 있도록 허용하는 액세스 제어 프레임워크는?

- A. 임의 액세스 제어 (DAC)
- B. 역할 기반 액세스 제어 (RBAC)
- C. 사용자 기반 액세스 제어 (UBAC)
- D. 역할 계층 및 권한 상속

> **정답: A**

---

### 31. 데이터베이스 전반에 걸쳐 민감한 고객 데이터를 자동으로 감지하고 분류하여 보안 관리를 돕는 기능은 무엇인가?

- A. 데이터 분류 (Data Classification)
- B. 오브젝트 태깅 (Object Tagging)
- C. Dynamic Data Masking
- D. Row Access Policy

> **정답: A**

---

### 32. Snowflake 연결 시 가독성이 높고 리전 이동 시에도 변하지 않아 사용이 권장되는 '표준 계정 식별자'의 형식은?

- A. `locator.region`
- B. `organization_name-account_name`
- C. `account_id.cloud_provider`
- D. `user_id.account_id`

> **정답: B**

---

### 33. 데이터 거버넌스 측면에서 특정 오브젝트에 메타데이터를 할당하여 비용 센터를 분류하거나 민감한 데이터를 추적하기 위해 사용되는 기능은?

- A. 데이터 분류 (Data Classification)
- B. 오브젝트 태깅 (Object Tagging)
- C. 리소스 모니터 (Resource Monitor)
- D. 동적 데이터 마스킹 (Dynamic Data Masking)

> **정답: B**

---

### 34. Snowflake 연결 및 드라이버 구성 시 권장되는 '표준 계정 식별자(Standard Account Identifier)'의 가장 올바른 예시는?

- A. `xy12345.us-east-1.aws`
- B. `my_organization-main_production_account`
- C. `account_locator.region_name`
- D. `user_id@organization_name`

> **정답: B**

---

### 35. 리소스 모니터(Resource Monitor)를 설정할 때, 할당량의 100%에 도달하면 웨어하우스를 정지시키되 현재 실행 중인 쿼리는 끝까지 완료하도록 허용하는 옵션은?

- A. Suspend Immediate
- B. Suspend
- C. Notify & Suspend
- D. Soft Suspend

> **정답: B**

---

### 36. 보안 관리자가 특정 사용자 그룹에게만 이메일 주소의 뒷부분을 별표(*)로 처리하여 보여주려고 할 때, 전체 행(Record)은 유지하면서 특정 열(Column)만 가리는 가장 적합한 기능은?

- A. Row Access Policy
- B. Object Tagging
- C. Dynamic Data Masking
- D. Secure View

> **정답: C**

---

### 37. 다음 중 Snowflake가 권장하는 '표준 계정 식별자(Standard Account Identifier)'를 구성하는 요소는 무엇인가?

- A. 계정 로케이터(Locator)와 리전(Region)
- B. 조직 이름(Organization)과 계정 이름(Account)
- C. 클라우드 서비스 제공자와 계정 번호
- D. 이메일 주소와 도메인 이름

> **정답: B**

---

### 38. 보안 가이드라인에 따라 서비스 계정(Service Account)이나 자동화된 스크립트가 Snowflake에 연결할 때 반드시 사용해야 하는 인증 방식은?

- A. 사용자 ID와 비밀번호
- B. 멀티 팩터 인증(MFA)
- C. 키 페어(Key-pair) 인증
- D. 싱글 사인온(SSO)

> **정답: C** — 기계나 자동화 도구는 비밀번호를 직접 입력할 수 없으므로 키 페어 인증이 필수입니다.

---

### 39. Snowflake RBAC 모델에서 계정 내 다른 모든 역할에 대한 권한을 관리하며, 시스템의 최고 권한을 가진 역할은 무엇인가?

- A. SYSADMIN
- B. SECURITYADMIN
- C. ACCOUNTADMIN
- D. USERADMIN

> **정답: C**

---

### 40. Snowflake 사용자가 클라우드 서비스(Cloud Services) 비용을 실제로 추가 지불해야 하는 시나리오는 무엇인가?

- A. 매일 발생하는 모든 클라우드 서비스 사용량에 대해 즉시 청구된다.
- B. 해당 리전의 표준 월정액 요금으로 청구된다.
- C. 일일 클라우드 서비스 사용량이 가상 웨어하우스(Compute) 사용량의 10%를 초과하는 경우, 그 초과분에 대해서만 청구된다.
- D. 클라우드 서비스는 항상 무료이며 컴퓨팅 비용에 모두 포함되어 있다.

> **정답: C**

---

### 41. Snowflake의 액세스 제어 프레임워크 중, 특정 오브젝트를 소유한 역할(Owner)이 해당 오브젝트에 대한 액세스 권한을 다른 역할에 부여할 수 있도록 허용하는 방식은 무엇인가?

- A. 역할 기반 액세스 제어 (RBAC)
- B. 임의 액세스 제어 (DAC)
- C. 사용자 기반 액세스 제어 (UBAC)
- D. 네트워크 정책 기반 제어 (NPBC)

> **정답: B**

---

### 42. 관리자가 보안 권장 사항에 따라 Snowflake 계정의 설정을 평가하고, 잠재적인 보안 위험을 시각적으로 점검하고자 할 때 가장 적합한 기능은?

- A. ACCOUNT_USAGE 스키마 조회
- B. Trust Center 활용
- C. Resource Monitor 설정
- D. Access History 뷰 분석

> **정답: B**

---

### 43. 기업의 보안 정책상 AWS/Azure PrivateLink 또는 Google Cloud Private Service Connect 기능을 반드시 사용해야 한다면, 최소한 어떤 Snowflake 에디션을 구독해야 하는가?

- A. Standard
- B. Enterprise
- C. Business Critical
- D. Virtual Private Snowflake (VPS)

> **정답: C**

---

### 44. 한 규정 준수 분석가가 데이터베이스 전반에 걸쳐 주민등록번호나 신용카드 번호와 같은 민감한 고객 데이터를 자동으로 감지하고 분류하려 할 때 사용하는 기능은?

- A. 오브젝트 태깅 (Object Tagging)
- B. 데이터 분류 (Data Classification)
- C. 동적 데이터 마스킹 (Dynamic Data Masking)
- D. 로우 수준 보안 (Row Access Policy)

> **정답: B**

---

### 45. 보안 관리자가 자신의 Snowflake 계정 보안 상태를 베스트 프랙티스에 따라 평가하고 점검하는 데 사용해야 할 도구는 무엇인가?

- A. ACCOUNT_USAGE 뷰 조회
- B. Trust Center
- C. Resource Monitor
- D. Snowsight 대시보드

> **정답: B**

---

## 3단계: 데이터 수집 — 도메인 3.0 예상 문제
> **도메인 3.0 Data Loading, Unloading, and Connectivity | 비중 18%**
> 스테이지(Internal/External), COPY INTO, Snowpipe, ON_ERROR 옵션, 파일 형식, Storage Integration

---

### 46. AWS S3나 Azure Blob Storage와 같은 외부 클라우드 저장소의 위치와 인증 정보를 Snowflake 내부에서 참조하기 위해 생성해야 하는 오브젝트는?

- A. Materialized View
- B. Internal Stage
- C. External Stage
- D. Virtual Warehouse

> **정답: C**

---

### 47. 로컬 컴퓨터에 있는 파일을 Snowflake 내부 스테이지로 업로드하기 위해 사용하는 'PUT' 명령어에 대한 설명으로 옳은 것은?

- A. Snowsight 웹 UI의 SQL 워크시트에서 실행 가능하다.
- B. SnowSQL(CLI) 또는 JDBC/ODBC/Python 등 드라이버·커넥터를 통해서만 실행 가능하다.
- C. 외부 스테이지(External Stage)로 파일을 보낼 때 사용한다.
- D. 데이터를 테이블로 직접 로드하는 명령어이다.

> **정답: B** — PUT 명령은 Snowsight 워크시트에서 실행할 수 없으며, 드라이버·커넥터에서만 사용할 수 있습니다. 단, Snowsight 내부 스테이지에 파일을 UI로 업로드하는 기능 자체는 제공됩니다(PUT 구문을 직접 입력하는 것과는 별개).

---

### 48. Snowflake 테이블의 데이터를 외부 스테이지로 언로딩(Unloading)할 때 지원되는 반정형 파일 형식은 무엇인가? *(두 가지 선택)*

- A. XML
- B. JSON
- C. Parquet
- D. ORC

> **정답: B, C** — 데이터 언로딩(`COPY INTO <location>`)은 **CSV, JSON, PARQUET** 세 형식만 지원합니다. XML(A), ORC(D)는 로딩(`COPY INTO <table>`) 시에만 지원되며 언로딩은 불가합니다.

---

### 49. 로컬 파일 시스템의 데이터를 스테이지로 업로드하는 'PUT' 명령어의 실행 환경에 대한 설명으로 옳은 것은?

- A. Snowsight 웹 인터페이스의 워크시트에서 실행할 수 있다.
- B. 반드시 SnowSQL(CLI) 또는 드라이버/커넥터를 통해 프로그래밍 방식으로 실행해야 한다.
- C. 외부 스테이지(S3, Azure 등)로 데이터를 업로드할 때만 사용한다.
- D. 데이터베이스 관리자(ACCOUNTADMIN)만 실행할 수 있다.

> **정답: B** — PUT은 내부 스테이지에만 직접 업로드하며 외부 스테이지는 지원하지 않습니다(C 오답). 드라이버·커넥터를 사용해야 하며, 권한은 해당 스테이지에 대한 WRITE/USAGE만 있으면 충분합니다(D 오답).

---

### 50. Snowpipe를 통해 실행하거나 호출할 수 있는 가장 적절한 구문은 무엇인가?

- A. 사용자 정의 함수 (UDF)
- B. 저장 프로시저 (Stored Procedure)
- C. 단일 COPY INTO 문
- D. 단일 INSERT INTO 문

> **정답: C**

---

### 51. COPY INTO 명령을 사용하여 데이터를 로드할 때, 파일에 오류가 포함되어 있어도 오류가 없는 다른 파일들을 계속 로드하게 하려면 어떤 옵션을 사용해야 하는가?

- A. `ON_ERROR = ABORT_STATEMENT`
- B. `ON_ERROR = SKIP_FILE`
- C. `ON_ERROR = CONTINUE`
- D. `ON_ERROR = IGNORE`

> **정답: B** — 파일 단위로 오류 파일을 건너뛸 때는 `SKIP_FILE`을 사용합니다. 오류 행을 건너뛰고 해당 파일을 계속 로드하려면 `CONTINUE`도 가능합니다.

---

### 52. 외부 스테이지(S3, Azure 등)를 생성할 때 클라우드 서비스 제공자의 인증 정보(Secret Key 등)를 Snowflake 내부에 직접 노출하지 않기 위해 생성해야 하는 오브젝트는?

- A. API Integration
- B. Storage Integration
- C. External Stage
- D. Network Policy

> **정답: B**

---

### 53. 대량의 데이터를 벌크 로딩하는 COPY INTO 문과 비교했을 때, Snowpipe의 주요 특징으로 옳은 것은?

- A. 가상 웨어하우스의 크기에 따라 성능이 결정된다.
- B. 사용자 관리 웨어하우스 대신 Snowflake 관리형 '서버리스' 컴퓨팅 자원을 사용한다.
- C. 한 번에 로드할 수 있는 파일의 크기에 제한이 없다.
- D. 로딩 중 발생한 오류를 수정하기 위해 ON_ERROR 옵션을 사용할 수 없다.

> **정답: B**

---

### 54. Snowflake 테이블의 데이터를 외부 저장소로 언로딩(Unloading)할 때 공식적으로 지원되는 파일 형식은 무엇인가? *(두 가지 선택)*

- A. ORC
- B. JSON
- C. XML
- D. Parquet

> **정답: B, D** — 데이터 언로딩(`COPY INTO <location>`)은 **CSV, JSON, PARQUET** 세 형식만 지원합니다. ORC(A)와 XML(C)은 로딩 시에만 지원됩니다. 참고: 정형 형식 CSV도 보기에 있었다면 복수 정답이 될 수 있습니다.

---

### 55. 지속적인 데이터 수집을 위해 사용하는 Snowpipe가 내부적으로 실행하거나 호출할 수 있는 구문은 무엇인가?

- A. 사용자 정의 함수 (UDF)
- B. 저장 프로시저 (Stored Procedure)
- C. 단일 COPY INTO 문
- D. 다중 테이블 INSERT 문

> **정답: C**

---

## 4단계: 최적화 및 변환 — 도메인 4.0 예상 문제
> **도메인 4.0 Performance Optimization, Querying, and Transformation | 비중 21%**
> Query Profile, 캐싱, Spilling, 클러스터링, Search Optimization, 반정형 데이터, UDF, Stream, Dynamic Tables, dbt

---

### 56. 이전에 실행된 동일한 쿼리의 결과를 활용하는 '결과 캐시(Result Cache)'를 사용하여 데이터를 조회할 때, 가상 웨어하우스의 상태는 어떠해야 하는가?

- A. 반드시 'Started' 상태여야 한다.
- B. 'Suspended' 상태여도 웨어하우스 가동 없이 결과를 반환할 수 있다.
- C. 최소 'Large' 크기 이상의 웨어하우스가 필요하다.
- D. 캐시 사용 시 크레딧이 2배로 소모된다.

> **정답: B**

---

### 57. Query Profile 분석 중 'Spilling to Remote Storage' 지표가 나타났다면, 성능 최적화를 위해 어떤 조치가 필요함을 의미하는가?

- A. 데이터 클러스터링 키 재설정
- B. 가상 웨어하우스의 크기(Size) 확대
- C. 리소스 모니터 한도 상향
- D. 결과 캐시(Result Cache) 비활성화

> **정답: B** — 스필링은 로컬 메모리와 디스크가 부족하여 원격 스토리지에 데이터를 쓸 때 발생하며, 더 큰 웨어하우스를 사용하여 해결할 수 있습니다.

---

### 58. 활성화된 가상 웨어하우스의 크기를 축소(Downsizing)할 때, 현재 실행 중인 쿼리는 어떻게 처리되는가?

- A. 즉시 중단되고 오류가 발생한다.
- B. 실행 중인 쿼리가 완료될 때까지 컴퓨팅 자원이 유지된 후 제거된다.
- C. 실행 중인 쿼리는 자동으로 더 작은 사이즈로 재배정된다.
- D. 쿼리 결과가 캐시에 저장된 후 중단된다.

> **정답: B** — 웨어하우스 축소 시, 기존에 실행 중인 문장이 완료될 때까지는 자원이 삭제되지 않습니다.

---

### 59. Query Profile 분석 중 'Spilling to Remote Storage' 현상이 나타날 때, 성능 개선을 위해 가장 먼저 고려해야 할 조치는?

- A. 마이크로 파티션의 개수를 수동으로 줄인다.
- B. 더 큰 크기의 가상 웨어하우스(Scaling Up)를 사용하여 로컬 메모리와 디스크 용량을 늘린다.
- C. 쿼리에 사용된 모든 뷰를 테이블로 변경한다.
- D. 결과 캐시(Result Cache) 기능을 끈다.

> **정답: B**

---

### 60. 활성화된 가상 웨어하우스의 크기를 더 작게 조정(Resize)할 때, 해당 웨어하우스에 캐시된 데이터는 어떻게 되는가?

- A. 웨어하우스 크기가 줄어들어 캐시를 더 이상 수용할 수 없게 되면 손실될 가능성이 있다.
- B. 컴퓨팅 리소스가 완전히 교체되므로 100% 삭제된다.
- C. 캐시 크기는 웨어하우스 크기와 무관하므로 전혀 영향을 받지 않는다.
- D. 새로운 컴퓨팅 리소스가 암호화 키에 접근할 수 없어 모두 삭제된다.

> **정답: A**

---

### 61. 테이블에 '클러스터링 키(Clustering Key)'를 설정하는 것이 적절함을 나타내는 지표는 무엇인가? *(두 가지 선택)*

- A. 테이블의 특정 컬럼 카디널리티(Cardinality)가 매우 낮을 때
- B. 테이블에 대한 DML 문이 차단(Blocked)될 때
- C. 테이블의 쿼리 실행 속도가 예상보다 느릴 때
- D. 테이블의 클러스터링 깊이(Clustering Depth)가 클 때

> **정답: C, D** — 쿼리 실행 속도가 느릴 때와 클러스터링 깊이가 클수록 데이터 분포가 비효율적임을 나타내므로 클러스터링 키 설정을 고려해야 합니다.

---

### 62. 가상 웨어하우스가 높은 동시성(High Concurrency) 워크로드를 처리하기 위해 자동으로 클러스터를 추가하는 '멀티 클러스터 웨어하우스' 기능을 사용하기 위한 최소 에디션은?

- A. Standard
- B. Enterprise
- C. Business Critical
- D. Virtual Private Snowflake (VPS)

> **정답: B**

---

### 63. 대규모 테이블에서 특정 값(Point lookup)을 검색하는 쿼리의 성능을 획기적으로 개선하며, 마이크로 파티션의 메타데이터를 활용하여 불필요한 스캔을 줄이는 서비스는?

- A. Materialized View
- B. Search Optimization Service (SOS)
- C. Query Acceleration Service
- D. Zero-copy Cloning

> **정답: B**

---

### 64. Snowflake 사용 비용을 절감하기 위해 권장되는 가상 웨어하우스 설정 중, 쿼리가 실행되지 않을 때 자동으로 가동을 멈추게 하는 옵션은?

- A. Auto-resume
- B. Auto-suspend
- C. Scaling Policy
- D. Resource Monitoring

> **정답: B**

---

### 65. 대규모 테이블에서 단 몇 개의 행(Row)만을 반환하는 '포인트 룩업(Point Lookup)' 쿼리의 성능을 개선하기 위해 설계된 서비스는 무엇인가?

- A. Query Acceleration Service
- B. Search Optimization Service (SOS)
- C. Zero-copy Cloning
- D. Result Cache

> **정답: B**

---

### 66. 이전에 실행된 쿼리와 동일한 결과를 반환하는 '결과 캐시(Result Cache)'는 원본 데이터가 변경되지 않았을 경우 최대 몇 시간 동안 유지되는가?

- A. 1시간
- B. 12시간
- C. 24시간
- D. 48시간

> **정답: C**

---

### 67. Query Profile 분석 중 'Spilling to Remote Storage' 현상이 발생했을 때의 설명으로 옳은 것은?

- A. 가상 웨어하우스의 로컬 메모리와 디스크가 부족하여 원격 스토리지(S3 등)에 데이터를 쓰고 읽고 있음을 의미한다.
- B. 데이터가 너무 작아서 파티션이 생성되지 않았을 때 발생한다.
- C. 결과 캐시(Result Cache)가 가득 찼을 때 나타나는 정상적인 현상이다.
- D. 쿼리 최적화 도구가 원격 데이터베이스에 직접 접근하고 있음을 의미한다.

> **정답: A**

---

### 68. 테이블의 데이터 분포가 쿼리 성능에 미치는 영향을 측정하는 지표 중 하나로, 이 값이 클수록 쿼리 성능 저하가 의심되어 클러스터링 키 설정을 고려해야 하는 것은?

- A. Clustering Cardinality
- B. Clustering Depth
- C. Micro-partition Count
- D. Pruning Ratio

> **정답: B**

---

### 69. 현재 활성화되어 실행 중인 가상 웨어하우스의 크기를 더 작은 사이즈로 축소(Resize)할 때, 웨어하우스 내부의 캐시(Cache) 데이터에 발생할 수 있는 현상은?

- A. 컴퓨팅 자원이 교체되므로 모든 캐시 데이터가 즉시 삭제된다.
- B. 캐시는 웨어하우스 크기와 무관하므로 항상 100% 보존된다.
- C. 축소된 웨어하우스 크기에 캐시가 더 이상 맞지 않을 경우 일부 데이터가 손실될 수 있다.
- D. 캐시 데이터는 자동으로 스토리지 계층으로 이동되어 보존된다.

> **정답: C**

---

### 70. 테이블에 '클러스터링 키(Clustering Key)'를 설정했을 때, 마이크로 파티션 내부의 데이터는 성능 최적화를 위해 어떻게 재구성되는가?

- A. 모든 행이 알파벳 순서대로 강제 재정렬된다.
- B. 데이터가 클러스터 키를 기준으로 동일한 파티션 내에 병치(Co-located)되어 프루닝(Pruning) 성능이 향상된다.
- C. 더 많은 병렬 처리를 위해 마이크로 파티션의 크기가 1MB 단위로 작아진다.
- D. 중복 데이터를 제거하기 위해 해시 값으로 모든 데이터가 변환된다.

> **정답: B**

---

### 71. 대규모 테이블에서 특정 값(Point Lookup)을 찾는 쿼리의 속도를 높이기 위해 영구적인 데이터 구조를 구축하는 기능은 무엇인가?

- A. Query Acceleration Service
- B. Result Cache
- C. Search Optimization Service (SOS)
- D. Zero-copy Cloning

> **정답: C**

---

### 72. '구체화된 뷰(Materialized View)'가 성능 최적화 측면에서 가장 큰 이점을 주는 시나리오는 무엇인가?

- A. 데이터가 끊임없이 업데이트되는 테이블에서 자주 필터링할 때
- B. 데이터 변경이 빈번하지 않은 대규모 테이블에서 복잡한 쿼리가 반복적으로 실행될 때
- C. 소량의 데이터를 연속적으로 로드할 때
- D. Snowflake 계정이 없는 소비자와 데이터를 공유할 때

> **정답: B**

---

### 73. Snowflake 외부(예: AWS Lambda)에서 실행되는 코드를 호출하여 Snowflake 내부에서 데이터 처리를 수행하고자 할 때 사용하는 기능은?

- A. User Defined Function (UDF)
- B. Stored Procedure
- C. External Function
- D. Task

> **정답: C** — 외부 함수는 Snowflake 외부 인프라에서 실행되는 코드와 상호 작용하는 데 사용됩니다.

---

### 74. 반정형 데이터를 저장하는 'VARIANT' 컬럼이 가질 수 있는 값의 유형은 무엇인가? *(두 가지 선택)*

- A. STRUCT
- B. OBJECT
- C. BINARY
- D. ARRAY
- E. CLOB

> **정답: B, D**

---

### 75. 테이블에 대한 DML(데이터 조작어) 변경 사항을 기록하여 데이터 파이프라인에서 CDC(변경 데이터 캡처)를 구현하는 데 사용되는 오브젝트는?

- A. Task
- B. Stream
- C. Pipe
- D. Sequence

> **정답: B**

---

### 76. Snowflake가 지원하는 최신 테이블 유형 중, 오픈 소스 스토리지 표준을 사용하면서 Snowflake의 쿼리 성능과 거버넌스 혜택을 동시에 누릴 수 있는 것은?

- A. External Table
- B. Transient Table
- C. Apache Iceberg Table
- D. Dynamic Table

> **정답: C**

---

### 77. SQL 문(예: SELECT 문) 내에서 직접 호출되어 특정 연산을 수행하고 결과값을 반환받는 데 가장 적합한 것은?

- A. Stored Procedure
- B. User-defined Function (UDF)
- C. External Table
- D. Materialized View

> **정답: B**

---

### 78. 반정형 데이터를 저장하는 VARIANT 데이터 타입이 **2026년 4월 공식 문서 기준** 한 행(Row)에 저장할 수 있는 **비압축(uncompressed)** 최대 크기는 얼마인가?

- A. 16 MB
- B. 64 MB
- C. 128 MB
- D. 무제한 (스토리지 용량에 따라 결정)

> **정답: C** — 2025_03 BCR 번들 활성화 이후 VARIANT/OBJECT/ARRAY의 **기본 최대 크기는 128 MB(비압축)** 로 확대되어 현재 전체 계정에 GA되어 있습니다(VARCHAR도 128 MB로 확장). 참고: 과거(번들 활성화 이전) 기준에서는 **16 MB**가 정답이었으므로 레거시 문항에서는 16 MB를 선택해야 할 수 있습니다.

---

### 79. JSON 데이터와 같은 반정형 데이터 내에 중첩된 배열(Array)을 개별 행(Row)으로 펼쳐서 관계형 테이블처럼 조회하고자 할 때 사용하는 함수는?

- A. PARSE_JSON
- B. FLATTEN
- C. GET_PATH
- D. UNNEST

> **정답: B**

---

### 80. Snowflake 아키텍처 외부(예: AWS Lambda, API Gateway)에서 실행되는 코드를 호출하여 결과를 Snowflake 쿼리 내에서 사용하려 할 때 정의해야 하는 오브젝트는?

- A. Stored Procedure
- B. External Function
- C. User-defined Function (UDF)
- D. External Table

> **정답: B**

---

### 81. Snowflake에서 '다이내믹 테이블(Dynamic Tables)'의 주요 목적은 무엇인가?

- A. 로컬 파일에서 실시간으로 데이터를 수집한다.
- B. 데이터 변환 파이프라인을 '대상 상태(Target State)'로 정의하여 자동으로 관리한다.
- C. 감사를 위해 과거 데이터를 쿼리하는 용도로만 사용된다.
- D. 특정 행에 대한 사용자 액세스 권한을 제어한다.

> **정답: B**

---

### 82. Snowflake 에코시스템에서 'dbt'는 데이터 라이프사이클의 어떤 단계에서 주로 사용되는가?

- A. 데이터 저장 및 아카이빙
- B. 온프레미스 소스에서의 데이터 수집
- C. 데이터 변환 및 파이프라인 오케스트레이션
- D. ID 공급자 및 SSO 연동

> **정답: C**

---

## 5단계: 공유 및 보호 — 도메인 5.0 예상 문제
> **도메인 5.0 Data Collaboration | 비중 10%**
> Time Travel, Fail-safe, Zero-copy Cloning, Secure Data Sharing, Reader Account, Snowflake Marketplace, Native Apps

---

### 83. 데이터 보호 기능 중 사용자가 직접 기간을 설정(0~90일)할 수 있으며, 실수로 삭제된 테이블을 'UNDROP' 명령어로 복구할 수 있는 기능은?

- A. Fail-safe
- B. Time Travel
- C. Zero-copy Cloning
- D. Database Replication

> **정답: B**

---

### 84. Snowflake 계정이 없는 외부 파트너에게 실시간으로 데이터를 공유하여 쿼리할 수 있게 하려 할 때, 공급자(Provider)가 비용을 부담하며 생성해주는 계정은?

- A. Consumer Account
- B. Admin Account
- C. Reader Account
- D. Managed Account

> **정답: C**

---

### 85. '보안 데이터 공유(Secure Data Sharing)'를 통해 소비자 계정과 공유할 수 있는 데이터베이스 오브젝트는 무엇인가? *(두 가지 선택)*

- A. 일반 테이블 (Tables)
- B. 태스크 (Tasks)
- C. 보안 뷰 (Secure Views)
- D. 마스킹 정책 (Masking Policies)

> **정답: A, C** — 2026.04 기준 공유 가능 오브젝트: **데이터베이스, 테이블, Dynamic Tables, 외부 테이블, Apache Iceberg™ 테이블(외부/관리형), Delta Lake 테이블, 일반·보안·MV·시맨틱 뷰, Cortex Search 서비스, UDF, USER_MODEL/CORTEX_FINETUNED/DOC_AI 모델**. 태스크, 마스킹/행 접근 정책, 저장 프로시저, 파이프, 시퀀스는 직접 공유 불가.

---

### 86. Snowflake 데이터 보호 기능 중 사용자가 보관 기간(Retention Period)을 0~90일 사이로 직접 제어할 수 있는 기능과, Snowflake가 7일간의 복구 기간을 강제적으로 관리하는 기능을 올바르게 짝지은 것은?

- A. Time Travel — Fail-safe
- B. Fail-safe — Time Travel
- C. Time Travel — Zero-copy Cloning
- D. Zero-copy Cloning — Fail-safe

> **정답: A**

---

### 87. Snowflake Marketplace에서 데이터를 구독하여 사용할 때의 주요 이점으로 옳은 것은?

- A. 데이터를 사용자의 스토리지로 복사(ETL)해야 하므로 데이터 일관성이 높다.
- B. 별도의 ETL 과정 없이 공유된 실시간 데이터에 즉시 접근하여 쿼리할 수 있다.
- C. 데이터 공급자의 웨어하우스 비용을 소비자가 전액 부담한다.
- D. 비정형 데이터만 공유가 가능하다.

> **정답: B**

---

### 88. Zero-copy Cloning 기능을 사용하여 1TB 크기의 테이블을 복제했을 때, 복제 직후의 스토리지 비용에 대한 설명으로 옳은 것은?

- A. 복제 즉시 1TB에 해당하는 추가 스토리지 비용이 발생한다.
- B. 메타데이터만 복제되므로 복제된 테이블에서 데이터 변경(DML)이 발생하기 전까지는 추가 스토리지 비용이 발생하지 않는다.
- C. 복제본은 원본 테이블의 스토리지 비용의 50%만 청구된다.
- D. Cloning은 임시(Temporary) 테이블에만 적용 가능하다.

> **정답: B**

---

### 89. 읽기 전용 계정(Reader Account)에서 발생하는 컴퓨팅(웨어하우스 사용) 비용은 누가 지불하는가?

- A. 데이터를 소비하는 외부 파트너(Consumer)
- B. 계정을 생성하고 데이터를 공유해준 공급자(Provider)
- C. Snowflake 본사
- D. 공급자와 소비자가 50%씩 분담한다.

> **정답: B**

---

### 90. '보안 데이터 공유(Secure Data Sharing)'를 통해 공유할 수 있는 데이터베이스 오브젝트는 무엇인가?

- A. 마스킹 정책 (Masking Policy)
- B. 태스크 (Task)
- C. 외부 테이블 (External Table) 및 Apache Iceberg™ 테이블
- D. 저장 프로시저 (Stored Procedure)

> **정답: C** — 외부 테이블과 Iceberg 테이블은 **직접(Direct) 공유 대상**입니다(2026.04 공식 문서 기준). 마스킹/행 접근 정책은 대상 오브젝트에 적용되어 함께 전파되지만 정책 자체를 공유하는 것은 아닙니다. Task·Stored Procedure는 공유 불가.

---

### 91. Snowflake 'Time Travel' 기능을 사용하여 수행할 수 있는 작업은 무엇인가?

- A. 지난 365일 이내에 생성된 모든 데이터 오브젝트 쿼리
- B. 지난 90일 이내에 삭제된 데이터 관련 오브젝트(테이블, 스키마 등) 복구
- C. 전체 기간에 걸친 데이터 사용 및 조작 분석
- D. 삭제된 지 7일이 지난 데이터의 강제 복구

> **정답: B**

---

### 92. Snowflake의 데이터 보호 기능 중 'Fail-safe'에 대한 설명으로 옳은 것은?

- A. 사용자가 직접 보관 기간을 0일에서 90일 사이로 설정할 수 있다.
- B. 데이터 보관 기간이 7일로 고정되어 있으며, 사용자가 이를 변경하거나 수동으로 삭제할 수 없다.
- C. Time Travel 기간이 시작되기 전에 먼저 실행되는 보호 계층이다.
- D. 가상 웨어하우스의 성능을 높여 데이터 복구 속도를 조절하는 기능이다.

> **정답: B**

---

### 93. 공급자(Provider)가 공유한 데이터를 소비자(Consumer)가 사용하는 '읽기 전용 계정(Reader Account)'에 대한 설명으로 옳은 것은?

- A. 소비자는 자신의 데이터를 해당 계정에 업로드할 수 있다.
- B. 소비자는 공유받은 데이터를 조회(Query)만 할 수 있고 수정이나 삭제는 불가능하다.
- C. 소비자가 직접 웨어하우스 비용을 지불해야 한다.
- D. 읽기 전용 계정은 Standard 에디션에서만 생성 가능하다.

> **정답: B**

---

### 94. 사용자가 제어할 수 없는 영역이며, Time Travel 보관 기간이 종료된 후 데이터 복구를 위해 Snowflake가 7일간 강제적으로 유지하는 기간은?

- A. Data Retention Period
- B. Fail-safe
- C. Metadata Retention
- D. Historical Snapshot

> **정답: B**

---

### 95. 'Zero-copy Cloning' 기능을 사용하여 데이터를 즉시 복제할 수 있는 오브젝트 수준이 아닌 것은?

- A. 데이터베이스 (Databases)
- B. 스키마 (Schemas)
- C. 내부 스테이지 (Internal Stages)
- D. 테이블 (Tables)

> **정답: C** — 스테이지는 복제 대상이 아닙니다.

---

### 96. 데이터 공급자(Provider)가 생성한 읽기 전용 계정(Reader Account)을 사용하는 소비자의 권한에 대한 설명으로 옳은 것은?

- A. 공급자가 공유해준 데이터를 기반으로 새로운 테이블을 생성하고 데이터를 추가할 수 있다.
- B. 공유받은 데이터를 조회할 수만 있으며, 해당 계정으로 데이터를 로컬에서 로드할 수는 없다.
- C. 리전이 다른 계정 간에도 데이터를 즉시 복제하여 저장할 수 있다.
- D. 소비자 자신의 데이터를 공급자의 계정으로 전송할 수 있는 전용 채널을 가진다.

> **정답: B**

---

### 97. 'Zero-copy Cloning'으로 생성된 복제본 테이블에 대한 스토리지 비용 청구 방식은?

- A. 복제된 시점부터 원본과 동일한 전체 스토리지 비용이 청구된다.
- B. 원본 데이터와 차이가 발생하는 시점(데이터 추가/수정/삭제)부터 변경된 데이터 부분에 대해서만 비용이 발생한다.
- C. 복제본은 항상 무료이며 원본 테이블 소유자가 비용을 부담한다.
- D. 복제본 테이블은 최대 24시간 동안만 유효하며 이후에는 자동 삭제되므로 비용이 없다.

> **정답: B**

---

### 98. Snowflake의 데이터 보호 기능인 'Time Travel'과 'Fail-safe'에 대한 설명 중 옳은 것은?

- A. 사용자는 Fail-safe 보관 기간을 직접 설정할 수 있다.
- B. Time Travel은 실수로 삭제된 테이블을 복구하는 데 사용되며, 사용자가 0~90일 사이로 기간을 조절할 수 있다.
- C. Fail-safe는 Time Travel 기간이 시작되기 전에 작동하는 예비 보호 기능이다.
- D. 두 기능 모두 데이터를 복제(Cloning)할 때만 활성화된다.

> **정답: B**

---

### 99. 사용자가 'Snowflake Native Apps'를 찾아 설치하고 실행할 수 있는 가장 적절한 장소는 어디인가?

- A. Snowflake CLI (SnowSQL)
- B. Snowpark Python 라이브러리
- C. Snowflake Marketplace
- D. 외부 클라우드 스토리지(S3 등)

> **정답: C**

---

### 100. 'Zero-copy Cloning' 기능을 사용하여 데이터베이스를 복제할 때, 기본 마이크로 파티션에는 어떤 일이 발생하는가?

- A. 즉시 새로운 스토리지 위치로 물리적 복사가 일어난다.
- B. 물리적 복사 없이 새로운 데이터베이스의 메타데이터에서 기존 파티션을 참조한다.
- C. 백업을 위해 하나의 ZIP 파일로 압축된다.
- D. 원본 데이터베이스에서 즉시 삭제된다.

> **정답: B**

---

> 이것으로 100개의 SnowPro Core (COF-C03) 예상 문제를 **공식 5개 도메인 순서**에 맞게 재편성한 문제집을 완성합니다.
