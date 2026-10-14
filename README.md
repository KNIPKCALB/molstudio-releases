# MolStudio 배포

MolStudio(분자·결정 구조 보기·편집, Gaussian/ORCA/VASP 계산 준비) 설치 파일을 받는 곳입니다. 소스 코드는 비공개이고, 이 저장소에는 설치 파일과 설치 스크립트만 올라갑니다.

**최신 버전: 2.2.0** — 화면을 더 또렷하게 바꿨습니다(진한 글씨, 영역·버튼 경계). 등록된 연구실 아이디로만 사용할 수 있습니다.

2.x는 앱 전체를 Rust로 새로 만든 버전입니다. 그림은 GPU(Mac은 Metal, Windows는 DirectX 12/Vulkan)로 그려서 원자가 수만~수십만 개여도 부드럽게 돌아가고, 설치 파일이 작고 빨리 뜹니다. 1.x(Python)의 메뉴와 기능을 그대로 옮겼고, ORCA 계산 번들은 1.x와 같은 파일을 만듭니다.

**필요 환경**
- Mac: Apple Silicon(M1 이후), macOS 11 Big Sur 이상. Intel Mac은 지원하지 않습니다.
- Windows: Windows 10 이상, 64비트(x64). DirectX 12 또는 Vulkan을 지원하는 그래픽 드라이버가 필요합니다.

## 설치

**Mac (Apple Silicon: M1 이후, macOS 11 이상)**: 터미널에 붙여 넣기
```bash
curl --proto '=https' --tlsv1.2 -fsSL -o molstudio-install.sh https://github.com/KNIPKCALB/molstudio-releases/releases/latest/download/install.sh && bash molstudio-install.sh
```

**Windows (10 이상, 64비트)**: PowerShell에 붙여 넣기 (시작 메뉴 → "PowerShell")
```powershell
irm https://github.com/KNIPKCALB/molstudio-releases/releases/latest/download/install.ps1 | iex
```

고를 것 없이 **명령을 붙여 넣은 컴퓨터에 맞는 버전**이 깔립니다. Mac 터미널이면 Mac 앱(`/Applications`), Windows PowerShell이면 Windows 설치 마법사입니다.

다른 컴퓨터용 파일만 받아 옮기려면:
- Mac에서 Windows 설치 파일 받기: 위 Mac 명령 끝에 ` --windows`를 붙입니다 (`~/Downloads`에 저장).
- Windows에서 Mac 디스크 이미지 받기: 먼저 `$env:MOLSTUDIO_TARGET = 'mac'`을 실행한 뒤 위 Windows 명령을 실행합니다 (`Downloads`에 저장).

처음 실행하면 **연구실 서버 아이디**를 묻습니다. **등록된 연구실 구성원의 아이디만** 받으며, 등록되지 않은 아이디로는 앱을 쓸 수 없습니다(2.1부터, 실행할 때마다 확인). ORCA 계산 번들의 서버 경로(`/home/<아이디>/…`)도 이 아이디로 만듭니다. 아이디가 거부되면 관리자에게 문의하세요.

## 업데이트 (이미 설치한 경우)
1. MolStudio를 종료합니다.
2. **처음 설치할 때와 같은 명령**을 다시 실행합니다. 명령은 항상 최신 버전(`latest`)을 받습니다.
   - Mac: 기존 `/Applications/MolStudio.app`을 새 버전으로 바꿉니다. 바꾸기 전에 확인을 묻고, 이전 버전은 휴지통으로 옮깁니다.
   - Windows: 새 설치 프로그램이 같은 위치에 덮어 설치합니다. 먼저 제거할 필요는 없습니다.
3. 연구실 아이디, 보기 설정, 최근 파일 같은 설정은 그대로 남습니다(앱 바깥에 저장돼 있습니다).
   - 1.x에서 2.x로 올릴 때는 연구실 아이디만 옮겨 옵니다. 보기 설정과 최근 파일은 새로 시작합니다.

1.0.0부터는 앱의 **Help ▸ Check for Updates…**로 새 버전이 있는지 확인할 수 있습니다. 새 버전이 있으면 위 명령을 보여 주고 복사 버튼을 줍니다. 앱이 직접 내려받거나 실행하지는 않습니다.

## 보안: 스크립트가 하는 일과 하지 않는 일

