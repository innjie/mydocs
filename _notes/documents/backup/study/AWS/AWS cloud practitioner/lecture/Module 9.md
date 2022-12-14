# Cloud Adoption Framework

## 1. 비즈니스 관점

-   비즈니스 요구 사항 반영, 투가자 결과와 연계되도록 보장

### 역할

-   비즈니스 관리자
-   재무 관리자
-   예산 소유자
-   전략 이해당사자

## 2. 인력 관점

-   클라우드 채택을 성공하기 위한 조직 관리 전략 개발
-   조직 구조 역할 / 새로운 기술 및 프로스세스 요구 사항 → 교육, 인력 배치, 조직 변화의 우선순위 지정

### 역할

-   인사 관리
-   인력 배치
-   인력 관리자

## 3. 거버넌스 관점

-   IT전략이 비즈니스 전략에 부합하도록 조정
-   직언 기술 및 프로세스 업데이트 방법 제공

### 역할

-   최고 정보 책임자 (ICO)
-   프로그램 관리자
-   엔터프라이즈 아키텍트
-   비즈니스 분석가
-   포트폴리오 관리자

## 4. 플랫폼 관점

-   클라우드 기반 솔루션 구현 / 온프레미스 마이그레이션
-   인프라 설계, 구현 및 최적화

### 역할

-   최고 기술 책임자 (CTO)
-   IT 관리자
-   솔루션스 아키텍트

## 5. 보안 관점

-   가시성, 감사 가능성, 제어 및 민첩성 보안 목표 충족

### 역할

-   최고 정보 보안 책임자 (CISO)
-   IT 보안 관리자
-   IT 보안 분석가

## 6. 운영 관점

-   비즈니스 이해당사자와 합의된 수준까지 IT 워크로드 구현, 실행, 사용, 운영 및 복구

### 역할

-   IT 운영 관리자
-   IT 지원 관리자

---

# 6가지 마이그레이션 전략

## 1. 리호스팅 (Rehosting)

-   `리프트 앤 시프트 (lift and shift)` - 애플리케이션 `변경 없이` 이전
-   마이그레이션 구현, 확장 → 레거시 시스템의 마이그레이션

## 2. 리플랫포밍 (Reflatforming)

-   `리프트 앤 시프트 및 수정 (lift, tinker and shift)`
-   클라우드 최적화 - `핵심 아키텍처 변경 X`

## 3. 리팩터링 (Refactoring) / 아키텍처 설계 (Re-architecting)

-   클라우드 네이티브 기능 → 애플리케이션 설계, 개발
-   기능 추가, 확장, 성능 개선의 필요성이 큰 경우

## 4. 재구매 (Repurchasing)

-   `Saas` 모델로 전환
-   다른 제품으로 전환

## 5. 유지 (Retaining)

-   소스 환경에 유지

## 6. 폐기 (Retiring)

-   필요하지 않은 애플리케이션 제거

# Migration
## AWS Application Discovery Service
-   온프레미스 데이터센터에서 실행되는 앱
-   종속성 및 성능 프로파일 자동 식별
-   필요 인프라를 자동 수집 후 목록 형식으로 작성

## AWS Database Migration Service
-   가장 널리 사용되는 상용 및 오픈소스 DB로부터 데이터 마이그레이션 가능
-   오라클 - 아마존 오로라 / ms- mysql 등 이기종 플랫폼 마이그레이션 가능
-   Redshift로 자원 스트리밍 -> 데이터 분석 가능

## AWS Server Migration Service
-   에이전트 없는 서비스
-   AWS SMS -> 라이브 서버 볼륨의 증분식 복제를 자동화, 예약, 추적 가능


---

# AWS Snow 패밀리 멤버

-   AWS - 고객 간 최대 엑사바이트 데이터를 `물리적으로 이동` 하는 `디바이스 모음`

## 1. AWS Snowcone

-   작은 엣지 컴퓨팅 및 데이터 전송
-   CPU 2 / RAM 4GB / STORAGE 8TB

## 2. <mark>AWS Snowball</mark>
-   대규모 데이터 전송 솔루션
-   데이터 보안 및 완전한 연계보관성 보장
-   데이터 보호를 위한 다중 보안 계층 사용
-   전송 작업 처리 이후 소프트웨어 삭제 수행

