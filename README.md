# PageRivet

PageRivet은 여러 HTML, CSS, JavaScript 파일로 구성된 정적 웹 프로젝트를 만들고 편집하며, 결과를 바로 미리 볼 수 있는 Windows 데스크톱 프로그램입니다.

내장 MCP 서버를 통해 Codex 등의 외부 AI 클라이언트가 현재 프로젝트를 읽고 분석하거나, 사용자의 승인을 거쳐 코드를 수정할 수 있습니다.

> 현재 배포 버전: **1.1.1 Portable**
>
> 배포 형태: **Windows x64 포터블 버전**

## 주요 기능

- 다중 HTML 페이지 생성, 편집, 이름 변경 및 삭제
- 다중 CSS·JavaScript 파일 관리
- 구문 강조, 코드 검색 및 줄 이동
- Microsoft Edge WebView2 기반 실시간 미리보기
- HTML·CSS·JavaScript 검증과 오류 위치 표시
- 브라우저 콘솔 및 JavaScript 예외 수집
- 프로젝트 히스토리, Undo/Redo 및 선택한 기록 복원
- 비정상 종료 시 미적용 편집 내용 복구
- 프로젝트 폴더·ZIP·코드 복사 내보내기
- 외부 파일 변경 감지
- 한국어·영어 UI와 내장 가이드
- 로컬 MCP 서버와 AI 읽기·쓰기·디버그 도구

## 1.1.1 안정화

1.1.1은 새로운 편집 기능이나 MCP 기능 범위를 확장하는 버전이 아니라, 1.1.0의 기존 기능을 더 안정적으로 유지하기 위한 하드닝 버전입니다.

- MainForm에 집중되어 있던 UI 작업 흐름을 Coordinator와 Panel/View로 분리
- 비정상 종료 시 `Apply` 전 편집 버퍼를 복구할 수 있는 Recovery 추가
- 오류·콘솔·History·Revision 정보를 이용한 AI 진단 흐름 보강
- MCP 진단 응답에서 토큰·API key·사용자 절대 경로 등 민감정보 제거
- 대량 파일, 5MB 소스, 500단계 History, 반복 Apply/Save/MCP/FileSystemWatcher에 대한 Stress·Soak 테스트 추가
- PDB의 파일·라인 기반 진단 정보는 유지하면서 실제 개발 PC 경로를 `/_/`로 정규화

최종 검증은 일반 Release 271개, Stress 3개, Soak 2개로 총 276개 항목이 모두 성공했으며 Release 빌드는 경고 0개, 오류 0개를 기준으로 합니다.

## 다운로드 및 실행

