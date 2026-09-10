# CANOV 현재 진행 상황

마지막 갱신: 2026-09-10

## 현재 단계

기획 디벨롭 및 개발 환경 결정 단계입니다. 아직 프론트엔드·백엔드 구현은 시작하지 않았습니다.

## 확정된 방향

- 서비스명: 카노브 / CANOV
- 아파트 입주민 대상 정기 출장세차 서비스
- 고객 접점: 브랜드 홈페이지와 메신저
- 운영 도구: 디테일러 Android 앱과 관리자 웹
- Android MVP를 먼저 개발하고 Google Play 출시를 목표로 함
- 브랜드 핵심: 차량별 Care Profile, 누적 관리 이력, 표준 작업 기준과 품질 확인
- Claude가 개발 및 1차 오류 수정, Codex가 2차 검증 수행

## 현재 문서

- `docs/planning/CANOV-PRD-v0.3-brand-development.md`
- `docs/planning/CANOV-development-tools-plan.md`
- `docs/references/CANOV-Brand-Story.pdf`

## 다음 작업

1. 공통 개발 규칙 프롬프트 작성
2. 데이터 구조와 권한 명세 작성
3. Care Profile의 필수 정보와 수정 권한 결정
4. 현장 작업 상태와 품질 확인 기준 결정
5. 섹션별 Claude 구현 프롬프트 작성

## 미정 사항

- PDF의 `정비`가 실제 서비스 범위인지 포괄적 표현인지
- 정기 상품, 방문 주기, 가격, 결제 방식
- 사용할 메신저와 자동 알림 방식
- Care Profile에서 고객에게 보여줄 정보와 내부 기록의 구분
- 품질 확인 주체와 완료 조건
- 추천 기술 조합의 최종 확정

## 작업 재개 방법

새 기기에서 저장소를 받은 다음 이 문서와 최신 Git 기록을 확인합니다. Claude 또는 Codex에게 현재 작업 섹션과 기준 커밋을 전달하고 작업을 이어갑니다.

