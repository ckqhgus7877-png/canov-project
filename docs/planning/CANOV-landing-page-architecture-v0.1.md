# CANOV 랜딩 페이지 기술·보안 결정 v0.1

작성일: 2026-09-16  
상태: 랜딩 페이지 비공개 미리보기 구현 기준. 운영 개인정보 정책과 외부 메시지 연동은 후속 확정한다.

## 1. 확정 기술 조합

| 영역 | 선택 | 역할 |
|---|---|---|
| 웹 | Astro + TypeScript | 브랜드 콘텐츠, 신청 화면, 서버 엔드포인트 |
| 데이터 | 별도 Supabase 테스트 프로젝트 | 테스트 단지·신청 데이터 저장 |
| 실행·배포 | Cloudflare Workers | 정적 자산 제공과 신청 API 실행 |
| 미리보기 보호 | Cloudflare Access | 승인된 이메일·계정 기반 접근 |

최신 Astro Cloudflare 어댑터의 동적 기능은 Cloudflare Workers를 대상으로 한다. 기존 문서의 Cloudflare Pages 후보는 랜딩 페이지 범위에서 Workers로 교체한다.

## 2. 첫 구현 요청 흐름

`브라우저 → Cloudflare Access 인증 → Astro 화면 → Workers 신청 API → 서버 검증·중복 방지 → Supabase 테스트 저장소 → 접수 식별값 반환 → 완료 화면`

- 신청 API는 허용된 필드만 받는다.
- 필수값, 길이, 형식과 허용값을 서버에서 다시 검증한다.
- 접수 식별값과 접수 시각은 서버 또는 데이터베이스에서 생성한다.
- 중복 방지 키를 사용해 같은 제출이 반복 저장되지 않게 한다.
- 데이터베이스 저장이 성공한 뒤에만 완료 응답을 반환한다.
- 후속 카카오톡·SMS 자동 알림은 신청 저장 이후의 별도 처리로 추가한다.

## 3. Supabase 직접 INSERT 대안

브라우저에서 `supabase-js`로 직접 INSERT하는 방식은 서버 계층을 줄일 수 있으므로 다음 조건을 모두 만족하는 단순 제출 기능의 대안으로 유지한다.

- 데이터 저장 외에 서버 검증, 중복 방지, 외부 알림과 비밀 키 사용이 필요하지 않다.
- 브라우저에는 publishable key 또는 기존 프로젝트의 legacy anon key만 제공한다.
- 익명 역할은 지정된 테이블의 INSERT만 가능하며 SELECT·UPDATE·DELETE 권한은 없다.
- RLS 정책과 별도로 데이터 타입, 길이, 필수값, 허용값과 고유 제약을 데이터베이스에서 강제한다.
- 자동화된 대량 제출을 줄일 속도 제한 또는 봇 방지 수단을 둔다.

`WITH CHECK (true)`만 있는 익명 INSERT 정책은 누구든 임의 형태의 요청을 반복할 수 있으므로 충분한 최종 정책이 아니다.

현재 범위는 접수 식별값, 서버 시각, 중복 방지와 후속 자동 알림 확장을 요구하므로 첫 구현에는 Workers 신청 API를 사용한다.

## 4. 키와 최소 권한

- 브라우저에 서버용 secret key 또는 legacy service role key를 포함하지 않는다.
- 서버용 키는 Cloudflare Workers의 암호화된 환경 변수에만 저장한다.
- `.env` 실제 값, 대시보드 토큰, 접근 토큰과 고객 데이터는 Git에 저장하지 않는다.
- 브라우저에 Supabase 키가 필요하다면 publishable key만 사용하고 RLS와 최소 권한을 함께 적용한다.
- 서버용 키는 RLS를 우회할 수 있으므로 Workers가 요청 필드와 권한을 검증한 뒤 제한된 작업만 수행한다.
- 가능하면 신청 쓰기와 관리자 읽기 경로를 분리하고 관리자 조회는 Access 인증 후에만 허용한다.
- 로그에는 키, 전체 전화번호와 신청 원문을 남기지 않는다.

Supabase의 현재 권장 명칭은 브라우저용 `publishable key`와 서버용 `secret key`다. `anon`과 `service_role`은 기존 프로젝트에서 볼 수 있는 legacy 명칭으로만 취급한다.

## 5. RLS·권한 기준

- `registrations`와 외부에 노출되는 모든 테이블에 RLS를 활성화한다.
- 역할별 GRANT와 RLS 정책을 함께 검토한다.
- 선택한 Workers 구조에서는 브라우저의 직접 테이블 접근을 기본 차단한다.
- 직접 INSERT 대안으로 변경할 때만 익명 INSERT 권한과 제한 정책을 추가한다.
- 익명 사용자의 SELECT·UPDATE·DELETE는 허용하지 않는다.
- 관리자 조회는 인증된 별도 경로에서 최소 필드만 반환한다.
- 허용·차단 시나리오를 데이터베이스 정책 테스트로 검증한다.

