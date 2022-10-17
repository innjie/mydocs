# Amazon CloudWatch

-   지표 모니터링, 관리 → `경보 작업 구성` 웹 서비스
-   지표를 사용하여 시간 경과에 따른 처리 `그래프` 자동 
-   AWS 리소스 뿐 아니라 애플리케이션과 서비스에서 생성된 사용자 정의 지표 및 로그 파일 모니털이 가능
-   리소스 사용률, 성능, 운영 상태 파악 가능

## AWS Config
-   AWS 리소스 인벤토리, 구성 기록, 구성 변경 알림 제공 -> 보안 및 거버넌스 실현
-   리소스의 구성 세부 정보 확인 가능
-   규정 준수 감사, 보안 분석, 리소스 변경 추적, 문제 해결 가능

## CloudWatch 경보
-   임계값 상회 / 하회 → 자동으로 `경보` 생성

## 대시보드
-   단일 위치에서 리소스에 대한 모든 지표에 액세스 가능
-   사용자 지정 구성 가능

## AWS OpsWorks
-   구성 관리 서비스
-   서버 구성을 코드로 취급하는 자동화 플랫폼인 `Chef`를 사용
-   EC2 인스턴스 / 온프레미스 환경 전반에 걸쳐 서버 구성, 배포, 관리 작업 자동화
-   AWS OpsWorks for Chef Automate / AWS OpsWorks Stacks

## AWS Service Catalog
-   AWS에서 사용이 승인된 IT 서비스 카탈로그 생성, 관리 가능
-   필요한 승인 IT 서비스만 신속하게 배포 가능

## AWS CloudTrail
-   `계정에 대한 API 호출` 기록 - `로그의 추적`
-   API 호출자 ID, 호출 시간, 소스 IP 주소 등
-   운영 분석 및 문제 해결을 지원하기 위한 <mark>로그 필터링</mark>

## CloudTrail Insight

-   비정상적인 API활동 자동 감지

## AWS Trusted Advisor
-   AWS `환경 검사`, `실시간 권장 사항 제시` 웹 서비스
-   비용 최적화, 성능, 보안, 내결함성, 서비스 한도 비교
-   `녹색 체크` : 문제가 감지되지 않은 항목 수
-   `주황 삼각형` : 권장 조사 항목 수
-   `빨간색 원` : 권장 조치 수
-   오픈 액세스 권한이 설정된 보안 검사 시행

## <mark>Amazon QuickSight</mark>
-   AWS 비용 및 사용 `보고서(CUR)를 수집하고 시각화`

## AWS Personal Health Dashboard
-   고객에게 영향을 주는 이벤트 발생 시 수정 지침 제공
-   AWS 서비스 성능 및 가용성에 대한 맞춤형 보기 제공

## AWS Managed Services
-   인프라 지속 관리 서비스
-   일반적인 활동 자동화, 인프라 지원 서비스 제공

## Amazon EC2 Systems Manager
-   소프트웨어 인벤토리 수집, OS 패치 적용, 시스템 이미지 생성 관리 서비스
-   온프레미스 데이터 센터로 확장되는 관리 접근 방식 제공

```ad-hint
title: 기능

1.  Run Command
-   쉘 스크립트 / 파워쉘 원격 실행 / SW 업데이트 설치, OS, EC2 등 관리 작업 자동화

2.  State Manager
-   OS 구성 일관성 있게 정의, 유지
-   대용량 인스턴스 구성 모니터링, 정책 지정 등

3.  재고
-   인스턴스에 대한 구성 및 정보 수집, 쿼리 수행 도움
-   시스템 구성 추적 및 감사

4.  유지 관리 기관
-   유지관리 작업 반복 시간대 정의

5.  Patch Manager
-   OS 및 SW 패치 자동 선택, 배포도움

6.  자동화
-   Amazon 머신 이미지 (AMI) 같은 일반적인 유지 관리 및 배포 간소화

7.  Paramter Store
-   암호 및 데이터베이스, 문자열과 같이 중요한 관리 정보 저장할 위치 제공
```

# 분석
## Amazon Athena
-   표준 SQL을 사용해 S3에 저장된 데이터 분석하는 대화식 쿼리 서비스
-   서버리스 서비스 -> 인프라 관리하지 않음

## Amazon EMR
-   동적 확장 가능 EC2 인스턴스 전반에 걸쳐 대량의 데이터를 효율적으로 처리하는 `하둡 프레임워크` 제공
-   분산 프레임워크 (Apache Spark, HBase, Presto, Flink) 실행
-   데이터 스토어의 데이터와 상호작용 가능

## Amazon CloudSearch
-   검색 솔루션 효율적으로 설정 가능

## Amazon Elasticsearch Service
-   Elasticsearch 배포, 운영, 확장
-   분석 도구 통합 제공

## Amazon Kinesis
-   스트리밍 데이터 로드 및 분석 서비스

### 1. Amazon Kinesis Firehose
-   스트리밍 데이터 캡쳐 및 변형 후 로드 -> `실시간에 가까운 분석`
-   데이터 처리량에 대응하여 자동으로 확장, 지속적 관리 필요 없음

### 2. Amazon Kinesis Analytics
-   표준 SQL을 통해 실시간 스트리밍 데이터 처리 가능한 가장 쉬운 방법
-   비즈니스 및 고객 요구에 신속하게 대응 가능

### 3. Amazon Kinesis Streams

-   특수요구에 맞는 데이터 처리 및 애플리케이션 구축 가능
-   다중 소스에서 대량의 데이터를 지속적으로 캡쳐 및 저장 가능
-   kCL 사용하여 동적 요금 및 알림 등 작업 수행 가능
-   다른 AWS 서비스로 데이터 전달 가능

---