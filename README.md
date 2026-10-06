# 겜마루 · 공모전/채용 데일리 스크럼 보관본

기존 프로젝트의 코드, 공개 이미지·영상, dots 지침과 운영 문서를 모은 독립 보관 폴더입니다. 원본 프로젝트와 운영 사이트를 변경하지 않았습니다.

## 어디부터 보면 되나요?

| 할 일 | 위치 |
| --- | --- |
| 전체 웹사이트 개발·실행 | [code/](code/) — 이 폴더가 Next.js 프로젝트 루트 |
| Discord 알림 봇만 실행 | [code/discord-notifier/](code/discord-notifier/README.md) |
| dots에게 겜마루 작업 전달 | [prompts/01-gammaru.md](prompts/01-gammaru.md) |
| dots에게 공모전·채용 작업 전달 | [prompts/02-personal.md](prompts/02-personal.md) |
| 현재 운영 구조·설정 확인 | [docs/OPERATIONS.md](docs/OPERATIONS.md) |
| 코드와 디자인 파일 찾기 | [docs/FILE-MAP.md](docs/FILE-MAP.md) |
| 복사·검증 내역 | [docs/VALIDATION.md](docs/VALIDATION.md), [SOURCE-MANIFEST.json](SOURCE-MANIFEST.json) |

## 웹사이트 실행

Node.js 22.19 이상을 사용합니다. 이 폴더에서 터미널을 열고 실행하세요.

```powershell
cd code
npm ci
Copy-Item .env.example .env.local
# .env.local의 빈 항목을 개발용 값으로 입력한 뒤 실행
npm run dev
```

- 랜딩: `http://localhost:3000/`
- 겜마루 보고서: `http://localhost:3000/reports`
- 공모전·채용 보고서: `http://localhost:3000/personal`
- 관리·업로드·Discord 로그인·보고서 저장에는 환경변수와 개발용 Private Blob 연결이 필요합니다. 빈 예제 파일만으로 해당 기능이 연결되지는 않습니다.
- Discord OAuth에는 실행 주소에 맞는 `/reports/auth/callback` 등록이 필요합니다. 두 보고서 공간이 같은 콜백을 사용합니다.

비밀값 없이 코드 검증만 하려면 `code/`에서 다음을 실행합니다. 보고서 테스트는 가상 저장소와 가상 Discord 응답을 사용합니다.

```powershell
npm run reports:test
npm run reports:test:browser
npm run build
```

브라우저 테스트는 Playwright와 설치된 Microsoft Edge를 사용합니다. 빌드는 Google Fonts 다운로드를 위해 네트워크 연결이 필요할 수 있습니다.

## 포함 범위

- 기존 랜딩·게임 아카이브 및 배포용 이미지·영상
- 겜마루 보고서, 공모전·채용 보고서, 로그인·권한·상태 저장·재발행·알림 코드
- Dishost 알림 워커, 자동 테스트, 디자인·동아리 정보, dots 원문 지침
- 이전 Codex/팩트챗 수집 시제품과 기획 문서 — 현재 Vercel 보고서 흐름과 구분해 보관

실제 `.env`, 토큰·비밀번호, 로그인 세션, 비공개 프로필, 보고서·회원 데이터, `.vercel`, `.git`, `node_modules`, 빌드 산출물은 제외했습니다. 실행에 필요한 키는 각 서비스의 비공개 설정에서 별도로 입력해야 합니다.

## 운영과의 관계

기준 소스는 `f03127ce751c90907486ddd4133e6d93b05dabc4`입니다. 실행 코드·의존성 버전·원래 경로는 보존하고, 보관본 안내와 환경변수 예제 및 오래된 운영 설명만 정리했습니다.

이 폴더를 만든 것으로 배포나 dots 예약이 변경되지는 않습니다. 기존 예약은 사용자가 전달한 확인 내용상 **한국 시간 08시 조사·09시 보고**이며, 실제 다음 실행 시각은 dots에서 확인해야 합니다. 현재 사이트 주소에 로그인해 발행하면 운영 데이터에 반영되므로 보관본 테스트에는 개발용 저장소와 주소를 사용합니다.

향후 별도 Vercel 프로젝트에 연결할 경우 프로젝트 루트를 `code`로 지정합니다. `code/`만 새 저장소에 넣는 경우에는 루트를 그대로 사용합니다. 이번 정리에서는 새 배포·저장소 생성·운영 데이터 이관을 하지 않았습니다.