## 6. Cloudflare Access 적용 범위

- Access는 `staging` 또는 `preview` 전용 호스트에만 적용한다.
- 승인된 이메일·계정 또는 이메일 일회용 인증번호처럼 Access가 검증하는 인증 방식만 허용한다.
- 공유 비밀번호가 반드시 필요해지면 별도의 서버측 보호 수단을 검토하며 클라이언트 코드에서 비밀번호를 비교하지 않는다.
- 실제 고객이 사용하는 공개 운영 도메인에는 Access를 적용하지 않는다.
- 검색엔진 차단은 Access와 함께 적용하지만 접근 제어를 대신하지 않는다.
- 자동 테스트가 필요하면 전용 우회 토큰을 사용하고 토큰을 Git과 로그에 남기지 않는다.

## 7. 비용 기록 원칙

비용은 변경 가능한 외부 조건이므로 구현 요구사항에 고정하지 않고 배포 직전에 공식 요금표를 다시 확인한다.

2026-09-16 공식 자료 기준 참고값:

- Cloudflare Workers Free: 하루 100,000건 요청, 호출당 CPU 10ms 한도.
- Workers Paid: 월 최소 5달러, 월 1,000만 건 포함, 초과 요청 100만 건당 0.30달러. CPU 사용량은 별도 기준이 있다.
- 정적 자산 요청은 Workers 요청 과금에서 제외된다.
- Cloudflare Zero Trust Free: 최대 50명. 50명 초과 유료 요금은 현재 사용자당 월 7달러다.
- Supabase Free: 프로젝트당 데이터베이스 500MB. Pro는 월 25달러부터다.

신청 1건의 실제 크기는 컬럼, 인덱스, 로그와 부가 데이터에 따라 달라지므로 `500MB = 신청 50만 건`처럼 고정 환산하지 않는다.

## 8. 구현 전 남은 결정

- 실제 신청 개인정보 처리 안내 문구와 법적 근거 최종 검토
- CANOV 보유 도메인, 개인정보 역할 주소용 유료 메일 제공업체·요금제, 인증번호 제공업체·요금제·세부 시도 한도·예산 통제, 등록 휴대전화 이용 불가 시 본인 확인 예외 절차와 카카오톡·CS 보조 문의 경로
- 운영 단지 기준정보
- 공개 운영 도메인과 미리보기 호스트 이름
- 승인된 카카오톡 채널 주소

관리자 확인은 Cloudflare Access로 보호된 읽기 전용 최소 화면으로 확정한다. 목록과 상세 조회만 제공하며 수정·삭제·내보내기는 전체 관리자 웹 단계까지 제외한다.

개인정보 권리 요청용 휴대전화 인증번호는 6자리·5분·일회용으로 처리한다. 재발송하면 이전 번호를 폐기하고 반복 실패·과다 발송을 제한하며, 인증번호 원문은 클라이언트·로그·AI에 노출하지 않는다. 이는 첫 랜딩 신청 제출 인증이 아니라 개인정보 권리 요청자의 본인 확인 기준이다.

인증번호 발송 서비스는 CANOV 운영 주체 명의의 업무용 계정으로 계약하고 친구가 관리자·결제·복구 권한을 인수한다. 개발자는 분리된 최소 권한만 사용하고 인계 시 접근 회수와 키 교체를 수행한다. 운영 주체와 요금제가 승인되기 전에는 업체 가입·결제·실제 연동을 시작하지 않는다. 낮은 초기 월 예산 한도와 단계별 알림을 적용하고 한도·비정상 사용 시 자동 발송을 중단하되 등록 번호 확인 전화로 전환한다. 구체 금액은 실제 단가·예상 정상 사용량·테스트 결과를 바탕으로 친구가 승인한다.

제공업체는 공개 준비 단계에서 운영 주체·발신 번호·예상 건수가 확정된 뒤 공식 업체 2~3곳을 같은 기준으로 비교해 선정한다. 선정 전 개발은 교체 가능한 모의 발송 모듈을 사용하며 실사용 계정·결제·키를 만들지 않는다. 승인 후 테스트 번호로 발송·보안·비용·장애 전환을 검증한 뒤 실제 연동한다.

## 9. 확인한 공식 자료

2026-09-16 확인:

- [Astro Cloudflare 어댑터](https://docs.astro.build/en/guides/integrations-guide/cloudflare/)
- [Astro 서버 엔드포인트](https://docs.astro.build/en/guides/endpoints/)
- [Supabase API 키](https://supabase.com/docs/guides/getting-started/api-keys)
- [Supabase 데이터 보안](https://supabase.com/docs/guides/database/secure-data)
- [Supabase Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security)
- [Cloudflare Workers 요금](https://developers.cloudflare.com/workers/platform/pricing/)
- [Cloudflare Zero Trust 요금](https://www.cloudflare.com/plans/zero-trust-services/)
