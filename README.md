# CANOV Project

아파트 입주민을 위한 정기 출장세차 서비스 **카노브(CANOV)**의 기획 및 개발 저장소입니다.

## 현재 방향

- 고객: 홈페이지와 메신저에서 서비스 확인 및 신청
- 디테일러: Android 앱에서 예약, 차량 정보, 작업 전후 기록 관리
- 운영자: 관리자 웹에서 고객, 차량, 예약, 배정, 작업 결과 관리
- 핵심 가치: 반복 예약 부담 감소, 차량별 Care Profile, 누적 관리 이력, 표준화된 작업 품질
- 고객 운영 목표: 서버측 AI 기반 고객 CS 자동화와 필요 시 상담원 연결. 첫 CS 기능은 OpenAI API로 시험하고 운영용 AI 제공업체는 개발 도구와 별도로 최종 선정
- 첫 출시 대상: Android

## 협업 방식

- Codex: 기획 정리, 프론트엔드·백엔드 구현, 테스트, 오류 수정과 인계를 주도
- Claude: 중요한 변경의 추가 검토. Codex가 필요성을 제안하고 전달용 프롬프트를 작성
- 사용자: Claude에 프롬프트 전달 및 검토 결과 회신, 사업 정책과 브랜드 방향 결정, 현장 피드백, 출시 승인

2026-09-29 사용자 확정 기준입니다. 인증·권한, 개인정보, DB 구조·마이그레이션, 결제, AI 업무 실행 범위 등 중요한 변경은 Codex가 Claude 검토를 제안합니다. 사용자가 가져온 지적의 타당성을 Codex가 확인해 수정·재검증합니다. 상세 절차는 `docs/planning/CANOV-development-tools-plan.md`, 새 채팅의 작업 시작 지침은 `AGENTS.md`를 따릅니다.

OpenAI API 사용은 첫 CS 기능의 시험 제공업체 결정이며 최종 운영 제공업체 확정이 아닙니다. 실제 상담 품질·응답 속도·비용을 같은 기준으로 기록한 뒤 운영 채택 여부를 결정합니다.

작업을 다른 기기에서 이어갈 때는 `docs/handoff/CURRENT.md`를 먼저 확인합니다.

완료된 58개 Q&A 결정사항과 외부 자료 대기 항목은 `docs/planning/CANOV-QNA-checkpoint-v0.1.md`에 정리되어 있습니다.

랜딩 페이지의 완료 조건과 핵심 검증 시나리오는 `docs/planning/CANOV-landing-page-acceptance-v0.1.md`를 기준으로 합니다.

랜딩 페이지의 확정 기술 조합과 보안 원칙은 `docs/planning/CANOV-landing-page-architecture-v0.1.md`에 정리되어 있습니다.

화면 디자인은 [CANOV 라이브 랜딩페이지](https://canov-landing-mvp.vercel.app/)를 우선 참고하며, 확인된 시각 언어와 적용 경계는 `docs/planning/CANOV-design-reference-v0.1.md`에 정리되어 있습니다.

첫 공개 신청서의 개인정보 항목과 보유·파기 기준은 `docs/planning/CANOV-privacy-data-policy-v0.1.md`를 기준으로 합니다.

## 폴더 구조

```text
docs/
  planning/    기획서와 개발 계획
  references/  브랜드 원본 및 참고자료
  handoff/     현재 진행 상태와 다음 작업
prompts/       Codex 구현 및 필요 시 Claude 전달용 검토 프롬프트
reviews/       Codex 검증 결과와 Claude 검토·반영 기록
apps/          디테일러 앱, 관리자 웹, 브랜드 홈페이지
backend/       데이터 구조와 서버 처리
```

현재는 기획 단계이므로 일부 폴더는 개발 시작 시 생성합니다.

## 문서 작성 원칙

- 사업 정책이 정해지지 않은 내용은 `미정` 또는 `제안`으로 표시합니다.
- 요구사항은 화면, 데이터, 권한, 정상 흐름, 예외 흐름, 완료 조건을 함께 작성합니다.
- 비밀번호, 토큰, 서비스 키, 앱 서명 키와 실제 고객 개인정보는 Git에 저장하지 않습니다.