- **해시 고정**: 설치 스크립트는 릴리스를 게시할 때 만들어지고, 그 릴리스 설치 파일의 SHA-256 값이 스크립트 안에 들어 있습니다. 받은 파일의 해시가 다르면 실행하지 않고 지웁니다.
- **받는 곳**: 파일은 HTTPS(TLS 1.2 이상)로 이 저장소의 해당 버전 릴리스에서만 받습니다.
- **변경 불가 릴리스**: 이 저장소는 GitHub immutable releases를 켜 두어서, 게시된 릴리스의 파일과 태그는 바뀌지 않습니다. GitHub가 서명한 릴리스 증명(attestation)도 붙습니다.
- **권한**: `sudo`를 쓰지 않습니다. Mac은 `/Applications`(쓰기 권한이 없으면 `~/Applications`)에만 설치합니다. 기존 앱은 휴지통으로 옮긴 뒤 바꿉니다.
- **앱 검사**: Mac에서는 설치 전후에 앱 서명 봉인(`codesign --verify --deep --strict`)을 검사합니다. Apple Silicon이 아니면 설치를 멈춥니다.
- **잘린 스크립트**: Mac 명령은 스크립트를 파일로 끝까지 받은 다음(`&&`) 실행합니다. 스크립트 전체가 하나의 블록이라 일부만 받은 파일은 문법 오류가 나서 아무것도 실행되지 않습니다. Windows의 `irm`도 전체를 받은 다음 실행합니다.
- **설치 실패 시 복구**: Mac에서는 새 앱을 임시 이름으로 복사하고 검사한 뒤에 기존 앱과 바꿉니다. 바꾸다 실패하면 기존 앱을 되돌립니다. 기존 앱은 지우지 않고 휴지통으로 옮기며, 그게 안 되면 `MolStudio-previous-…`로 남겨 둡니다.
- **자동 검증**: 릴리스를 게시할 때마다 이 저장소의 GitHub Actions가 실제 Windows(PowerShell 5.1·7)와 macOS에서 다음을 확인합니다.
  - 설치 명령이 동작하는지
  - 설치한 앱이 실행되는지
  - **해시를 바꾼 스크립트가 설치를 거부하는지**

  결과는 [Actions](../../actions) 탭에 있습니다.

### 이 방식이 보장하는 것과 보장하지 않는 것

- **보장하는 것**: 설치 스크립트를 믿는다면, 스크립트가 받는 설치 파일은 그 릴리스에 게시된 원본입니다. 받는 도중 바뀌었거나 깨졌다면 설치하지 않습니다.
- **보장하지 않는 것**: 스크립트 자체는 검증 없이 실행됩니다. 이 GitHub 계정이 탈취되면 공격자가 새 `latest` 버전을 올릴 수 있습니다. 이미 게시된 버전은 바꿀 수 없습니다.

### 가장 안전한 방법: 버전 고정 + GitHub 서명 확인

[GitHub CLI](https://cli.github.com)(`gh`)가 있으면, 실행 전에 스크립트가 이 저장소에서 게시된 원본인지 GitHub 서명(release attestation)으로 확인할 수 있습니다.

```bash
TAG=v2.2.0
gh release download "$TAG" -R KNIPKCALB/molstudio-releases -p install.sh
gh release verify-asset "$TAG" install.sh -R KNIPKCALB/molstudio-releases && bash install.sh
```
Windows (PowerShell):
```powershell
$TAG = 'v2.2.0'
gh release download $TAG -R KNIPKCALB/molstudio-releases -p install.ps1
gh release verify-asset $TAG install.ps1 -R KNIPKCALB/molstudio-releases; if ($?) { & ([scriptblock]::Create((Get-Content -Raw install.ps1))) }
```
설치 파일만 직접 받을 때는 `shasum -a 256 -c SHA256SUMS`(Windows: `Get-FileHash <파일> -Algorithm SHA256`)로 확인하거나, `gh release verify-asset`을 씁니다.

### 서명에 대해

앱은 유료 개발자 인증서로 서명돼 있지 않습니다. Mac은 ad-hoc 서명만 있고 Apple Developer ID와 공증(notarization)이 없으며, Windows는 Authenticode 서명이 없습니다.
- 해시 검사와 GitHub 서명은 **파일이 게시된 원본인지**를 확인할 뿐입니다. Apple이나 Microsoft가 보증하는 **게시자 확인을 대신하지는 않습니다.**
- 설치 스크립트는 macOS 격리 속성(quarantine)을 지우지 않습니다. `curl`로 받은 파일에는 원래 이 속성이 붙지 않을 뿐입니다.
- 격리 속성을 직접 지우거나 Gatekeeper를 끄는 방법은 권하지 않습니다.

릴리스 페이지에서 파일을 직접 받으면 처음 한 번은 경고가 나옵니다.

- **Mac**: 시스템 설정 → 개인정보 보호 및 보안 → "그래도 열기"
- **Windows**: "추가 정보" → "실행"

## 제거

- **Mac**: `/Applications/MolStudio.app`을 휴지통으로 옮기면 됩니다. 설정까지 지우려면 `~/Library/Application Support/MolStudio` 폴더와 (1.x를 썼다면) `~/Library/Preferences/com.molstudio.MolStudio.plist`도 지웁니다.
- **Windows**: 설정 → 앱 → MolStudio → 제거. 설정까지 지우려면 `%APPDATA%\MolStudio` 폴더도 지웁니다.

## 버전

[Releases](../../releases)에서 버전별 파일, SHA-256 값, 빌드한 소스 커밋을 볼 수 있습니다.
