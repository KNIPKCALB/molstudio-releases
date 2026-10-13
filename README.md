# MolStudio 배포

MolStudio(분자 보기·편집, Gaussian/ORCA 입력 준비) 설치 파일을 받는 곳입니다. 소스 코드는 비공개이고, 이 저장소에는 설치 파일과 설치 스크립트만 올라갑니다.

## 설치

**Mac (Apple Silicon: M1 이후)**: 터미널에 붙여 넣기
```bash
curl -fsSL https://github.com/KNIPKCALB/molstudio-releases/releases/latest/download/install.sh | bash
```

**Windows**: PowerShell에 붙여 넣기 (시작 메뉴 → "PowerShell")
```powershell
irm https://github.com/KNIPKCALB/molstudio-releases/releases/latest/download/install.ps1 | iex
```

실행하면 무엇을 받을지 고르는 메뉴가 나옵니다.

| 실행한 곳 | 1번 | 2번 |
|---|---|---|
| Mac 터미널 | macOS 설치 (`/Applications`에 설치) | Windows 설치 파일만 받기 (`~/Downloads`) |
| Windows PowerShell | Windows 설치 (설치 마법사 실행) | Mac 디스크 이미지만 받기 (`Downloads`) |

처음 실행하면 **연구실 서버 아이디**를 묻습니다. ORCA 계산 번들의 서버 경로(`/home/<아이디>/…`)를 이 아이디로 만듭니다.

## 보안: 스크립트가 하는 일과 하지 않는 일

- **해시 고정**: 설치 스크립트는 릴리스를 게시할 때 만들어지고, 그 릴리스 설치 파일의 SHA-256 값이 스크립트 안에 들어 있습니다. 받은 파일의 해시가 다르면 실행하지 않고 지웁니다.
- **받는 곳**: 파일은 HTTPS(TLS 1.2 이상)로 이 저장소의 해당 버전 릴리스에서만 받습니다.
- **변경 불가 릴리스**: 이 저장소는 GitHub immutable releases를 켜 두어서, 게시된 릴리스의 파일과 태그는 바뀌지 않습니다. GitHub가 서명한 릴리스 증명(attestation)도 붙습니다.
- **권한**: `sudo`를 쓰지 않습니다. Mac은 `/Applications`(쓰기 권한이 없으면 `~/Applications`)에만 설치합니다. 기존 앱은 휴지통으로 옮긴 뒤 바꿉니다.
- **앱 검사**: Mac에서는 설치 전후에 앱 서명 봉인(`codesign --verify --deep --strict`)을 검사합니다. Apple Silicon이 아니면 설치를 멈춥니다.
- **잘린 스크립트**: 스크립트는 끝까지 받은 다음에야 실행됩니다. 다운로드가 중간에 끊겨도 반쪽짜리 스크립트가 실행되지 않습니다.
- **자동 검증**: 릴리스를 게시할 때마다 이 저장소의 GitHub Actions가 실제 Windows(PowerShell 5.1·7)와 macOS에서 다음을 확인합니다.
  - 설치 명령이 동작하는지
  - 설치한 앱이 실행되는지
  - **해시를 바꾼 스크립트가 설치를 거부하는지**

  결과는 [Actions](../../actions) 탭에 있습니다.

### 직접 확인하고 설치하기

명령을 바로 실행하는 대신, 먼저 받아서 읽어 보고 실행할 수 있습니다.

```bash
curl -fsSLO https://github.com/KNIPKCALB/molstudio-releases/releases/latest/download/install.sh
less install.sh          # 내용 확인
bash install.sh
```

[GitHub CLI](https://cli.github.com)가 있으면 받은 파일이 이 저장소 릴리스의 원본인지 GitHub 서명으로 확인할 수 있습니다.

```bash
gh release verify-asset v0.3.0 MolStudio-0.3.0-macos-arm64.dmg -R KNIPKCALB/molstudio-releases
shasum -a 256 -c SHA256SUMS      # Windows: Get-FileHash <파일> -Algorithm SHA256
```

### 서명에 대해

앱은 유료 개발자 인증서로 서명돼 있지 않습니다(Apple Developer ID와 Windows 코드 서명 모두 없음). 설치 스크립트로 설치하면 위의 해시 검사가 그 역할을 대신합니다. 릴리스 페이지에서 파일을 직접 받으면 처음 한 번은 경고가 나옵니다.

- **Mac**: 시스템 설정 → 개인정보 보호 및 보안 → "그래도 열기"
- **Windows**: "추가 정보" → "실행"

## 제거

- **Mac**: `/Applications/MolStudio.app`을 휴지통으로 옮기면 됩니다. 설정까지 지우려면 `~/Library/Preferences/com.molstudio.MolStudio.plist`도 지웁니다.
- **Windows**: 설정 → 앱 → MolStudio → 제거

## 버전

[Releases](../../releases)에서 버전별 파일, SHA-256 값, 빌드한 소스 커밋을 볼 수 있습니다.
