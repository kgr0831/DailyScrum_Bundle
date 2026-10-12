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

2026-10-07 추가: `code/funding/vercel/notification-target.mjs`는 설정된 알림 대상 ID와 선택적인 Discord 이름 조회를 담당합니다. `code/funding/vercel/tests/notification-target.test.mjs`에서 권한·읽기 전용 동작·실패 처리를 검증합니다.

2026-10-12 추가:

- `code/funding/vercel/s3-files.mjs`, `store-factory.mjs`: 비공개 S3와 기존 Blob 저장소 선택.
- `code/funding/vercel/migration.mjs`, `retention.mjs`, `maintenance.mjs`: 검증 후 데이터 복사, 7일 보관·삭제 재시도, 인증된 유지보수.
- `code/src/app/api/maintenance/`: 이전 POST API와 일일 정리 GET API.
- `code/scripts/bootstrap-reports-aws.py`, `code/infra/aws-bootstrap-permissions.json`: 지정 AWS 계정의 전용 버킷·OIDC 역할 구성과 권한.
- `code/docs/aws-reports-storage.md`: 실제 이전 순서, 아직 해결되지 않은 권한·원본 접근 문제.

전체 웹 AWS 이전 추가:

- `code/infra/aws-web.json`, `aws-web-bootstrap.sh`: 전용 EC2·IAM·CloudFront VPC origin과 초기 서비스 준비.
- `code/infra/aws-web-deploy-permissions.json`: 고객 관리형 IAM 정책으로 추가할 웹 배포 권한.
- `code/scripts/deploy-aws-web.py`, `aws-install-release.sh`, `aws-configure-runtime.py`: 계정·대상 확인, 소스 배포, Linux 빌드, 암호화 설정과 systemd.
- `code/scripts/run-report-retention.mjs`: AWS의 7일 HTML 정리 타이머 진입점.
- `code/scripts/test-aws-standalone.mjs`, `funding/vercel/tests/proxy-origin.test.mjs`: 자체 호스팅 로그인·프록시 신뢰 경계 검증.
- `code/docs/aws-web-migration.md`: 실제 AWS 주소·구성·검증·운영과 dots 전환 절차.
- `code/docs/dots-aws-switch.md`: 기존 두 dots 예약의 주소만 바꾸는 복사용 전달문.
- `code/.gitattributes`: AWS Linux 실행 스크립트의 LF 줄바꿈 유지.
- `code/vercel.json`: 이전 도메인의 AWS redirect. 기존 Vercel cron은 제거.

도트 랜딩 통합:

- `code/src/app/route.ts`, `code/src/landing/index.html`: `/`에서 완성 HTML 제공. 이전 홈 `page.tsx`를 교체했다.
- `code/public/landing/`: 선택한 도트 시안의 스타일·영상·작품·canvas와 세션 기반 보고서 메뉴.
- `code/scripts/test-landing-reel.mjs`: 새 MP4 플레이어의 반응형 재생·제작자 표시·정지·대체 화면 검증.
- `code/docs/landing-redesign.md`: 시안 출처·운영 통합·검증 명령.
- `code/public/landing/chrome.css`: 메인과 내부 페이지가 함께 쓰는 반투명 메뉴·푸터·색상.
- `code/src/app/interior.css`, `code/src/components/SiteHeader.tsx`, `SiteFooter.tsx`: 게임 목록·상세·소개·활동·기록의 디자인 통일과 내부 페이지 보고서 메뉴.
- `code/src/components/PixelScene.tsx`, `code/public/landing/illustrations.js`: 메인과 내부 페이지에서 함께 사용하는 도트·모션 장면. 화면 밖·메뉴 뒤·숨긴 탭에서 정지하며 페이지 전환 때 이전 장면을 해제한다.
- `code/scripts/test-pixel-scenes.mjs`: 실제 픽셀 재생·정지, 역할 탭, 페이지 이동·중복 방지, 동작 줄이기·데이터 절약과 반응형 화면 검증.
