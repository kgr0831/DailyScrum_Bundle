# 현재 운영 기준

이 문서는 2026-10-06의 구현과 사용자가 전달한 운영 설정을 정리한 것입니다. 이전 시제품 문서보다 현재 운영 판단에 우선합니다. 이번 폴더 정리에서 클라우드 예약·운영 데이터·서비스 설정을 변경하지 않았습니다.

## 두 보고서 공간

| 구분 | 겜마루 | 공모전·채용 |
| --- | --- | --- |
| 경로 | `/reports` | `/personal` |
| 조사 대상 | 동아리가 직접 받을 운영비·물품·장비·공간·단체 서비스 지원 | 개발·AI·IT 공모전과 채용공고, 트레이딩 제외 |
| 읽기 권한 | Discord 로그인 후 관리자 승인 | Discord 로그인 즉시, 승인 없음 |
| 진행 상태·메모 | 동아리 공유 | Discord 계정별 서버 저장 |
| 일일 알림 | 겜마루 봇 서버의 데일리-스크럼 | 별도 데일리 스크럼 서버의 데일리-스크럼 |
| 실제 발송 | Dishost 워커 | Vercel에서 Discord REST API 호출 |
| 업로드 로그인 | `/reports/login/publisher` | `/personal/login/publisher` |
| 업로드 키 변수 | `FUNDING_PUBLISHER_TOKEN` | `PERSONAL_PUBLISHER_TOKEN` |
| 조사 자료 | `/reports/upload?guide=1` | `/personal/upload?guide=1` |
| 저장 경로 | `gammaru/briefs/` | `personal/briefs/` |

공모전·채용 보고서는 로그인한 이용자들이 같은 HTML을 읽습니다. 이용자별 맞춤 HTML을 매일 생성하는 구조는 아닙니다. 일반 이용자의 상태·메모는 각각 분리되며, 다음 자동 조사에는 기존 운영자의 조사 조건·진행 기록이 반영됩니다. 상세 비공개 프로필·메모 원문은 발행 HTML이나 채널 요약에 싣지 않습니다.

## 수집 → 발행 → 알림

1. dots가 클라우드에서 최신 지침·디자인·동아리 정보 또는 비공개 조사 조건과 `research-state-json`을 읽습니다.
2. 공식 원문을 조사하고 기존 ID·상태·메모를 반영해 HTML과 비실행 JSON 목록을 만듭니다.
3. 발행 직전 `stateVersion`과 오늘 보고서 날짜·버전을 다시 읽습니다. 상태가 바뀌면 실제 내용을 반영해 다시 미리보기합니다.
4. 사이트에서 HTML 업로드 또는 전체 내용 붙여넣기 → 미리보기 → 발행 확인을 진행합니다. 매일 재배포할 필요는 없습니다.
5. 사이트가 알림 작업을 만들고 지정된 전송 경로가 요약과 URL을 게시합니다. 현재 날짜·버전·전달 대상의 **전송 완료**를 확인합니다.

상태 버튼과 메모 저장은 사이트가 담당합니다. dots가 만든 HTML 안에 실행 JavaScript나 상태 변경 폼을 넣지 않습니다. 사용자가 요청한 수정본만 재발행하며, 이전 HTML·진행 상태·메모를 보관합니다. 업로드 성공이나 대기 0건만으로 알림 성공을 판단하지 않습니다.

## 설정 위치

웹사이트는 [code/.env.example](../code/.env.example), 봇은 [봇 환경변수 예제](../code/discord-notifier/.env.example)를 기준으로 설정합니다. 토큰 값은 이 보관본에 없습니다.

- 웹사이트: Private Blob, 양쪽 관리자·업로드 키, 공통 Discord OAuth, 서로 다른 채널 ID를 설정합니다.
- 겜마루 워커: 웹사이트와 Dishost의 `DISCORD_WORKER_TOKEN`이 같아야 하며 웹사이트는 `DISCORD_DELIVERY_MODE=worker`를 사용합니다.
- 공모전·채용: `PERSONAL_DISCORD_REPORT_CHANNEL_ID`를 사용합니다. 워커 모드와 별개로 Vercel에서 직접 보내므로 **Vercel의 `DISCORD_BOT_TOKEN`도 유지**합니다.
- 봇: `REPORTS_SITE_URL`, `DISCORD_BOT_TOKEN`, `DISCORD_WORKER_TOKEN`, `DISCORD_POLL_SECONDS`만 필요합니다. 관리자·업로드·Blob 키는 전달하지 않습니다.
- OAuth: 두 공간 모두 `<사이트 원점>/reports/auth/callback`을 사용합니다.
- Vercel 구성의 함수 리전은 `icn1`입니다. 기존 연결 정보 `.vercel/`은 복사하지 않았습니다.

같은 운영 Blob을 새 실행본에 연결하면 기존 회원·보고서 데이터에 접근하게 됩니다. 개발용 설정은 운영용과 분리해 직접 입력합니다. 이 폴더는 운영 데이터 백업이 아닙니다.

## dots 예약과 프롬프트

[prompts/README.md](../prompts/README.md)에서 대상에 맞는 프롬프트 하나를 사용합니다. 현재 두 작업은 **08:00 조사·09:00 보고, Asia/Seoul** 기준으로 안내합니다. 이는 사용자가 전달한 예약 확인 내용을 반영한 것이며, 이번 작업에서 dots의 예약이나 다음 실행을 직접 조회하지 않았습니다.

- 기존 예약이 있으면 해당 예약의 지침만 갱신하고 중복 예약을 만들지 않습니다.
- 08시에는 조사·초안 준비, 09시에는 최신 상태 재확인 후 발행·알림 확인을 수행합니다.
- 예약 저장과 다음 실행 시각은 실제 도구가 반환한 값으로만 확인합니다. `null`이면 미확인입니다.
- 업로드 세션은 만료될 수 있으므로 만료 시 클라우드의 비공개 로그인으로 다시 연결합니다.
- 이 로컬 보관본을 수정해도 운영 사이트의 지침은 자동으로 바뀌지 않습니다. 운영 사이트는 현재 배포된 `code/docs/`에 해당하는 파일을 읽습니다.

## 보관한 이전 구현

`funding/server.mjs`, `funding/providers.mjs`, `funding/platform.mjs`, `funding/portal/server.mjs`는 초기 상시 서버 시제품입니다. 해당 README의 FactChat 대체 조사·메일 실행 승인·SQLite·CMS 설명은 현재 Vercel 보고서 기능과 구분합니다.

현재 흐름에서는 dots가 조사·HTML 작성, Vercel이 저장·권한·상태, Dishost가 동아리 알림 전송을 맡습니다. FactChat 자동 대체와 Codex CLI 조사는 시제품 코드에 보관되어 있으며 현재 업로드 경로에 연결되어 있지 않습니다.
