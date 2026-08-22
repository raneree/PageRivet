# PageRivet

PageRivet은 여러 HTML, CSS, JavaScript 파일로 구성된 정적 웹 프로젝트를 만들고 편집하며, 결과를 바로 미리 볼 수 있는 Windows 데스크톱 프로그램입니다.

내장 MCP 서버를 통해 Codex 등의 외부 AI 클라이언트가 현재 프로젝트를 읽고 분석하거나, 사용자의 승인을 거쳐 코드를 수정할 수 있습니다.

> 현재 배포 버전: **1.1.0 Portable**  
> 배포 형태: **Windows x64 포터블 버전**

## 주요 기능

- 다중 HTML 페이지 생성, 편집, 이름 변경 및 삭제
- 다중 CSS·JavaScript 파일 관리
- 구문 강조, 코드 검색 및 줄 이동
- Microsoft Edge WebView2 기반 실시간 미리보기
- HTML·CSS·JavaScript 검증과 오류 위치 표시
- 브라우저 콘솔 및 JavaScript 예외 수집
- 프로젝트 히스토리, Undo/Redo 및 선택한 기록 복원
- 프로젝트 폴더·ZIP·코드 복사 내보내기
- 외부 파일 변경 감지
- 한국어·영어 UI와 내장 가이드
- 로컬 MCP 서버와 AI 읽기·쓰기·디버그 도구

## 다운로드 및 실행

1. [Releases](https://github.com/raneree/PageRivet/releases)에서 최신 `PageRivet-1.1.0-Portable.zip`을 다운로드합니다.
2. ZIP 파일의 압축을 원하는 폴더에 완전히 풉니다.
3. 최상위 폴더의 `PageRivetLauncher.exe`를 실행합니다.

ZIP 파일 안에서 직접 실행하지 마세요. 다음 폴더 구조를 유지해야 합니다.

```text
PageRivet-1.1.0-Portable/
├─ PageRivetLauncher.exe
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

PageRivet을 제거하려면 압축을 풀었던 폴더를 삭제하면 됩니다. MCP 연결 정보와 WebView2 사용자 데이터까지 모두 제거하려면 `%LocalAppData%\PageRivet` 폴더도 별도로 정리할 수 있습니다.

## 현재 지원 범위

PageRivet은 정적 HTML·CSS·JavaScript 프로젝트를 대상으로 합니다. PHP, Node.js, ASP.NET과 같은 백엔드 서버 실행이나 React·Vue·Angular 빌드 파이프라인 관리는 지원하지 않습니다.

HTML·CSS·JavaScript 소스 파일은 현재 프로젝트 루트에 배치되며, 이미지·폰트·동영상 등의 일반 자산을 관리하는 전용 UI는 제공하지 않습니다.

## 파일 검증

현재 실행 파일에는 디지털 코드 서명이 적용되어 있지 않습니다. 공식 GitHub Releases에서만 파일을 다운로드하고, 릴리스에 함께 안내된 SHA-256 값과 비교하세요.

---

## English

PageRivet is a portable Windows desktop editor for creating, editing, validating, previewing, and exporting static websites made of multiple HTML, CSS, and JavaScript files.

It also runs a local MCP server, allowing supported external AI clients to inspect the active project and propose or apply changes through PageRivet's validation, approval, and history workflow.

### Quick start

1. Download `PageRivet-1.1.0-Portable.zip` from [Releases](https://github.com/raneree/PageRivet/releases).
2. Extract the entire archive.
3. Run `PageRivetLauncher.exe` from the top-level folder.

Do not run the application directly from inside the ZIP archive, and keep `PageRivetLauncher.exe` and the `App` folder together.

### Requirements

- 64-bit Windows 10 version 1809 or later, or Windows 11
- [Microsoft Edge WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/)

The required .NET runtime is included in the portable package. General portable settings are stored in `App/Data`, while MCP credentials are generated on first run and stored separately at `%LocalAppData%\PageRivet\Mcp`. MCP credentials are not included in the release archive or the portable application folder.

PageRivet currently targets static front-end projects. Backend servers and framework build pipelines such as React, Vue, and Angular are outside the current scope.
