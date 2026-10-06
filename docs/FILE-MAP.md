# 파일 찾기

실행 코드의 상대경로와 테스트 의존성을 유지하기 위해 원래 프로젝트를 `code/`에 통째로 보관했습니다. `code/` 안에서 npm 명령을 실행합니다.

| 위치 | 역할 |
| --- | --- |
| `code/src/app/` | 기존 랜딩·게임 아카이브와 Next.js 경로 |
| `code/src/app/reports/[[...path]]/route.ts` | 겜마루 보고서 진입점 |
| `code/src/app/personal/[[...path]]/route.ts` | 공모전·채용 보고서 진입점 |
| `code/funding/vercel/router.mjs`, `config.mjs` | 두 공간 라우팅·서버 환경변수 |
| `code/funding/vercel/handler.mjs`, `access.mjs` | 로그인·권한·업로드·페이지 응답 |
| `code/funding/vercel/storage.mjs` | Private Blob 저장·조건부 갱신 |
| `code/funding/vercel/opportunities.mjs`, `member-progress.mjs` | 공고 데이터·공유/계정별 진행 기록 |
| `code/funding/vercel/personal-account.mjs`, `personal-profile.mjs` | 소유자 연결·비공개 조사 조건 |
| `code/funding/vercel/report-versions.mjs` | 같은 날짜 수정본·이전 버전 보존 |
| `code/funding/vercel/notify.mjs`, `worker-jobs.mjs` | 직접 알림·워커 대기 작업 |
| `code/funding/vercel/views.mjs`, `progress-views.mjs`, `reader.mjs` | 보고서 화면·진행 버튼·읽기 화면 |
| `code/funding/portal/style.css`, `views.mjs` | 현재 보고서에서도 사용하는 공통 스타일·HTML 도우미 |
| `code/discord-notifier/` | Dishost에 올리는 독립 알림 워커 전체 |
| `code/funding/vercel/tests/` | 현재 보고서 기능의 자동·브라우저 테스트 |
| `code/Design.md` | 겜마루 보고서 디자인·데이터 형식 |
| `code/gammaruInfo.md` | 동아리 소개·외부 자원 조사 기준 |
| `code/docs/dots-daily-brief.md` | 사이트가 읽는 겜마루 운영 지침 원본 |
| `code/docs/dots-personal-brief.md` | 사이트가 읽는 공모전·채용 운영 지침 원본 |
| `code/docs/personal-Design.md` | 공모전·채용 보고서 디자인 |
| `code/docs/personal-brief-profile.md` | 공개 가능한 조사 조건 기본 틀; 실제 비공개 프로필은 미포함 |
| `code/public/`, `src/data/`, `scripts/` | 공개 자산·아카이브 자료·기존 관리 스크립트 |
| `code/funding/README.md`, `portal/README.md`, `docs/gammaru-platform-plan.md` | 이전 시제품·기획 기록 |

`prompts/`는 dots 대화에 붙이는 예약 연결용 문서입니다. 사이트에 제공되는 상세 지침은 위 `code/docs/` 파일이 기준입니다. 보고서 디자인 또는 업로드 데이터 구조를 바꿀 때는 상세 지침과 실행 코드를 함께 검토합니다.

`SOURCE-MANIFEST.json`은 복사한 원본 파일의 커밋·크기·SHA-256과 보관본에서 바뀐 설명 문서, 추가 파일을 기록합니다. 비밀값·실제 회원 데이터는 포함하지 않습니다.
