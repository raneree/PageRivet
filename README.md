# PageRivet 2.3.0

PageRivet은 여러 HTML, CSS, JavaScript 파일과 웹 리소스로 구성된 정적 웹 프로젝트를 만들고 편집하며 결과를 바로 미리 볼 수 있는 Windows 데스크톱 프로그램입니다.

내장 MCP 서버를 통해 Codex, Claude Desktop 등의 외부 AI 클라이언트가 현재 프로젝트를 읽고 분석하거나 사용자의 승인을 거쳐 코드를 수정할 수 있습니다.

> 현재 배포 버전: **2.3.0 Portable**
>
> 배포 형태: **Windows x64 포터블 버전**

공식 홈페이지: <https://pagerivet.github.io/>

## 2.3.0 주요 변경

- Claude Desktop 자동 연결 지원
- Claude Desktop용 고정 stdio Bridge 포함
- PageRivet 종료 후 재실행 시 Bridge 자동 재연결
- Claude Desktop structured content 응답 호환성 수정
- Claude Desktop 단일 연결 제한 안내창 추가
- 메인 창 초기 크기 약 20% 축소

Claude Desktop은 연결 방식 제한으로 한 번에 하나의 PageRivet만 연결할 수 있습니다. 이 제한은 Claude Desktop에만 적용되며 Codex 등 기존 Streamable HTTP 클라이언트에서는 여러 PageRivet 연결을 계속 사용할 수 있습니다.

## 주요 기능

- 다중 HTML 페이지 생성, 편집, 이름 변경 및 삭제
- 다중 CSS·JavaScript 파일과 하위 디렉터리 소스 관리
- 이미지·폰트·동영상·오디오·문서·데이터·웹 자산·3D 모델 관리
- 구문 강조, 코드 검색 및 줄 이동
- Microsoft Edge WebView2 기반 실시간 미리보기
- HTML·CSS·JavaScript 검증과 오류 위치 표시
- 브라우저 콘솔 및 JavaScript 예외 수집
- 프로젝트 히스토리, Undo/Redo 및 선택한 기록 복원
- 비정상 종료 시 미적용 편집 내용 복구
- 프로젝트 폴더·ZIP·코드 복사 내보내기
- 외부 파일 변경 감지
- 한국어·영어 UI와 내장 가이드
- 공식 홈페이지 기반 시작 화면과 오프라인 대체 안내
- 다중 실행을 지원하는 로컬 MCP 서버와 42개 AI 협업 Tool

## 다운로드 및 실행

