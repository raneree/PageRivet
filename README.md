# PageRivet 2.3.1

PageRivet은 정적 웹 프로젝트를 만들고 편집하며, 사용자의 승인을 거쳐 AI 클라이언트와 안전하게 협업할 수 있는 Windows 데스크톱 프로그램입니다.

> 현재 배포 버전: **2.3.1 Portable**
>
> 배포 형태: **Windows x64 포터블 폴더**

공식 홈페이지: <https://pagerivet.github.io/>

## 2.3.1 주요 변경

- YAML `.yml`, `.yaml` 소스 관리
- Markdown `.md`, `.markdown` 소스 관리
- SCSS `.scss` 소스 관리
- ES Module `.mjs` 소스 관리
- TypeScript `.ts` 소스 관리
- 확장자별 구문 강조와 전용 텍스트 소스 탭
- 추가 소스의 저장, 외부 변경 감지, History, Recovery 및 내보내기 지원
- 추가 소스용 MCP 읽기·생성·패치·이름 변경·삭제 Tool
- 내장 MCP Tool 47개

SCSS 및 TypeScript 파일을 편집하고 보존할 수 있지만 PageRivet이 Sass 또는 TypeScript 컴파일러를 실행하지는 않습니다. Markdown 렌더링과 YAML 변환도 이번 버전에는 포함되지 않습니다.

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
- 로컬 MCP 서버와 승인 기반 AI 쓰기
- Claude Desktop용 동봉 Bridge

## 다운로드 및 실행

1. 공식 Releases에서 `PageRivet-2.3.1-Windows-x64-Portable.zip`을 다운로드합니다.
2. ZIP 파일 전체를 원하는 폴더에 풉니다.
3. 최상위 폴더의 `PageRivetLauncher.exe`를 실행합니다.

```text
PageRivet-2.3.1-Windows-x64-Portable/
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

## MCP 추가 Tool

2.3.1에서 다음 Tool이 추가되었습니다.

| Tool | 설명 |
|---|---|
| `read_text_source` | YAML·Markdown·SCSS·MJS·TypeScript 적용 소스를 읽습니다. |
| `create_text_source` | 지원 확장자의 프로젝트 소스를 생성합니다. |
| `apply_text_source_patch` | 기존 추가 소스에 최소 텍스트 패치를 제안합니다. |
| `rename_text_source` | 추가 소스의 프로젝트 상대 경로를 변경합니다. |
| `delete_text_source` | 추가 소스를 삭제합니다. |

MCP 쓰기는 현재 활성 프로젝트만 대상으로 하며 Revision 검사, 사용자 승인, 전체 검증, History 및 원자적 저장 정책을 따릅니다.

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

PageRivet 2.3.1 is a portable Windows desktop editor for static web projects. It supports project-relative HTML, CSS, JavaScript, YAML, Markdown, SCSS, MJS, and TypeScript source files through the same validation, history, recovery, export, and MCP approval boundaries.

### What's new in 2.3.1

- YAML: `.yml`, `.yaml`
- Markdown: `.md`, `.markdown`
- SCSS: `.scss`
- ES modules: `.mjs`
- TypeScript: `.ts`
- A dedicated text-source editor tab with syntax highlighting
- Source lifecycle support across save, Save As, file watching, history, recovery, and export
- Five new MCP tools for reading and managing these sources

PageRivet does not compile SCSS or TypeScript and does not render Markdown in this release.

### Quick start

1. Download `PageRivet-2.3.1-Windows-x64-Portable.zip` from the official Releases page.
2. Extract the complete archive.
3. Run `PageRivetLauncher.exe` from the top-level directory.

The .NET runtime is included. Microsoft Edge WebView2 Runtime is required for preview. MCP credentials and recovery data are stored under the Windows user profile and are not included in the portable folder.