### 1. Snowball Edge Storage Optimized

-   대규모 마이그레이션 / 반복 전송 워크플로 / 대용량 로컬 컴퓨팅
-   `스토리지` : 블록 볼륨, S3 호환용 80TB HDD, 1TB SSD
-   `컴퓨팅` : EC2 인스턴스 지원용 40개의 vCPU와 80GiB 메모리
- <mark>오프라인에서</mark>데이터 수집, 지정 처리 후 데이터 이동

### 2. Snowball Edge Compute Optimized

-   기계 학습, 풀 모션 비디어 분석, 분석 및 컴퓨팅 스택
-   `스토리지` : S3 호환 객체 스토리지, EBS 호환 블록용 42TB HDD, 7.68TB NVMe SSD
-   `컴퓨팅` : 52개 vCPU, 208GiB 메모리, NVIDIA Tesla V100 GPU 등

## 3. AWS Snowmobile

-   대용량 데이터를 AWS로 이동하는데 사용하는 엑사바이트 규모 데이터 전송 서비스
-   `스토리지` : 100PB

## AWS Snowball Edge
-   온보드 스토리지 및 컴퓨팅 기능 포함 데이터 전송 디바이스
-   전송 중 전담 인력, GPS 추적, 모니터링, 감시 등 다중 보안 계층 사용
-   AWS KMS를 통해 관리됨
- <mark>EC2를 기본적으로 지원</mark>

---

# AWS 서비스를 통한 혁신

## 1. 서버리스 애플리케이션

-   사용자가 서버를 프로비저닝, 유지관리 필요 없음

## 2. 인공 지능

-   `Amazon Transcribe` - 음성 → 텍스트 변환
-   `Amazon Comprehend` - 텍스트 패턴 검색
-   `Amazon Fraud Detector` - 잠재적인 온라인 사기 행위 식별
-   `Amazon Lex` - 음성 및 텍스트 챗봇 빌드
- <mark>Amazon polly</mark> - 텍스트 -> 음성 변환 (transcribe와 반대)
- AmazonRekognition - 이미지 분석 / 딥러닝 기반 시각 검색 및 이미지 분류 기능 추가 가능

## 3. 기계 학습
-   `Amazon SageMaker` : 모델 빌드, 훈련, 배포

## 4. 모바일 서비스
```ad-hint
title: 기능

-   앱 분석
-   앱 콘텐츠 전송
-   클라우드 로직
-   NoSQL DB
-   푸시 알림
-   사용자 데이터 스토리지
-   사용자 로그인
-   커넥터
-   대화형 봇
-   사용자 참여
```

## Amazon Cognito
-   로그인 기능 추가 서비스
-   디바이스에 데이터 로컬 저장 - 오프라인 상태일때 동작가능

## Amazon Pinpoint
-   캠페인을 생성하여 모바일 앱에서 사용자 참여 유도
-   사용자 행동 기반 푸시 알림으로 참여율 증가

## AWS Device Farm
-   앱 테스트 서비스
-   상호작용 및 실시간 디바이스 문제 재현

## AWS Mobile SDK
-   다양한 AWS 서비스 액세스 가능

## Amazon Mobile Analytics
-   앱 사용량 및 수익 측정 가능
-   S3과 Redshift로 커스텀 분석 실행 가능

## 5. 애플리케이션 서비스
## AWS Step Functions
-   시각적 워크플로 사용- 분산 앱 및 구성요소 조정 가능
-   구성 요소를 단계적으로 배열 및 시각화 가능한 그래픽 콘솔 제공
-   각 단계를 트리거 및 추적, 오류 발생 시 재시도

## Amazon API Gateway
-   API를 생성, 유지 관리 가능한 완전 관리형 서비스
-   백엔드 서비스로부터 로직 또는 기능에 액세스 가능

## Amazon Elastic Transcoder
-   미디어 트랜스코딩 기능 제공

## Amazon SWF
-   병렬적 / 순차적 백그라운드 작업을 빌드, 실행, 조정 가능
-   완전관리형 상태 추적기 / 작업 조정자 기능

## Cloud formation
- 코드로서 인프라 설명