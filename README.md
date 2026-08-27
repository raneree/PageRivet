# PageRivet 3.3.0-beta.1

PageRivet은 정적 웹 프로젝트를 만들고 편집하며 AI 클라이언트와 안전하게 협업할 수 있는 데스크톱 프로그램입니다.

> 현재 개발 버전: **3.3.0-beta.1**
>
> UI 엔진: **Avalonia 12.1**
>
> 검증 상태: **Windows x64 자동 검증 완료, macOS·Linux 실기 검증 필요**

공식 홈페이지: <https://pagerivet.github.io/>

## 주요 기능

- 다중 HTML·CSS·JavaScript 파일 관리
- YAML·Markdown·SCSS·MJS·TypeScript 소스 관리
- `*.page.yml`·`*.page.yaml` 기반 PageRivet 웹페이지 작성
- 하나의 NativeWebView에서 HTML·PageRivet YAML 페이지 미리보기
- YAML 페이지의 스타일·스크립트·버튼·링크·입력 요소 실행
- 이미지·폰트·동영상·오디오·문서·데이터·웹 자산·3D 모델 관리
- 구문 강조, 코드 검색 및 줄 이동
- HTML·CSS·JavaScript·텍스트 UTF-8·PageRivet YAML 스키마 검증
- 프로젝트 History, Undo/Redo 및 선택 기록 복원
- 비정상 종료 시 미적용 편집 내용 Recovery
- 프로젝트 폴더·ZIP·코드 복사 내보내기
- 외부 파일 변경 감지
- 한국어·영어 UI와 시스템·밝은·어두운 테마
- 로컬 MCP 서버와 56개 AI 협업 Tool
- 지원 플랫폼용 동봉 Stdio Bridge

## 다운로드 및 실행

### Windows

1. 공식 Releases에서 `PageRivet-3.3.0-beta.1-Windows-x64-Portable.zip`을 다운로드합니다.
2. ZIP 파일 전체를 원하는 폴더에 풉니다.
3. 최상위 폴더의 `PageRivetLauncher.exe`를 실행합니다.

```text
PageRivet-3.3.0-beta.1-Windows-x64-Portable/
├─ PageRivetLauncher.exe
└─ App/
   ├─ PageRivet.exe
   ├─ PageRivet.StdioBridge.exe
   ├─ portable.flag
   └─ ...
```

### macOS·Linux 베타

macOS는 `.app` 번들, Linux는 x64 포터블 폴더를 기준으로 준비되어 있습니다. 3.3.0-beta.1의 macOS·Linux 배포 후보는 해당 운영체제에서 NativeWebView, 실행 권한과 패키지 검증을 통과한 뒤 제공합니다.

자세한 내용은 [플랫폼 실행 안내](../PageRivet%203.3.0-beta.1%20플랫폼%20실행%20안내.md)를 참고하세요.

## 시스템 요구 사항

- Windows 10 버전 1809 이상 또는 Windows 11 x64
- Windows 미리보기: [Microsoft Edge WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/)
- macOS 12 이상 x64 또는 Apple Silicon
- Linux x64, GTK 3 및 WebKitGTK 4.1·libsoup 3 또는 지원되는 WPE WebKit

각 배포물은 .NET 런타임을 포함하는 자체 포함 방식으로 준비됩니다.

## 프로젝트 소스

필수 기본 파일은 프로젝트 루트에 유지됩니다.

```text
project/
├─ index.html
├─ style.css
└─ script.js
```

추가 소스와 PageRivet YAML 페이지는 안전한 프로젝트 상대 경로에서 관리합니다.

```text
project/
├─ pages/home.page.yml
├─ config/site.yml
├─ docs/readme.md
├─ styles/theme.scss
├─ scripts/module.mjs
└─ src/main.ts
```

일반 `.yml`·`.yaml`은 텍스트 소스로 유지됩니다. 파일명이 `*.page.yml` 또는 `*.page.yaml`이고 문서의 `schema`가 `pagerivet.page/v1`인 경우에만 웹페이지 대상으로 처리합니다.

절대 경로, 상위 디렉터리 이동, Windows 예약 이름, `.editor` 내부 접근 및 재분석 지점을 이용한 프로젝트 루트 우회는 허용하지 않습니다.

## MCP Tool

기존 56개 MCP Tool과 허용 범위·Revision·검증·History·원자 저장 정책을 그대로 사용합니다. PageRivet YAML 페이지는 기존 텍스트 소스 Tool로 읽고 생성·패치·이름 변경·삭제하며 적용 전에 전용 스키마 검증을 통과해야 합니다.

MCP 설정 화면에서는 현재 운영체제에서 지원되고 설치가 감지된 클라이언트를 설정하거나 PageRivet 연결만 제거할 수 있습니다. 다른 클라이언트 설정과 다른 MCP 서버 항목은 유지됩니다.

## 포터블 데이터와 보안

Windows 포터블 기준 데이터 위치는 다음과 같습니다.

- 일반 설정, 로그, 최근 프로젝트와 내보내기 프리셋: `App\Data`
- MCP 설정과 인증: `%LocalAppData%\PageRivet\Mcp`
- Avalonia WebView2 사용자 데이터: `%LocalAppData%\PageRivet\WebView2-Avalonia`
- Recovery 세션: `%LocalAppData%\PageRivet\Recovery\Sessions`

MCP Access Token, Recovery 데이터와 개발자 로컬 설정은 배포 폴더에 포함하지 않습니다. MCP 서버와 미리보기 서버는 로컬 루프백 주소만 사용합니다.

## 현재 지원 범위

PageRivet은 정적 프런트엔드 프로젝트를 대상으로 합니다. 백엔드 서버 실행과 React·Vue·Angular 등의 빌드 파이프라인 관리는 지원하지 않습니다. SCSS와 TypeScript를 편집·보존하지만 컴파일하지 않습니다.

`3.3.0-beta.1`은 Avalonia 이식 최초 베타입니다. Windows 자동 회귀는 통과했지만 macOS·Linux 실기 실행, 플랫폼 패키지 서명·공증, 실제 한글 IME 및 장시간 수동 검증은 정식 전환 전에 완료해야 합니다.

---

## English

PageRivet 3.3.0-beta.1 is the first Avalonia-based beta of the static web project editor. It preserves the existing project, validation, history, recovery, export, and MCP safety boundaries while adding executable PageRivet YAML pages.

### Main features

- Manage HTML, CSS, JavaScript, YAML, Markdown, SCSS, MJS, and TypeScript sources
- Preview HTML and `*.page.yml` / `*.page.yaml` pages in one NativeWebView
- Run project CSS, JavaScript, buttons, links, inputs, and declarative events from YAML pages
- Manage project resources, History, Recovery, file watching, and exports
- Use the local MCP server and its 56 collaboration tools under the same approval and path boundaries

### Quick start

For Windows, extract the complete `PageRivet-3.3.0-beta.1-Windows-x64-Portable.zip` and run `PageRivetLauncher.exe`. The package includes .NET; Microsoft Edge WebView2 Runtime is required.

macOS uses WKWebView and Linux requires WebKitGTK 4.1 with GTK 3 and libsoup 3, or a supported WPE WebKit runtime. Those platform packages remain beta candidates until native validation is complete.

Only files named `*.page.yml` or `*.page.yaml` with `schema: pagerivet.page/v1` are executable YAML pages. Ordinary YAML files remain text sources. PageRivet does not compile SCSS or TypeScript and does not run backend servers or framework build pipelines.