1. [Releases](https://github.com/raneree/PageRivet/releases)에서 최신 `PageRivet-1.1.1-Portable.zip`을 다운로드합니다.
2. ZIP 파일의 압축을 원하는 폴더에 완전히 풉니다.
3. 최상위 폴더의 `PageRivetLauncher.exe`를 실행합니다.

ZIP 파일 안에서 직접 실행하지 마세요. 다음 폴더 구조를 유지해야 합니다.

```text
PageRivet-1.1.1-Portable/
├─ PageRivetLauncher.exe
├─ PageRivetLauncher.pdb
└─ App/
   ├─ PageRivet.exe
   ├─ portable.flag
   └─ Data/
```

별도의 .NET 설치는 필요하지 않습니다. PageRivet 실행에 필요한 .NET 런타임은 포터블 패키지에 포함되어 있습니다.

## 시스템 요구 사항

- 64비트 Windows 10 버전 1809 이상 또는 Windows 11
- [Microsoft Edge WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/)

대부분의 최신 Windows 환경에는 WebView2 Runtime이 이미 설치되어 있습니다. 미리보기가 초기화되지 않는 경우 Microsoft 공식 페이지에서 Runtime 설치 여부를 확인하세요.

## MCP 연결

PageRivet은 프로그램 내부에서 로컬 전용 MCP 서버를 실행합니다. MCP 접근 토큰은 Windows 사용자 환경에서 최초 실행할 때 새로 생성됩니다.

MCP 포트, 접근 토큰과 승인 정책은 실행 폴더가 아닌 다음 사용자 전용 위치에 저장됩니다.

```text
%LocalAppData%\PageRivet\Mcp\mcp-server.json
```

따라서 사용 중인 PageRivet 폴더를 다른 사람에게 전달해도 MCP 접근 토큰은 프로그램 폴더에 포함되지 않습니다. 다만 배포할 때는 사용 중인 폴더보다 공식 Releases의 깨끗한 ZIP을 공유하는 것을 권장합니다.

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

Generic MCP Client에는 수동 연결 정보를 제공합니다. Claude Desktop은 전송 방식 차이로 인해 현재 자동 연결 대상에서 제외되어 있습니다.

MCP 클라이언트의 업데이트에 따라 설정 형식이나 연결 동작이 달라질 수 있습니다. 문제가 발생하면 PageRivet의 **설정 → MCP 설정**에서 현재 연결 상태를 확인하세요.

상단 도구 모음의 MCP 표시는 클라이언트 연결 설정을 기준으로 합니다.

- 녹색 `MCP: 연결됨`: 현재 서버 주소와 인증값이 일치하는 지원 클라이언트가 하나 이상 있음
- 적색 `MCP: 연결 끊김`: 연결 미설정, 재설정 필요, 상태 확인 오류 또는 서버 시작 실패

`재설정 필요`가 표시되면 **설정 → MCP 설정 → 재설정**을 실행한 뒤 AI 클라이언트를 다시 시작하세요.

## MCP 명령어

일반 사용자가 다음 명령어를 직접 입력할 필요는 없습니다. AI 클라이언트에 원하는 작업을 자연어로 요청하면 AI가 필요한 PageRivet 도구를 선택합니다. 모든 코드 변경은 활성 프로젝트만 대상으로 하며 PageRivet의 검증과 승인 정책을 따릅니다.

| 카테고리 | 명령어 | 설명 |
|---|---|---|
| 프로젝트 조회 | `get_application_info` | PageRivet의 버전, 실행 정보와 지원 기능을 확인합니다. |
| 프로젝트 조회 | `get_project_info` | 현재 열린 프로젝트의 이름, 경로, Revision과 편집 상태를 확인합니다. |
| 프로젝트 조회 | `get_project_files` | 프로젝트의 HTML·CSS·JavaScript 파일 목록과 크기 정보를 조회합니다. |
| 프로젝트 조회 | `get_export_preset` | 현재 프로젝트에 선택된 내보내기 프리셋과 제한 조건을 확인합니다. |
| 코드 읽기 | `read_html` | 기본 페이지인 `index.html`의 적용된 코드를 읽습니다. |
| 코드 읽기 | `read_html_page` | 파일명을 지정해 원하는 HTML 페이지의 적용된 코드를 읽습니다. |
| 코드 읽기 | `read_css` | 기본 스타일 파일인 `style.css`의 적용된 코드를 읽습니다. |
| 코드 읽기 | `read_css_file` | 파일명을 지정해 원하는 CSS 파일의 적용된 코드를 읽습니다. |
| 코드 읽기 | `read_javascript` | 기본 스크립트 파일인 `script.js`의 적용된 코드를 읽습니다. |
| 코드 읽기 | `read_javascript_file` | 파일명을 지정해 원하는 JavaScript 파일의 적용된 코드를 읽습니다. |
| 오류 및 검증 | `get_debug_context` | 오류, 콘솔, 최근 변경과 정상 히스토리를 묶어 AI 디버깅 시작 정보를 제공합니다. |
| 오류 및 검증 | `get_errors` | 검증, 미리보기, 파일, 내보내기와 MCP에서 기록된 오류를 조회합니다. |
| 오류 및 검증 | `get_console_log` | 미리보기 브라우저와 에디터의 최신 콘솔 기록을 조회합니다. |
| 오류 및 검증 | `validate_project` | 파일을 변경하지 않고 프로젝트의 HTML·CSS·JavaScript 전체를 검증합니다. |
| 오류 및 검증 | `validate_export` | 파일을 만들지 않고 현재 내보내기 프리셋 기준으로 프로젝트를 검증합니다. |
| 히스토리 | `get_recent_changes` | 변경 주체와 검증 결과를 포함한 최근 프로젝트 변경을 조회합니다. |
| 히스토리 | `get_history` | 프로젝트 히스토리와 현재 Undo·Redo 위치를 확인합니다. |
| 히스토리 | `get_history_diff` | 두 히스토리 시점의 파일 변경 내용을 줄 단위로 비교합니다. |
| 히스토리 | `restore_history` | 선택한 히스토리 상태를 기존 기록을 보존한 새로운 변경으로 복원합니다. |
| 코드 수정 | `apply_html_patch` | `index.html`에 정확히 일치하는 최소 텍스트 변경을 제안합니다. |
| 코드 수정 | `apply_css_patch` | `style.css`에 정확히 일치하는 최소 텍스트 변경을 제안합니다. |
| 코드 수정 | `apply_javascript_patch` | `script.js`에 정확히 일치하는 최소 텍스트 변경을 제안합니다. |
| 코드 수정 | `apply_project_patch` | 여러 기존 HTML·CSS·JavaScript 파일의 변경을 하나의 작업으로 제안합니다. |
| HTML 파일 관리 | `create_html_page` | 프로젝트 루트에 새로운 HTML 페이지를 생성합니다. |
| HTML 파일 관리 | `apply_html_page_patch` | 파일명을 지정해 기존 HTML 페이지의 코드를 수정합니다. |
| HTML 파일 관리 | `rename_html_page` | HTML 페이지의 이름을 변경합니다. `index.html`은 변경할 수 없습니다. |
| HTML 파일 관리 | `delete_html_page` | HTML 페이지를 삭제합니다. `index.html`은 삭제할 수 없습니다. |
| CSS 파일 관리 | `create_css_file` | 프로젝트 루트에 새로운 CSS 파일을 생성합니다. |
| CSS 파일 관리 | `apply_css_file_patch` | 파일명을 지정해 기존 CSS 파일의 코드를 수정합니다. |
| CSS 파일 관리 | `rename_css_file` | CSS 파일의 이름을 변경합니다. `style.css`는 변경할 수 없습니다. |
| CSS 파일 관리 | `delete_css_file` | CSS 파일을 삭제합니다. `style.css`는 삭제할 수 없습니다. |
| JavaScript 파일 관리 | `create_javascript_file` | 프로젝트 루트에 새로운 JavaScript 파일을 생성합니다. |
| JavaScript 파일 관리 | `apply_javascript_file_patch` | 파일명을 지정해 기존 JavaScript 파일의 코드를 수정합니다. |
| JavaScript 파일 관리 | `rename_javascript_file` | JavaScript 파일의 이름을 변경합니다. `script.js`는 변경할 수 없습니다. |
| JavaScript 파일 관리 | `delete_javascript_file` | JavaScript 파일을 삭제합니다. `script.js`는 삭제할 수 없습니다. |

## 포터블 데이터

언어·Undo 횟수 등의 일반 설정과 내보내기 프리셋은 실행 후 `App/Data/` 폴더에 생성됩니다. PageRivet 폴더를 이동할 때는 최상위 폴더 전체를 함께 이동하세요.

MCP 연결 정보는 포터블 폴더와 분리되어 다음 위치에 저장됩니다.

```text
%LocalAppData%\PageRivet\Mcp
```

WebView2 사용자 데이터는 Windows의 다음 위치에 별도로 생성될 수 있습니다.

```text
%LocalAppData%\PageRivet\WebView2
```

비정상 종료에 대비한 미적용 편집 버퍼 Recovery는 다음 위치에 저장됩니다.

```text
%LocalAppData%\PageRivet\Recovery
```

정상 종료, Apply, 편집 내용 폐기 또는 프로젝트 닫기 시 해당 Recovery 데이터는 정리됩니다.

PageRivet을 제거하려면 압축을 풀었던 폴더를 삭제하면 됩니다. MCP 연결 정보, WebView2 사용자 데이터와 Recovery 데이터까지 모두 제거하려면 `%LocalAppData%\PageRivet` 폴더도 별도로 정리할 수 있습니다.

## 현재 지원 범위

PageRivet은 정적 HTML·CSS·JavaScript 프로젝트를 대상으로 합니다. PHP, Node.js, ASP.NET과 같은 백엔드 서버 실행이나 React·Vue·Angular 빌드 파이프라인 관리는 지원하지 않습니다.

HTML·CSS·JavaScript 소스 파일은 현재 프로젝트 루트에 배치되며, 이미지·폰트·동영상 등의 일반 자산을 관리하는 전용 UI는 제공하지 않습니다.

## 파일 검증

현재 실행 파일에는 디지털 코드 서명이 적용되어 있지 않습니다. 공식 GitHub Releases에서만 파일을 다운로드하고, 릴리스에 함께 안내된 SHA-256 값과 비교하세요.

---

## English

PageRivet is a portable Windows desktop editor for creating, editing, validating, previewing, and exporting static websites made of multiple HTML, CSS, and JavaScript files.

It also runs a local MCP server, allowing supported external AI clients to inspect the active project and propose or apply changes through PageRivet's validation, approval, and history workflow.

### 1.1.1 stabilization

Version 1.1.1 focuses on hardening rather than expanding the editor or MCP feature scope. It includes UI responsibility separation, recovery of unapplied editor buffers after abnormal termination, richer diagnostics with sensitive-data redaction, stress/soak coverage, and normalized PDB source paths while preserving file/line diagnostics.

### Quick start

1. Download `PageRivet-1.1.1-Portable.zip` from [Releases](https://github.com/raneree/PageRivet/releases).
2. Extract the entire archive.
3. Run `PageRivetLauncher.exe` from the top-level folder.

Do not run the application directly from inside the ZIP archive, and keep `PageRivetLauncher.exe` and the `App` folder together.

### Requirements

- 64-bit Windows 10 version 1809 or later, or Windows 11
- [Microsoft Edge WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/)

The required .NET runtime is included in the portable package. General portable settings are stored in `App/Data`, while MCP credentials are generated on first run and stored separately at `%LocalAppData%\PageRivet\Mcp`. Recovery data for unapplied editor buffers is stored under `%LocalAppData%\PageRivet\Recovery` and is cleaned up on normal completion of the relevant session. MCP credentials are not included in the release archive or the portable application folder.

### MCP connection status

The MCP indicator in the top toolbar reflects the supported client configuration, not merely whether PageRivet's internal server process is running.

- Green `MCP: Connected`: at least one supported client matches the current server endpoint and credentials.
- Red `MCP: Disconnected`: the connection is missing, requires reconfiguration, could not be checked, or the server failed to start.

If reconfiguration is required, open **Settings → MCP Settings → Reconfigure**, then restart the AI client.

### MCP command reference

You do not need to type these tool names directly. Describe the task in natural language and the AI client will select the PageRivet tools it needs. Code-changing tools target only the active project and follow PageRivet's validation and approval policy.

| Category | Command | Description |
|---|---|---|
| Project overview | `get_application_info` | View PageRivet's version, runtime information, and supported capabilities. |
| Project overview | `get_project_info` | View the active project's name, path, revision, and editing state. |
| Project overview | `get_project_files` | List the project's HTML, CSS, and JavaScript files with size information. |
| Project overview | `get_export_preset` | View the selected export preset and its restrictions. |
| Read source code | `read_html` | Read the applied source of the default `index.html` page. |
| Read source code | `read_html_page` | Read the applied source of a named HTML page. |
| Read source code | `read_css` | Read the applied source of the default `style.css` file. |
| Read source code | `read_css_file` | Read the applied source of a named CSS file. |
| Read source code | `read_javascript` | Read the applied source of the default `script.js` file. |
| Read source code | `read_javascript_file` | Read the applied source of a named JavaScript file. |
| Diagnostics and validation | `get_debug_context` | Collect errors, console entries, recent changes, and the last valid history state for AI debugging. |
| Diagnostics and validation | `get_errors` | View recorded validation, preview, file, export, and MCP errors. |
| Diagnostics and validation | `get_console_log` | View the latest preview browser and editor console entries. |
| Diagnostics and validation | `validate_project` | Validate all applied HTML, CSS, and JavaScript without changing files. |
| Diagnostics and validation | `validate_export` | Validate the project against the selected export preset without creating output. |
| History | `get_recent_changes` | View recent changes, including their origin and validation result. |
| History | `get_history` | View project history and the current undo/redo position. |
| History | `get_history_diff` | Compare two history snapshots with a line-based diff. |
| History | `restore_history` | Restore a snapshot as a new change while preserving existing history. |
| Modify code | `apply_html_patch` | Propose exact minimal text replacements for `index.html`. |
| Modify code | `apply_css_patch` | Propose exact minimal text replacements for `style.css`. |
| Modify code | `apply_javascript_patch` | Propose exact minimal text replacements for `script.js`. |
| Modify code | `apply_project_patch` | Propose one atomic change across multiple existing HTML, CSS, and JavaScript files. |
| Manage HTML files | `create_html_page` | Create a new root-level HTML page. |
| Manage HTML files | `apply_html_page_patch` | Modify a named existing HTML page. |
| Manage HTML files | `rename_html_page` | Rename an HTML page. `index.html` cannot be renamed. |
| Manage HTML files | `delete_html_page` | Delete an HTML page. `index.html` cannot be deleted. |
| Manage CSS files | `create_css_file` | Create a new root-level CSS file. |
| Manage CSS files | `apply_css_file_patch` | Modify a named existing CSS file. |
| Manage CSS files | `rename_css_file` | Rename a CSS file. `style.css` cannot be renamed. |
| Manage CSS files | `delete_css_file` | Delete a CSS file. `style.css` cannot be deleted. |
| Manage JavaScript files | `create_javascript_file` | Create a new root-level JavaScript file. |
| Manage JavaScript files | `apply_javascript_file_patch` | Modify a named existing JavaScript file. |
| Manage JavaScript files | `rename_javascript_file` | Rename a JavaScript file. `script.js` cannot be renamed. |
| Manage JavaScript files | `delete_javascript_file` | Delete a JavaScript file. `script.js` cannot be deleted. |

PageRivet currently targets static front-end projects. Backend servers and framework build pipelines such as React, Vue, and Angular are outside the current scope.