1. [Releases](https://github.com/raneree/PageRivet/releases)에서 `PageRivet-2.3.0-Windows-x64-Portable.zip`을 다운로드합니다.
2. ZIP 파일의 압축을 원하는 폴더에 완전히 풉니다.
3. 최상위 폴더의 `PageRivetLauncher.exe`를 실행합니다.

ZIP 파일 안에서 직접 실행하지 마세요. 다음 폴더 구조를 유지해야 합니다.

```text
PageRivet-2.3.0-Windows-x64-Portable/
├─ PageRivetLauncher.exe
├─ PageRivetLauncher.pdb
└─ App/
   ├─ PageRivet.exe
   ├─ PageRivet.StdioBridge.exe
   ├─ portable.flag
   └─ ...
```

별도의 .NET 설치는 필요하지 않습니다. PageRivet 실행에 필요한 .NET 런타임은 포터블 패키지에 포함되어 있습니다.

## 시스템 요구 사항

- 64비트 Windows 10 버전 1809 이상 또는 Windows 11
- [Microsoft Edge WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/)

대부분의 최신 Windows 환경에는 WebView2 Runtime이 이미 설치되어 있습니다. 미리보기가 초기화되지 않는 경우 Microsoft 공식 페이지에서 Runtime 설치 여부를 확인하세요.

## 프로젝트 구성

필수 기본 파일은 프로젝트 루트에 유지됩니다.

```text
project/
├─ index.html
├─ style.css
└─ script.js
```

추가 HTML, CSS 및 JavaScript 파일은 안전한 프로젝트 하위 디렉터리에서 관리할 수 있습니다.

```text
project/
├─ index.html
├─ pages/
│  └─ about.html
├─ style.css
├─ css/
│  └─ components.css
├─ script.js
├─ js/
│  └─ navigation.js
└─ assets/
   ├─ images/
   ├─ fonts/
   ├─ videos/
   ├─ audio/
   ├─ documents/
   ├─ data/
   ├─ web/
   └─ models/
```

절대 경로, 상위 디렉터리 이동, Windows 예약 이름, `.editor` 내부 접근 및 재분석 지점을 이용한 프로젝트 루트 우회는 허용하지 않습니다.

## MCP 연결

PageRivet은 프로그램 내부에서 로컬 전용 MCP 서버를 실행합니다. 서버는 외부 네트워크 인터페이스가 아닌 `127.0.0.1` 루프백 주소에만 바인딩하고 Bearer Access Token으로 요청을 인증합니다.

MCP 포트, Access Token과 승인 정책은 실행 폴더가 아닌 다음 사용자 전용 위치에 저장됩니다.

```text
%LocalAppData%\PageRivet\Mcp\mcp-server.json
```

따라서 포터블 폴더나 공식 배포 ZIP에는 사용자의 MCP Access Token이 포함되지 않습니다.

PageRivet을 여러 개 실행하면 첫 번째 프로그램은 설정 포트(기본값 `51247`)와 `pagerivet` 연결 이름을 사용합니다. 포트가 이미 사용 중이면 다음 프로그램이 `51248` / `pagerivet-2`, `51249` / `pagerivet-3`처럼 순차 포트와 고유 연결 이름을 선택합니다.

자동 연결 기능이 구현된 클라이언트:

- Codex
- Claude Code
- Cursor
- VS Code (GitHub Copilot)
- Windsurf
- Cline
- GitHub Copilot CLI
- Gemini CLI
- Antigravity
- OpenClaw
- Claude Desktop

Generic MCP Client에는 수동 연결 정보를 제공합니다.

### Claude Desktop 연결

Claude Desktop은 PageRivet의 MCP 설정 화면에서 자동으로 구성할 수 있습니다. 자동 설정은 기존 Claude Desktop 환경설정과 다른 MCP 서버를 보존하고 `pagerivet` Bridge 항목 하나만 관리합니다.

최초 연결 또는 PageRivet 설치 경로 변경 후에는 Claude Desktop을 완전히 종료한 뒤 다시 실행해야 합니다. Windows에서는 창만 닫으면 Claude Desktop이 시스템 트레이에 남을 수 있으므로 트레이 아이콘에서도 종료됐는지 확인하세요.

Claude Desktop이 Bridge의 Tool 목록을 한 번 불러온 이후에는 다음 흐름으로 PageRivet만 다시 실행할 수 있습니다.

```text
PageRivet 종료
    ↓
Claude Desktop과 Bridge 연결 유지
    ↓
PageRivet 재실행
    ↓
Bridge 자동 재연결
```

제한 사항:

- Claude Desktop은 한 번에 하나의 PageRivet만 연결할 수 있습니다.
- 여러 PageRivet이 실행 중이면 먼저 연결을 점유한 PageRivet이 Claude Desktop 대상이 됩니다.
- 다른 PageRivet과 연결하려면 현재 연결된 PageRivet을 종료한 뒤 사용할 PageRivet을 새로 실행하세요.
- PageRivet이 연결되지 않은 동안 Tool을 호출하면 연결되지 않았다는 오류가 반환됩니다.
- 중단된 Tool 호출은 다른 PageRivet에 자동으로 재전송되지 않습니다.
- 최초 Tool 목록을 확인하기 전에 PageRivet이 연결되지 않았다면 PageRivet 연결 후 Claude Desktop 재시작이 필요할 수 있습니다.

PageRivet 시작 시 Claude Desktop이 감지되면 다음 안내가 표시됩니다.

> Claude Desktop의 연결 방식 제한으로 한 번에 하나의 PageRivet만 연결할 수 있습니다.

### 연결 상태 확인

상단 도구 모음의 MCP 표시는 클라이언트 연결 설정을 기준으로 합니다.

- 녹색 `MCP: 연결됨`: 현재 서버 주소와 인증값이 일치하는 지원 클라이언트가 하나 이상 있음
- 적색 `MCP: 연결 끊김`: 연결 미설정, 재설정 필요, 상태 확인 오류 또는 서버 시작 실패

`재설정 필요`가 표시되면 **설정 → MCP 설정 → 재설정**을 실행한 뒤 해당 AI 클라이언트를 다시 시작하세요.

Claude Desktop 문제를 진단할 때는 다음 로그를 확인할 수 있습니다.

```text
%APPDATA%\Claude\logs\mcp.log
%APPDATA%\Claude\logs\mcp-server-pagerivet.log
```

## MCP 명령어

일반 사용자가 다음 Tool 이름을 직접 입력할 필요는 없습니다. AI 클라이언트에 원하는 작업을 자연어로 요청하면 AI가 필요한 PageRivet Tool을 선택합니다. 모든 코드 변경은 활성 프로젝트만 대상으로 하며 PageRivet의 검증, 승인 및 히스토리 정책을 따릅니다.

| 카테고리 | Tool | 설명 |
|---|---|---|
| 프로젝트 조회 | `get_application_info` | PageRivet의 버전, 실행 정보와 지원 기능을 확인합니다. |
| 프로젝트 조회 | `get_project_info` | 현재 열린 프로젝트의 이름, 경로, Revision과 편집 상태를 확인합니다. |
| 프로젝트 조회 | `get_project_files` | 프로젝트의 HTML·CSS·JavaScript 파일 목록과 크기 정보를 조회합니다. |
| 프로젝트 조회 | `get_project_resources` | 관리 리소스의 경로, 종류, 미디어 타입, 크기와 지원 확장자를 조회합니다. |
| 프로젝트 조회 | `get_export_preset` | 현재 프로젝트에 선택된 내보내기 프리셋과 제한 조건을 확인합니다. |
| 코드 읽기 | `read_html` | 기본 페이지 `index.html`의 적용된 코드를 읽습니다. |
| 코드 읽기 | `read_html_page` | 파일명을 지정해 원하는 HTML 페이지의 적용된 코드를 읽습니다. |
| 코드 읽기 | `read_css` | 기본 스타일 파일 `style.css`의 적용된 코드를 읽습니다. |
| 코드 읽기 | `read_css_file` | 파일명을 지정해 원하는 CSS 파일의 적용된 코드를 읽습니다. |
| 코드 읽기 | `read_javascript` | 기본 스크립트 파일 `script.js`의 적용된 코드를 읽습니다. |
| 코드 읽기 | `read_javascript_file` | 파일명을 지정해 원하는 JavaScript 파일의 적용된 코드를 읽습니다. |
| 오류 및 검증 | `get_debug_context` | 오류, 콘솔, 최근 변경과 정상 히스토리를 묶어 AI 디버깅 시작 정보를 제공합니다. |
| 오류 및 검증 | `get_errors` | 검증, 미리보기, 파일, 내보내기와 MCP 오류를 조회합니다. |
| 오류 및 검증 | `get_console_log` | 미리보기 브라우저와 에디터의 최신 콘솔 기록을 조회합니다. |
| 오류 및 검증 | `validate_project` | 파일을 변경하지 않고 프로젝트 전체를 검증합니다. |
| 오류 및 검증 | `validate_export` | 파일을 만들지 않고 현재 내보내기 프리셋 기준으로 검증합니다. |
| 히스토리 | `get_recent_changes` | 변경 주체와 검증 결과를 포함한 최근 변경을 조회합니다. |
| 히스토리 | `get_history` | 프로젝트 히스토리와 현재 Undo·Redo 위치를 확인합니다. |
| 히스토리 | `get_history_diff` | 두 히스토리 시점의 파일 변경 내용을 비교합니다. |
| 히스토리 | `restore_history` | 기존 기록을 보존하면서 선택한 상태를 새로운 변경으로 복원합니다. |
| 코드 수정 | `apply_html_patch` | `index.html`에 정확히 일치하는 최소 텍스트 변경을 제안합니다. |
| 코드 수정 | `apply_css_patch` | `style.css`에 정확히 일치하는 최소 텍스트 변경을 제안합니다. |
| 코드 수정 | `apply_javascript_patch` | `script.js`에 정확히 일치하는 최소 텍스트 변경을 제안합니다. |
| 코드 수정 | `apply_project_patch` | 여러 기존 소스 파일 변경을 하나의 작업으로 제안합니다. |
| HTML 관리 | `create_html_page` | 프로젝트 상대 경로에 HTML 페이지를 생성합니다. |
| HTML 관리 | `apply_html_page_patch` | 지정한 HTML 페이지를 수정합니다. |
| HTML 관리 | `rename_html_page` | HTML 페이지 이름을 변경합니다. `index.html`은 제외됩니다. |
| HTML 관리 | `delete_html_page` | HTML 페이지를 삭제합니다. `index.html`은 제외됩니다. |
| CSS 관리 | `create_css_file` | 프로젝트 상대 경로에 CSS 파일을 생성합니다. |
| CSS 관리 | `apply_css_file_patch` | 지정한 CSS 파일을 수정합니다. |
| CSS 관리 | `rename_css_file` | CSS 파일 이름을 변경합니다. `style.css`는 제외됩니다. |
| CSS 관리 | `delete_css_file` | CSS 파일을 삭제합니다. `style.css`는 제외됩니다. |
| JavaScript 관리 | `create_javascript_file` | 프로젝트 상대 경로에 JavaScript 파일을 생성합니다. |
| JavaScript 관리 | `apply_javascript_file_patch` | 지정한 JavaScript 파일을 수정합니다. |
| JavaScript 관리 | `rename_javascript_file` | JavaScript 파일 이름을 변경합니다. `script.js`는 제외됩니다. |
| JavaScript 관리 | `delete_javascript_file` | JavaScript 파일을 삭제합니다. `script.js`는 제외됩니다. |
| 리소스 관리 | `import_project_resource` | 사용자 승인 후 로컬 파일을 관리 리소스 폴더로 가져옵니다. |
| 리소스 관리 | `rename_project_resource` | 종류를 유지하면서 관리 리소스 이름을 변경합니다. |
| 리소스 관리 | `delete_project_resource` | 관리 리소스를 프로젝트에서 삭제합니다. |

## 쓰기 승인과 안전 경계

- MCP 쓰기 Tool은 현재 활성 프로젝트만 대상으로 합니다.
- 쓰기 적용 전에 프로젝트 Revision과 현재 상태를 다시 확인합니다.
- 리소스 가져오기는 승인 정책과 관계없이 외부 원본 경로를 표시하고 명시적인 사용자 승인을 요청합니다.
- 소스와 리소스 경로는 프로젝트 루트 밖으로 벗어날 수 없습니다.
- 쓰기 결과는 프로젝트 히스토리에 남아 Undo 또는 선택 시점 복원이 가능합니다.
- Bridge는 연결이 바뀐 뒤 실패한 쓰기 요청을 자동 재시도하지 않습니다.

## 포터블 데이터

언어, Undo 횟수 등의 일반 설정과 내보내기 프리셋은 실행 후 `App\Data`에 생성됩니다. PageRivet 폴더를 이동할 때는 최상위 폴더 전체를 함께 이동하세요.

사용자별 데이터 위치:

```text
일반 포터블 설정     App\Data
MCP 설정과 인증      %LocalAppData%\PageRivet\Mcp
WebView2 사용자 데이터 %LocalAppData%\PageRivet\WebView2
복구 세션            %LocalAppData%\PageRivet\Recovery\Sessions
```

PageRivet을 제거하려면 압축을 풀었던 포터블 폴더를 삭제하면 됩니다. MCP 연결 정보, WebView2 사용자 데이터와 복구 정보까지 모두 제거하려면 `%LocalAppData%\PageRivet` 폴더도 별도로 정리할 수 있습니다.

## 지원 리소스

| 폴더 | 지원 리소스 |
|---|---|
| `assets/images` | PNG, JPEG, GIF, WebP, AVIF, SVG, ICO, BMP |
| `assets/fonts` | WOFF2, WOFF, TTF, OTF |
| `assets/videos` | MP4, WebM, OGV |
| `assets/audio` | MP3, M4A, AAC, WAV, OGG, Opus, WebA, FLAC |
| `assets/documents` | PDF, TXT, Markdown |
| `assets/data` | JSON, XML, CSV, WebVTT |
| `assets/web` | Web App Manifest, WebAssembly, Source Map |
| `assets/models` | glTF, GLB |

가져온 리소스는 프로젝트 저장, 다른 이름으로 저장, 미리보기와 내보내기에 포함됩니다. 리소스 목록은 이름·확장자·종류·크기를 기준으로 오름차순 또는 내림차순 정렬할 수 있습니다. JSON, XML, CSV, WebVTT, TXT, Markdown, Web App Manifest, Source Map, glTF JSON과 SVG는 리소스 탭에서 텍스트 미리보기와 편집을 지원합니다. 같은 형식은 MCP의 범용 텍스트 리소스 읽기·생성·패치 Tool로도 수정할 수 있습니다.

## 현재 지원 범위

PageRivet은 정적 HTML·CSS·JavaScript 프로젝트를 대상으로 합니다. PHP, Node.js, ASP.NET 같은 백엔드 서버 실행이나 React, Vue, Angular 빌드 파이프라인 관리는 지원하지 않습니다.

## 배포 파일 검증

현재 실행 파일에는 디지털 코드 서명이 적용되어 있지 않습니다. 공식 GitHub Releases에서만 파일을 다운로드하고 릴리스에 안내된 SHA-256 값과 비교하세요.

PageRivet 2.3.0 Windows x64 포터블 ZIP:

```text
SHA-256: 22A32BEE5A4B8D4431E097010B4B17CFB60FD3D40C09AC6EB468DE68C6F70E40
```

---

## English

PageRivet 2.3.0 is a portable Windows desktop editor for creating, editing, validating, previewing, and exporting static websites made of multiple HTML, CSS, JavaScript, and managed resource files.

It runs a loopback-only MCP server so supported AI clients can inspect the active project and propose or apply changes through PageRivet's validation, approval, and history workflow.

### What's new in 2.3.0

- Automatic Claude Desktop configuration through the bundled `PageRivet.StdioBridge`
- Automatic bridge reconnection after PageRivet restarts
- Structured-content compatibility fix for nullable MCP response fields
- A themed Claude Desktop connection notice
- A 20% smaller initial main-window size

### Quick start

1. Download `PageRivet-2.3.0-Windows-x64-Portable.zip` from [Releases](https://github.com/raneree/PageRivet/releases).
2. Extract the entire archive.
3. Run `PageRivetLauncher.exe` from the top-level folder.

Do not run the application from inside the ZIP. Keep `PageRivetLauncher.exe` and the `App` directory together. The required .NET runtime is included. Microsoft Edge WebView2 Runtime is required for previewing projects.

### Claude Desktop

PageRivet can add one fixed `pagerivet` bridge entry while preserving unrelated Claude Desktop preferences and MCP servers. Fully quit and restart Claude Desktop after the initial configuration or after the PageRivet installation path changes.

Due to Claude Desktop's connection model, only one PageRivet can be connected to Claude Desktop at a time. Other supported Streamable HTTP clients can continue to connect to multiple running PageRivet processes.

After Claude Desktop has loaded the bridge and its tools once, the bridge remains available while PageRivet is closed and reconnects when a new PageRivet starts. Tool calls made while no PageRivet is connected return an explicit error and are not automatically retried against a replacement process.

Claude Desktop MCP logs are available under `%APPDATA%\Claude\logs`.

### Security and local data

- MCP endpoints bind only to the local loopback interface.
- MCP access tokens are stored under `%LocalAppData%\PageRivet\Mcp` and are not written to Claude Desktop configuration files.
- General portable settings are stored in `App\Data`.
- Recovery sessions are isolated under `%LocalAppData%\PageRivet\Recovery\Sessions`.
- MCP writes target only the active project and follow PageRivet's validation, approval, and history policies.

PageRivet targets static front-end projects. Backend servers and framework build pipelines such as React, Vue, and Angular are outside the current scope.
