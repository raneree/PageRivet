# PageRivet 2.3.3

PageRivet은 정적 웹 프로젝트를 만들고 편집하며 AI 클라이언트와 안전하게 협업할 수 있는 Windows 데스크톱 프로그램입니다.

> 현재 배포 버전: **2.3.3 Portable**
>
> 배포 형태: **Windows x64 포터블 폴더**

공식 홈페이지: <https://pagerivet.github.io/>

## 주요 기능

- 다중 HTML·CSS·JavaScript 파일 관리
- YAML·Markdown·SCSS·MJS·TypeScript 소스 관리
- 이미지·폰트·동영상·오디오·문서·데이터·웹 자산·3D 모델 관리
- 구문 강조, 코드 검색 및 줄 이동
- WebView2 기반 정적 프로젝트 미리보기
- HTML·CSS·JavaScript 검증과 모든 텍스트 소스의 UTF-8 검증
- 프로젝트 History, Undo/Redo 및 선택 기록 복원
- 비정상 종료 시 미적용 편집 내용 Recovery
- 프로젝트 폴더·ZIP·코드 복사 내보내기
- 외부 파일 변경 감지
- 한국어·영어 UI
- 로컬 MCP 서버와 56개 AI 협업 Tool
- Claude Desktop용 동봉 Bridge

## 다운로드 및 실행

1. 공식 Releases에서 `PageRivet-2.3.3-Windows-x64-Portable.zip`을 다운로드합니다.
2. ZIP 파일 전체를 원하는 폴더에 풉니다.
3. 최상위 폴더의 `PageRivetLauncher.exe`를 실행합니다.

```text
PageRivet-2.3.3-Windows-x64-Portable/
├─ PageRivetLauncher.exe
├─ PageRivetLauncher.pdb
└─ App/
   ├─ PageRivet.exe
   ├─ PageRivet.StdioBridge.exe
   ├─ portable.flag
   └─ ...
```

별도의 .NET 설치는 필요하지 않습니다. 프로젝트 미리보기에는 Microsoft Edge WebView2 Runtime이 필요합니다.

## 시스템 요구 사항

- 64비트 Windows 10 버전 1809 이상 또는 Windows 11
- [Microsoft Edge WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/)

## 프로젝트 소스

필수 기본 파일은 프로젝트 루트에 유지됩니다.

```text
project/
├─ index.html
├─ style.css
└─ script.js
```

추가 소스는 안전한 프로젝트 상대 경로에서 관리할 수 있습니다.

```text
project/
├─ config/site.yml
├─ docs/readme.md
├─ styles/theme.scss
├─ scripts/module.mjs
└─ src/main.ts
```

절대 경로, 상위 디렉터리 이동, Windows 예약 이름, `.editor` 내부 접근 및 재분석 지점을 이용한 프로젝트 루트 우회는 허용하지 않습니다. `assets/documents`의 Markdown은 관리 리소스로 유지됩니다.

## MCP Tool

추가 텍스트 소스는 다음 Tool로 관리할 수 있습니다.

| Tool | 설명 |
|---|---|
| `read_text_source` | YAML·Markdown·SCSS·MJS·TypeScript 적용 소스를 읽습니다. |
| `create_text_source` | 지원 확장자의 프로젝트 소스를 생성합니다. |
| `apply_text_source_patch` | 기존 추가 소스에 최소 텍스트 패치를 제안합니다. |
| `rename_text_source` | 추가 소스의 프로젝트 상대 경로를 변경합니다. |
| `delete_text_source` | 추가 소스를 삭제합니다. |

PageRivet 본체와 Preview에는 다음 Tool을 사용할 수 있습니다.

| Tool | 설명 |
|---|---|
| `get_ui_state` | 현재 언어, 테마와 주요 UI 상태를 조회합니다. |
| `get_preview_state` | Preview 초기화 여부와 현재 페이지를 조회합니다. |
| `get_recovery_status` | Recovery 내용을 노출하지 않고 복구 가능 상태를 조회합니다. |
| `get_mcp_status` | 인증값을 제외한 MCP 서버와 연결 상태를 조회합니다. |
| `get_mcp_log` | 민감정보가 제거된 최근 MCP 활동 로그를 조회합니다. |
| `get_export_presets` | 저장된 내보내기 프리셋을 조회합니다. |
| `save_project` | 현재 프로젝트의 적용된 상태를 저장합니다. |
| `refresh_preview` | 현재 Preview를 새로고침합니다. |
| `preview_html_page` | 기존 HTML 페이지를 Preview에 표시합니다. |
| `export_project` | 기존 프리셋 또는 일회성 설정으로 프로젝트를 내보냅니다. |

MCP 작업은 현재 활성 프로젝트만 대상으로 하며 Revision 검사, 검증, History 및 원자적 저장 경계를 따릅니다.

## 포터블 데이터와 보안

- 일반 설정과 내보내기 프리셋: `App\Data`
- MCP 설정과 인증: `%LocalAppData%\PageRivet\Mcp`
- WebView2 사용자 데이터: `%LocalAppData%\PageRivet\WebView2`
- Recovery 세션: `%LocalAppData%\PageRivet\Recovery\Sessions`

MCP Access Token과 Recovery 데이터는 공식 포터블 폴더에 포함되지 않습니다. MCP 서버는 로컬 루프백 주소만 사용합니다.

## 현재 지원 범위

PageRivet은 정적 프런트엔드 프로젝트를 대상으로 합니다. 백엔드 서버 실행과 React·Vue·Angular 등의 빌드 파이프라인 관리는 지원하지 않습니다. SCSS와 TypeScript의 컴파일도 현재 범위 밖입니다.

---

## English

PageRivet 2.3.3 is a portable Windows desktop editor for static web projects. It supports project-relative HTML, CSS, JavaScript, YAML, Markdown, SCSS, MJS, and TypeScript source files through the same validation, history, recovery, export, and MCP safety boundaries.

### Main features

- Manage multiple HTML, CSS, and JavaScript files
- Manage YAML, Markdown, SCSS, MJS, and TypeScript sources
- Manage images, fonts, video, audio, documents, data, web assets, and 3D models
- Syntax highlighting, code search, and line navigation
- Static project preview powered by WebView2
- HTML, CSS, JavaScript, and UTF-8 text-source validation
- Project History, Undo/Redo, Recovery, file watching, and export
- Korean and English UI
- A local MCP server with 56 AI collaboration tools
- A bundled Bridge for Claude Desktop

### Quick start

1. Download `PageRivet-2.3.3-Windows-x64-Portable.zip` from the official Releases page.
2. Extract the complete archive.
3. Run `PageRivetLauncher.exe` from the top-level directory.

The .NET runtime is included. Microsoft Edge WebView2 Runtime is required for preview. MCP credentials and recovery data are stored under the Windows user profile and are not included in the portable folder.

PageRivet targets static front-end projects. It preserves SCSS and TypeScript sources but does not compile them, and it does not run backend servers or framework build pipelines.
