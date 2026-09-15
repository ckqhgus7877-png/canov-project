# CANOV 현재 진행 상황

마지막 갱신: 2026-09-16

## 현재 단계

사용자·우현과 19개 항목의 기획 Q&A를 진행 중입니다. 결정사항은 진행 중 체크포인트로 저장했으며 아직 통합 PRD 확정본은 아닙니다. 랜딩 페이지 구현 프롬프트 초안은 작성했지만 프론트엔드·백엔드 구현은 시작하지 않았습니다.

## 확정된 방향

- 서비스명: 카노브 / CANOV
- 아파트 입주민 대상 정기 출장세차 서비스
- 고객 접점: 브랜드 홈페이지와 메신저
- 운영 도구: 디테일러 Android 앱과 관리자 웹
- Android MVP를 먼저 개발하고 Google Play 출시를 목표로 함
- 브랜드 핵심: 차량별 Care Profile, 누적 관리 이력, 표준 작업 기준과 품질 확인
- Claude가 개발 및 1차 오류 수정, Codex가 2차 검증 수행
- Claude 기반 고객 CS 자동화를 제품 목표에 포함
- 세차 상품과 제공 범위는 관련 자료 수령 후 디벨롭
- 첫 개발 대상은 랜딩 페이지이며 이후 고객 신청·백엔드·관리자 웹·Android 앱 순으로 진행
- 랜딩 페이지는 로컬 기본 검사 후 비공개 미리보기 주소에서 검증하고, 사용자 승인 후 공개
- 비공개 미리보기는 테스트 저장소에 연결해 신청 입력·검증·제출·관리자 확인까지 검증하고 실제 개인정보는 사용하지 않음
- 신청 완료 화면에 접수 결과·연락 절차·카카오톡 상담 버튼을 먼저 제공하고, 실제 카카오톡·SMS 자동 알림은 후속 운영 단계에서 추가
- 비공개 미리보기는 비밀번호 또는 승인 계정으로 보호하고 검색 차단만으로 접근 보호를 대신하지 않음
- 랜딩 페이지 기술 조합은 Astro + TypeScript, Supabase 테스트 프로젝트, Cloudflare Workers + Access로 확정
- 브라우저의 Supabase 직접 INSERT는 단순 저장 대안으로만 유지하고, 첫 구현은 Workers API를 사용
- 테스트 신청은 Cloudflare Access로 보호된 읽기 전용 최소 확인 화면에서 확인하며 수정·삭제·내보내기는 제외
- 타이어 공기압·워셔액 같은 간단한 점검과 보충을 서비스 방향에 포함하되 수리·분해·진단은 제외
- 간단한 상태 점검은 기본 제공하고 실제 보충은 고객 동의와 차량별 기준 확인 후 진행
- 보충 동의는 Care Profile에 차량별·항목별로 기록하며 고객이 언제든 철회할 수 있고 실제 보충 결과를 방문 리포트로 안내

## 현재 문서

- `docs/planning/CANOV-PRD-v0.4-cs-automation.md`
- `docs/planning/CANOV-development-tools-plan.md` v0.2
- `docs/planning/CANOV-QNA-checkpoint-v0.1.md`
- `docs/planning/CANOV-landing-page-acceptance-v0.1.md`
- `docs/planning/CANOV-landing-page-architecture-v0.1.md`
- `docs/references/CANOV-Brand-Story.pdf`
- `prompts/08-landing-page.md`

## 다음 작업

1. Q&A 체크포인트의 `Q&A 재개 지점`에서 첫 운영 버전의 보충 동의 변경 경로 결정
2. 우현의 세차 상품·제공 범위 자료 반영
3. 실제 신청 개인정보 항목·동의 문구·보관 기간 결정
4. 전체 Q&A 결과를 통합 PRD로 정리
5. 사용자 승인 후 `prompts/08-landing-page.md`로 구현 시작

## 미정 사항

- 세차 상품 세부 제공 범위와 차종 분류: 우현 자료 수령 대기
- 항목별 보충 동의의 변경 경로, 가격과 상세 작업 기준
- 결제대행사와 환불 기준

## 작업 재개 방법

새 기기에서 저장소를 받은 다음 이 문서와 최신 Git 기록을 확인합니다. Claude 또는 Codex에게 현재 작업 섹션과 기준 커밋을 전달하고 작업을 이어갑니다.
